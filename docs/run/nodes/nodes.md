# **Running Ceramic in production**

---

This guide provides complete instructions and various tools for launching a well-connected, production-ready Ceramic node.

## **Who should run a Ceramic node?**

---

To run your application on `mainnet` you'll need to run your own production-ready node or to use a community provider like [hirenodes](https://hirenodes.io/).

## **Things to know**

---

**Ceramic networks** – There are currently three Ceramic networks: `mainnet`, `testnet-clay`, and `dev-unstable`. Learn more about each network [here](https://developers.ceramic.network/learn/networks/). By default, Ceramic will connect to `testnet-clay` and a [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service) running on Gnosis. When you are ready to get on Ceramic `mainnet`, check out [this guide](https://composedb.js.org/docs/0.4.x/guides/composedb-server/access-mainnet) to get access to our `mainnet` anchor service.

**Running IPFS** – Ceramic relies on a system called [IPFS](https://docs.ipfs.io/) to connect to and share data in Ceramic networks. IPFS runs as a separate process from the Ceramic node itself, with each Ceramic node connected to a dedicated IPFS node over HTTP. The Ceramic Daemon can launch an IPFS process automatically (referred to as running IPFS in "bundled" mode in the Ceramic config file), which is designed for testing and local development only. For production deployments you should run your own IPFS process manually and point your Ceramic node at it (referred to as running ipfs in "remote" mode in the Ceramic config file). This allows for more configuration options for your IPFS node allowing for more controlled resource allocation, as well as improved maintenance, debugging and observability. Note that Ceramic only supports `go-ipfs` version 0.12 or later.

The rest of this guide assumes you are running Ceramic with IPFS in "remote" mode.

**Process management, restarts and data persistence** – Ceramic and IPFS will not automatically restart if they crash. You should configure your own restart mechanism, and you must ensure data persistence between restarts.

## **Required steps**

---

Below are the steps required for running a Ceramic node in production. This guide will teach you how to:

1. [Install and run the Ceramic daemon](#running-the-daemon)
2. [Configure data persistence](#data-persistence)
3. [Get connected to the network](#get-connected-to-the-network)
4. [Get observability data from your node (optional)](#observability)

## **Quick start**

---

### [**Run Ceramic on AWS ECS with Terraform →**](https://github.com/ceramicnetwork/terraform-aws-ceramic)

The 3Box Labs team has written a [Terraform module](https://github.com/ceramicnetwork/terraform-aws-ceramic) that configures Ceramic and IPFS in AWS ECS using Fargate. Using this module is a fast and reliable way to run Ceramic in the cloud because it is set up for data persistence and auto-restarts. The module currently requires some common AWS resources to be pre-configured as well as Cloudflare. See an [example of the module in use](https://github.com/ceramicnetwork/terraform-aws-ceramic/blob/main/examples/ecs/main.tf).

> We highly encourage the community to create Terraform modules or other templates for different infrastructure providers to further decentralize the Ceramic network.

## **Running the daemon**

---

The [js-ceramic](https://github.com/ceramicnetwork/js-ceramic) node is run as a daemon using Node.js or Docker.

By default, the Ceramic daemon runs bundled with a [go-ipfs](https://github.com/ipfs/go-ipfs) node and connects to the Clay testnet and Gnosis [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service). In production, you should change these defaults to secure your data and accommodate your infrastructure setup.

The Ceramic daemon can be configured with a JSON file which is created on start and located at `$HOME/.ceramic/daemon.config.json` by default (you can also point to a custom location for the config file using the `--config` flag when starting the Ceramic Daemon). See [example daemon.config.json](#example-daemonconfigjson) below. Configuration options can be viewed in the [reference documentation for the DaemonConfig class](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_cli.daemonconfig-1.html).

### **Run with Docker containers**

Docker images to run Ceramic and IPFS are built from the source code of the [js-ceramic](https://github.com/ceramicnetwork/js-ceramic) and [go-ipfs-daemon](https://github.com/ceramicnetwork/go-ipfs-daemon) repositories respectively. Images built from the main branches are tagged with `latest` and the git commit hash from which the image was built. You can view the image builds of [js-ceramic on DockerHub](https://hub.docker.com/r/ceramicnetwork/js-ceramic). The Docker image for `go-ipfs-daemon` pre-configures IPFS with plugins that make it easy to run on cloud infrastructure. You can view the image builds for [go-ipfs-daemon on DockerHub](https://hub.docker.com/r/ceramicnetwork/go-ipfs-daemon).

### **Run outside of containers**

If you would like to run Ceramic and IPFS outside of containers or on bare metal, start by installing [go-ipfs](https://github.com/ipfs/go-ipfs) (**version 0.12 or later**). Depending on your infrastructure setup you may consider building `go-ipfs` with the [healthcheck plugin](https://github.com/ceramicnetwork/go-ipfs-healthcheck) and [S3 datastore plugin](https://github.com/3box/go-ds-s3). Once IPFS is installed, configure it to use pubsub, which Ceramic relies on for message passing. This can be done by running the IPFS daemon with the `--enable-pubsub-experiment` flag or modifying the configuration by running `ipfs config --json Pubsub.Enabled true` (learn more in the [IPFS docs](https://github.com/ipfs/go-ipfs/blob/master/docs/experimental-features.md#ipfs-pubsub)). Next you can run IPFS and install the Ceramic daemon with the [js-ceramic CLI](https://www.npmjs.com/package/@ceramicnetwork/cli), which is available as a public NPM module. It is currently compatible with **Node.js versions 14 and 16.**

## **Data Persistence**

To run a Ceramic node in production, it is critical to persist the [Ceramic state store](https://developers.ceramic.network/run/nodes/nodes/#ceramic-state-store) and the [IPFS datastore](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#datastorespec). The form of storage you choose should also be configured for disaster recovery with data redundancy, and some form of snapshotting and/or backups.

**Loss of this data can result in permanent loss of Ceramic streams and will cause your node to be in a corrupt state.**

The Ceramic state store and IPFS datastore are stored on your machine's filesystem by default. The Ceramic state store defaults to `$HOME/.ceramic/statestore`. The IPFS datastore defaults to `ipfs/blocks` located wherever you run IPFS.

The fastest way to ensure data persistence is by mounting a persistent volume to your instances and configuring the Ceramic and IPFS nodes to write to the mount location. The mounted volume should be configured such that the data persists if the instance shuts down.

You can also use AWS S3 for data storage which is supported for both Ceramic and IPFS. Examples of the configuration for both storage options are listed below.

#### IPFS Datastore

The IPFS datastore stores the raw IPFS blocks that make up Ceramic streams. To prevent data corruption, use environment variables written to your profile file, or otherwise injected into your environment on start so that the datastore location does not change between reboots.

Note: Switching between data storage locations is an advanced feature and should be avoided. Depending on the sharding implementation you may need to do a data migration first. See [https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#datastorespec](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#datastorespec) for more information.

#### Ceramic State Store

The Ceramic state store holds state for pinned streams and the acts as a cache for the Ceramic streams that your node creates or loads. To ensure that the data you create with your Ceramic node does not get lost you must pin streams you care about and you must ensure that the state store does not get deleted.


## **Examples**

---

### Example with Docker containers

```bash
docker pull ceramicnetwork/go-ipfs-daemon:latest

# Use this snippet to keep the datastore in the volume
docker run \
  -p 5001:5001 \ # API port
  -p 8011:8011 \ # Healthcheck port
  -v /path_on_volume_for_ipfs_repo:/data/ipfs \
  --name ipfs \
  go-ipfs-daemon

# Use this snippet to keep the datastore in S3
docker run \
  -p 5001:5001 \ # API port
  -p 8011:8011 \ # Healthcheck port
  -v /path_on_volume_for_ipfs_repo:/data/ipfs \
  -e IPFS_ENABLE_S3=true \
  -e IPFS_S3_REGION=region \
  -e IPFS_S3_BUCKET_NAME=bucket_name \
  -e IPFS_S3_ROOT_DIRECTORY=root_directory \
  -e IPFS_S3_ACCESS_KEY_ID=aws_access_key_id \
  -e IPFS_S3_SECRET_ACCESS_KEY=aws_secret_access_key \
  -e IPFS_S3_KEY_TRANSFORM=next-to-last/2 \ # Sharding method
  --name ipfs \
  go-ipfs-daemon

# Get the IP address
docker inspect -f \
  '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' \
  ipfs
```

Before running the Ceramic daemon, configure it to use IPFS in "remote" mode. See [example daemon.config.json](#example-daemonconfigjson) below.

```bash
docker pull ceramicnetwork/js-ceramic:latest

# Use this snippet to keep the statestore in a volume
docker run -d \
  -p 7007:7007 \
  -v /path_on_volume_for_daemon_config:/root/.ceramic/daemon.config.json \
  -v /path_on_volume_for_ceramic_logs:/root/.ceramic/logs \
  -v /path_on_volume_for_ceramic_statestore:/root/.ceramic/statestore \
  -e NODE_ENV=production \
  --name js-ceramic \
  ceramicnetwork/js-ceramic:latest

# Use this snippet to keep the statestore in S3
docker run -d \
  -p 7007:7007 \
  -v /path_for_daemon_config:/root/.ceramic/daemon.config.json \
  -v /path_for_ceramic_logs:/root/.ceramic/logs \
  -e NODE_ENV=production \
  -e AWS_ACCESS_KEY_ID=s3_access_key_id \
  -e AWS_SECRET_ACCESS_KEY=s3_secret_access_key \
  --name js-ceramic \
  ceramicnetwork/js-ceramic:latest
```

### Example without containers

After installation, both daemons can be run from the command line

```bash
ipfs init
ipfs daemon
```

Before running the Ceramic daemon, configure it to use IPFS in "remote" mode. See [example daemon.config.json](#example-daemonconfigjson) below.

```bash
ceramic daemon
```

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
        "mode": "remote",
        "host": "http://ipfs_ip_address:5001"
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
        "mode": "s3",
        "s3-bucket": "bucket_name"
    }
}
```

To use volume storage for the statestore instead of S3

```json
"state-store": {
    "mode": "fs",
    "local-directory": "/path_for_ceramic_statestore", // Defaults to $HOME/.ceramic/statestore
}
```

### Example AWS S3 Policies

IPFS AWS S3 policy for the access key

```json
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

!!! info ""

    The [S3 datastore](https://github.com/3box/go-ds-s3) is not available out-of-the-box in vanilla `go-ipfs`. In order to use it with minimal configuration, use the 3Box Labs [go-ipfs-daemon](https://github.com/ceramicnetwork/go-ipfs-daemon).

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

## **Get connected to the network**

---

### **Connecting to Ceramic**

The Ceramic daemon serves an HTTP API that clients use to interact with your Ceramic node. The default API port is `7007`. Make sure this port is available to all clients you plan to use for your application.

!!! warning ""

    Healthchecks can be run against the API endpoint `/api/v0/node/healthcheck`.

### **Staying connected to IPFS**

Ceramic nodes rely on IPFS for networking. IPFS nodes connect to each other using a Libp2p module called "switch" (aka "swarm"). This module operates over a websocket, on port `4001` by default, as well as port `4011`. The websocket port must be accessible to the internet so your Ceramic node can be connected to the network, i.e. not blocked by a firewall. In addition, the port must match the swarm address announced by IPFS.

!!! warning ""

    Healthchecks can be run against the `HEALTHCHECK_PORT` (port `8011` by default) when `HEALTHCHECK_ENABLED` is `true`.

Additionally, when running IPFS the IPFS API port must be accessible by the Ceramic node. The default API port is `5001`. The IPFS node address will then be passed to Ceramic with the `ipfs.host` option in the Ceramic daemon config file.

### **Connect to the mainnnet anchor service**

For nodes that wish to connect to Ceramic mainnet, the node's IP address will have to be added to the allowlist for the Ceramic Anchor Service node operated by 3BoxLabs. Once you have fully configured your Ceramic node with this guide and have a way to persist its configuration and state, open an issue in the [Ceramic Anchor Allowlist Repo](https://github.com/3box/ceramic-anchor-allowlist) with the public, static *egress* IP address for your Ceramic node, and a brief description of the data persistence setup for the multiaddress, Ceramic State Store, and IPFS Repo. Once your issue is closed, you will be connected to the Ceramic network and the [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service).

Nodes that wish to connect to other Ceramic networks, such as the clay testnet, do not need to do anything special to gain access to the Ceramic Anchor Service.  The Anchor Service for the `testnet-clay` network is open to everyone and does not have an IP allowlist like the mainnet service does.

!!! warning ""

    Mainnet nodes will not run immediately after start up until your pull request is reviewed and your IP address is added to the allow list for the 3Box Labs hosted anchor service.

## **Observability**

---

Ceramic has a debug mode that you can enable using the `--debug` flag. This will allow you to see all logs printed to your console, including debug logs, API requests, events, and errors.

For observability, it is best to have these logs written to files to debug any issues and to generate metrics. Logging to files can be enabled with the `logger.log-to-files` config file option. The default location for logs is `~/.ceramic/logs` but this path can be configured with the `logger.log-directory` config file option. Even without debug mode enabled you will still get critical logs and metrics written to files.

Request and event logs are written in [logfmt](https://brandur.org/logfmt). This makes them easy to import into [Grafana](https://grafana.com/) dashboards using a log scraping agent like [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) and a log aggregator like [Loki](https://grafana.com/docs/loki/latest/), which can be used as a data source for Grafana. An example of such a setup can be found [here](https://github.com/3box/ceramic-stats).

## **Next steps**

---

Congratulations! You have now set up a well-connected Ceramic node in the cloud which can receive HTTP requests from the local environment, the [JS HTTP Client](../../build/javascript/installation.md#js-http-client), or to simply serve as another node to replicate and pin streams. Please report any bugs as issues on the [JS Ceramic GitHub](https://github.com/ceramicnetwork/js-ceramic).
