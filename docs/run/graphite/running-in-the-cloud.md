# Graphite: Running in the cloud

Run Graphite on hosted cloud providers. Running Graphite on a cloud provider is a quick way to get up and running. Using these images, you can be up and running with a Ceramic node in less than half an hour without compiling Graphite on your local machine.

## Cloud Providers

---

### DigialOcean

### Docker

The js-ceramic repo builds Docker images that run the Ceramic daemon and IPFS from the source code of the master branch. These images are tagged with "latest" and the git commit hash of the source code that the image was built from. You can view the image builds of [js-ceramic on DockerHub](https://hub.docker.com/r/ceramicnetwork/js-ceramic) and compatible builds of IPFS with the image builds of [ipfs-daemon on DockerHub](https://hub.docker.com/r/ceramicnetwork/ipfs-daemon).

```bash
docker pull ceramicnetwork/ipfs-daemon:latest

docker run -d -p 4011:4011 -p 5011:5011 -e CERAMIC_NETWORK=mainnet --name ipfs-daemon ceramicnetwork/ipfs-daemon:latest

# Get the IP address
docker inspect -f \
  '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' \
  ipfs-daemon

docker pull ceramicnetwork/js-ceramic:latest
```

#### Option A. Run Ceramic configured via JSON file
```
docker run -d \
  -p 7007:7007 \
  -v /path_for_daemon_config:/root/.ceramic/daemon.config.json \
  -v /path_for_ceramic_logs:/root/.ceramic/logs \
  -v /path_for_ceramic_statestore:/root/.ceramic/statestore \
  --name js-ceramic \
  ceramicnetwork/js-ceramic:latest
```
See: [Example daemon.config.json](#example-daemonconfigjson)

#### Option B. Run Ceramic configured via CLI flags
```
docker run -d \
  -p 7007:7007 \
  -v /path_for_ceramic_logs:/root/.ceramic/logs \
  -v /path_for_ceramic_statestore:/root/.ceramic/statestore \
  --name js-ceramic \
  ceramicnetwork/js-ceramic:latest \
  --network mainnet \
  --ethereum-rpc https://eg_infura_endpoint \
  --log-to-files \
  --ipfs-api http://ipfs_ip_address_from_above:5011
```

### AWS

The 3Box team has written a [Terraform module](https://github.com/ceramicnetwork/terraform-aws-ceramic) that configures Ceramic and IPFS in AWS ECS. Using this module is the fastest way to run Ceramic in the cloud. It runs Ceramic and IPFS out-of-process in containers using Docker images. This module currently requires some common AWS resources to be pre-configured as well as Cloudflare. You can see an example of the module in use here: [https://github.com/ceramicnetwork/terraform-aws-ceramic/blob/main/examples/ecs/main.tf](https://github.com/ceramicnetwork/terraform-aws-ceramic/blob/main/examples/ecs/main.tf)

We highly encourage others to create Terraform modules for other infrastructure providers and using different platforms.

### npm

The [js-ceramic CLI](https://www.npmjs.com/package/@ceramicnetwork/cli) and [ipfs-daemon](https://www.npmjs.com/package/@ceramicnetwork/ipfs-daemon) are available as public NPM modules. They are currently compatible with **Node.js version 14**. After a global installation, the daemons can be run from the command line.

```bash
npm install -g @ceramicnetwork/ipfs-daemon
export CERAMIC_NETWORK=mainnet # Set the Ceramic network for the IPFS node
ipfs-daemon

# In a new shell, configure Ceramic to use IPFS

npm install -g @ceramicnetwork/cli
```

#### Option A. Run Ceramic configured via JSON file
```bash
ceramic daemon
```

##### Example

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

#### Option B. Run Ceramic configured via CLI flags
```
ceramic daemon \
  --network mainnet \
  --ethereum-rpc https://eg_infura_endpoint \
  --log-to-files \
  --ipfs-api http://localhost:5011 # Remove this line to run IPFS in-process
```


## Using the image

## Next Steps

---

- [Next step 1]()
- [Next step 2]()
- [Next step 3]()

---

Was this page helpful? Y / N

[Edit this page]() on GitHub or [open an issue]()

