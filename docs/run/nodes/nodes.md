# **Launching Ceramic in the cloud**

---

This guide provide complete instructions and various tools for launching a highly-available, well-connected Ceramic node in the cloud. 

## **Who should run Ceramic in the cloud?**

---

At this time, any application that wishes to deploy to mainnet needs to run their own node. Additionally, developers building on testnet may wish to run their own node so they don't need to rely on [community-hosted nodes](https://developers.ceramic.network/run/nodes/community-nodes/) which may be unstable and/or wipe data from time to time.

## **Things to know**

---

**Ceramic networks** – There are currently three Ceramic networks: `mainnet`, `testnet-clay`, and `dev-unstable`. Learn more about each network [here](https://developers.ceramic.network/learn/networks/). By default, Ceramic will connect to the Clay testnet and a [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service) running on Ethereum Ropsten. When you are ready to get on Ceramic mainnet, reach out on the [Ceramic Discord →](https://chat.ceramic.network). For now, mainnet is limited to early partners and we will need to test your connectivity before granting you access.

**Running IPFS** – Ceramic relies on the IPFS interplanetary file system. By default, Ceramic includes an internal IPFS instance. You can run Ceramic and IPFS in this single process or in separate processes that communicate over HTTP. For production, it is recommended to run as separate processes for better scalability, resource utilization, control, debugging, and observability.

**DagJose codec** – When using an IPFS node running separately from Ceramic, this IPFS node needs to support the `dagJose` codec, which is not included in IPFS by default. You should use the [@ceramicnetwork/ipfs-daemon](https://www.npmjs.com/package/@ceramicnetwork/ipfs-daemon), package which includes `js-ipfs` configured with dagJose support. Config options for the IPFS daemon can be viewed in the [ipfs-daemon README](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/ipfs-daemon) and in the [ipfs-daemon source code](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/ipfs-daemon/src/ipfs-daemon.ts).

**Restarts and maintaining data persistence and connectivity** – Ceramic and IPFS will not automatically restart if they crash. You should configure your own restart mechanism and you must ensure data persistence between restarts. If the IPFS multiaddress changes for any reason (your node goes down or restarts without pulling in an existing config file), your node will regenerate this file upon restarting with a new address and all other nodes on the network will lose connection to you. 

## **Quick start**

---

### [**Run Ceramic on AWS ECS with Terraform →**](https://github.com/ceramicnetwork/terraform-aws-ceramic)

The 3Box Labs team has written a Terraform module that configures Ceramic and IPFS in AWS ECS. This module is currently the fastest way to run Ceramic in the cloud. It runs Ceramic and IPFS separately in containers using Docker images. This module currently requires some common AWS resources to be pre-configured as well as Cloudflare. You can see an [example of the module in use](https://github.com/ceramicnetwork/terraform-aws-ceramic/blob/main/examples/ecs/main.tf).

> We highly encourage others to create Terraform modules for other infrastructure providers and using different platforms.


## **Required steps**

---

Below are the steps required for running a Ceramic node in the cloud. This guide will teach you how to:

1. [Install and run the Ceramic daemon]()
2. [Configure data persistence]()
3. [Stay connected to the network]()
4. [Get observability data from your node]()



## **Running the daemon**

---

The JS Ceramic node is run as a daemon using Docker or Node.js, It can be configured with a JSON file which is created on start and located at `$HOME/.ceramic/daemon.config.json`. Configuration options can be viewed in the [reference documentation for the DaemonConfig class](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_cli.daemonconfig-1.html).


### **Install from DockerHub**

The JS Ceramic repo builds Docker images that run the Ceramic daemon and IPFS from the source code of the master branch. These images are tagged with `latest` and the git commit hash of the source code that the image was built from. You can view the image builds of [js-ceramic on DockerHub](https://hub.docker.com/r/ceramicnetwork/js-ceramic) and compatible builds of [ipfs-daemon on DockerHub](https://hub.docker.com/r/ceramicnetwork/ipfs-daemon).

```bash
docker pull ceramicnetwork/ipfs-daemon:latest

docker run -d -p 4011:4011 -p 5011:5011 -e CERAMIC_NETWORK=mainnet --name ipfs-daemon ceramicnetwork/ipfs-daemon:latest

# Get the IP address
docker inspect -f \
  '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' \
  ipfs-daemon

docker pull ceramicnetwork/js-ceramic:latest
```

Next launch the daemons:

```
docker run -d \
-p 7007:7007 \
-v /path_for_daemon_config:/root/.ceramic/daemon.config.json \
-v /path_for_ceramic_logs:/root/.ceramic/logs \
-v /path_for_ceramic_statestore:/root/.ceramic/statestore \
--name js-ceramic \
ceramicnetwork/js-ceramic:latest
```

Then, configure your setup using a JSON file. See [example daemon.config.json](#example-daemonconfigjson) below.


### **Install from npm**

The [JS Ceramic CLI](https://www.npmjs.com/package/@ceramicnetwork/cli) and [ipfs-daemon](https://www.npmjs.com/package/@ceramicnetwork/ipfs-daemon) are available as npm modules. They are currently compatible with **Node.js version 14**. After a global installation, the daemons can be run from the command line:

```bash
npm install -g @ceramicnetwork/ipfs-daemon
export CERAMIC_NETWORK=mainnet # Set the Ceramic network for the IPFS node
ipfs-daemon

# In a new shell, configure Ceramic to use IPFS

npm install -g @ceramicnetwork/cli
```

Next launch the daemons:

```bash
ceramic daemon
```
Then, configure your setup using a JSON file. See [example daemon.config.json](#example-daemonconfigjson) below.



### **Example daemon.config.json**

```json
{
    "anchor": {
        "ethereum-rpc-url": "https://eg_infura_endpoint" // Replace with an Ethereum RPC endpoint to avoid rate limiting
    },
    "http-api": {
        "cors-allowed-origins": [
            ".*"
        ]
    },
    "ipfs": {
        "mode": "remote", // Use "remote" for IPFS out-of-process or "bundled" for in-process
        "host": "http://ipfs_ip_address:5011"
    },
    "logger": {
        "log-level": 2, // 0 is most verbose
        "log-to-files": true
    },
    "network": {
        "name": "mainnet", // Connect to mainnet, testnet-clay, or dev-unstable
    },
    "node": {},
    "state-store": {
        "mode": "fs",
        "local-directory": "/path_for_ceramic_statestore" // Defaults to $HOME/.ceramic/statestore
    }
}
```

## **Configure data persistence**

---

When running a Ceramic node in production, it is critical to persist the IPFS multiaddress for network connectivity, the IPFS repo, and the Ceramic state store for data persistence since there is no guarantee another node is also keeping your data available. **Loss of this data can result in permanent loss of Ceramic streams and will cause your node to be in a corrupt state.**

!!! warning ""

    Data persistence is the most critical step to properly running a Ceramic node. The form of storage you choose should be configured for disaster recovery with data redundancy, some form of snapshotting, and/or backups.

The IPFS repo and the Ceramic state store are stored on your machine's filesystem by default. The IPFS repo defaults to a directory called `ipfs` located wherever you run the `ipfs-daemon` process (or where you run the`ceramic daemon` process if keeping Ceramic and IPFS bundled, which isn't recommended). The Ceramic state store defaults to `~/.ceramic/statestore`.

The fastest way to ensure data persistence is by mounting a persistent volume to your instances and configuring the Ceramic and IPFS nodes to write to the mount location. The mounted volume should be configured such that the data persists if the instance shuts down.

You can also use AWS S3 for data storage which is supported for both Ceramic and IPFS. Below are examples of the configuration for both storage options.


### **Persisting IPFS data**

The IPFS repo holds configuration settings and all the raw IPFS data for the Ceramic streams used by your node. It is essential to keep the file names `config` generated by IPFS so that your node can stay connected to the Ceramic network. This file is located at the root of the IPFS repo directory.

!!! warning ""

    The IPFS config file holds your node's private key which is used to generate your node's peerId and multiaddress. If this file is deleted it will be re-created on start with a different key, peerId and multiaddress. This will result in your node and the rest of the network not being able to connect to each other.

!!! warning ""

    Environment variables should be written to your profile file, or otherwise injected into your environment on start so that they persist between reboots.


=== "Volume storage"

    ```bash
    # Environment variable to use a mounted volume for IPFS persistence
    export IPFS_PATH="/mnt_volume_path_for_ipfs"
    ```

=== "AWS S3"

    ```bash
    # Environment variables to use S3 for IPFS persistence
    export IPFS_S3_REPO_ENABLED="true"
    export IPFS_PATH="directory_for_the_bucket"
    export AWS_BUCKET_NAME="bucket_name"
    export AWS_ACCESS_KEY_ID="aws_access_key_id"
    export AWS_SECRET_ACCESS_KEY="aws_secret_access_key"
    export IPFS_BACKEND_ROOT="s3"
    export IPFS_BACKEND_BLOCKS="s3"
    export IPFS_BACKEND_KEYS="s3"
    export IPFS_BACKEND_PINS="s3"
    export IPFS_BACKEND_DATASTORE="s3"
    ```

    ```json
    // IPFS AWS S3 policy for the access key
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Action": [
            "s3:GetObject",
            "s3:ListBucket",
            "s3:PutObject",
            "s3:DeleteObject"
        ],
        "Effect": "Allow",
        "Resource": ["ipfs_bucket_arn", "ipfs_bucket_arn/*"]
        }
    ]
    }
    ```

### **Persisting the Ceramic State Store**

The Ceramic state store holds commits for pinned streams and the acts as a cache for the Ceramic streams that your node creates or loads. To ensure that the data you create with your Ceramic node does not get lost you must pin streams you care about and you must ensure that the state store does not get deleted.

=== "Volume storage"

    Using `daemon.config.json`

    ```json
        "state-store": {
            "mode": "fs",
            "local-directory": "/mnt_volume_path_for_statestore",
        },
    ```

=== "AWS S3"

    Using `daemon.config.json`

    ```json
        "state-store": {
            "mode": "s3",
            "s3-bucket": "bucket_name"
        },
    ```

    Ceramic state store AWS S3 policy for the access key

    ```json
    {
     "Version": "2012-10-17",
     "Statement": [
        {
        "Action": [
            "s3:ListBucket",
            "s3:GetObject",
            "s3:PutObject",
            "s3:DeleteObject"
        ],
        "Effect": "Allow",
        "Resource": ["state_store_bucket_arn", "state_store_bucket_arn/*"]
        }
    ]
    }
    ```

## **Stay connected to the network**

---

### **Staying connected to Ceramic**

The Ceramic daemon serves an HTTP API that clients use to interact with your Ceramic node. The default API port is `7007`. Make sure this port is available to all clients you plan to use for your application.

!!! warning ""

    Healthchecks can be run against the API endpoint `/api/v0/node/healthcheck`.

### **Staying connected to IPFS**

Ceramic nodes rely on IPFS for networking. IPFS nodes connect to each other using a Libp2p module called "switch" (aka "swarm"). This module operates over a websocket, on port `4011` by default. The websocket port must be accessible to the internet so your Ceramic node can be connected to the network.

!!! warning ""

    We recommend using SSL for a secure websocket (port `4012` by default).

!!! warning ""

    Healthchecks can be run against the `HEALTHCHECK_PORT` (port `8011` by default) when `HEALTHCHECK_ENABLED` is `true`.

Additionally when running IPFS separately from Ceramic, the IPFS API port must be accessible by the Ceramic node. The default API port is `5011`. The IPFS node address will then be passed to Ceramic with a CLI flag `--ipfs-api <ipfs_api_url>`.

### **Join the peerlist**

The `ipfs-daemon` package used by Ceramic has IPFS's node discovery mechanism, the Libp2p DHT, turned off by default. With node discovery turned off, we must manually create a connected network of peers by sharing known addresses and dialing them explicitly. The `ipfs-daemon` package handles this logic and requires that every node that wants to be in the network be in a "peerlist" which is maintained [here](https://github.com/ceramicnetwork/peerlist).

Once you have fully configured your Ceramic node with this guide and have a way to persist its configuration and state, submit a pull request to the [Ceramic peerlist](https://github.com/ceramicnetwork/peerlist) with the persistent multiaddress of your IPFS node, the IP address for your Ceramic node, and a brief description of the data persistence setup for the multiaddress, Ceramic State Store, and IPFS Repo. When a pull request is submitted, it triggers a connectivity test to ensure the node can successfully connect to the network. If this fails, the 3Box Labs team will reach out to you directly to triage the issue. Make sure there are no firewalls blocking your instance and that your port is properly exposed. Once your pull request is merged, you will be connected to the Ceramic network and the [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service).
    
!!! warning ""

    Mainnet nodes will not run immediately after start up until your IP address is added to the allow list for the anchor service hosted by 3Box Labs and your PR to the peerlist is merged.

Once you are on the peerlist, you should monitor your IPFS node and alert our team on Discord in the case of any planned or unexpected downtime. Please make your best effort to come back online within 24 hours. If we can not connect to your IPFS node for over 24 hours, we will remove it from the peerlist and you can resubmit your multiaddress in a new PR once your node becomes stable again. If the connectivity test in your PR to the peerlist fails and it is due to a node other than your own, we will update the peerlist and re-run the tests for you.

## **Observability**

---

Ceramic has a debug mode that you can enable using the `--debug` flag. This will allow you to see all logs printed to your console, including debug logs, API requests, events, and errors.

For observability, it is best to have these logs written to files to debug any issues and to generate metrics. Logging to files can be enabled with the `--log-to-files` flag. The default location for logs is `~/.ceramic/logs` but this path can be configured with the `--log-directory` flag. Without debug mode enabled you will still get critical logs and metrics written to files.

Request and event logs are written in [logfmt](https://brandur.org/logfmt). This makes them easy to import into [Grafana](https://grafana.com/) dashboards using a log scraping agent like [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) and a log aggregator like [Loki](https://grafana.com/docs/loki/latest/), which can be used as a data source for Grafana. An example of such a setup can be found [here](https://github.com/3box/ceramic-stats).

## **Next steps**

---

Congratulations! You have now set up a highly-available, well-connected Ceramic node in the cloud which can receive HTTP requests from the local environment, the [JS HTTP Client](../../build/javascript/installation.md#js-http-client), or to simply serve as another node to replicate and pin streams. Please report any bugs as issues on the [JS Ceramic GitHub](https://github.com/ceramicnetwork/js-ceramic).
