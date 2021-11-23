# Graphite: Configuration and Advance Usage

This guide documents environment variables, configuration and other advanced features in the Graphite Node.

## Configuration

---

The Ceramic daemon can be configured with a JSON file which is created on start and located at `$HOME/.ceramic/daemon.config.json`. Configuration options can be viewed in the [reference documentation for the DaemonConfig class](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_cli.daemonconfig-1.html).
You may also set these options with command line flags which can be viewed from the Ceramic CLI with `ceramic daemon --help`, but note that these CLI flags are deprecated.

The Ceramic daemon stores a configuration file in `~/.ceramic/daemon.config.json`. Note that by default all settings are commented. Here is an example configuration:

```json
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
```

## Connectivity

---

## Storage

---

### Running IPFS Out-of-process

The Ceramic daemon by default will start its own IPFS node internally, but it can also be configured to connect to an externally running IPFS node over HTTP. We refer to the latter as running IPFS "out-of-process". Running IPFS out-of-process is helpful for more controlled resource allocation, maintenance, debugging, and observability. This is highly recommended, especially if you are planning to be an infrastructure provider for other Ceramic applications.

When connecting to an out-of-process IPFS node, it is important that the IPFS node be configured with support for the dagJose plugin that Ceramic relies on. DagJose support is not included in IPFS by default, so we have provided the [@ceramicnetwork/ipfs-daemon package](https://www.npmjs.com/package/@ceramicnetwork/ipfs-daemon), which is a wrapper around js-ipfs configured with dagJose support specifically for use with Ceramic. Configuration options for the IPFS daemon can be viewed in the [ipfs-daemon README](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/ipfs-daemon) and in the [ipfs-daemon source code](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/ipfs-daemon/src/ipfs-daemon.ts).

The rest of this guide assumes you are running IPFS out-of-process.

## Environment variables

---

## Controlling a remote daemon

---

It is possible to use the CLI with a remote Ceramic node over HTTP, instead of a local node. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

```bash
ceramic config set ceramicHost 'https://yourceramicnode.com'
```

When using the CLI with a remote node, you have a few options:

- [Free community nodes](../../run/nodes/community-nodes.md)
- [Commercial node providers](../../run/nodes/node-providers.md)
- [Host your own node](../../run/nodes/nodes.md)

## Log level control

---

Ceramic has a debug mode that you can enable using the `--debug` flag. This will allow you to see all logs printed to your console, including debug logs, API requests, events, and errors.

For observability, it is best to have these logs written to files to debug any issues and to generate metrics. Logging to files can be enabled with the `--log-to-files` flag. The default location for logs is `~/.ceramic/logs` but this path can be configured with the `--log-directory` flag. Without debug mode enabled you will still get critical logs and metrics written to files.

Request and event logs are written in [logfmt](https://brandur.org/logfmt). This makes them easy to import into [Grafana](https://grafana.com/) dashboards using a log scraping agent like [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) and a log aggregator like [Loki](https://grafana.com/docs/loki/latest/), which can be used as a data source for Grafana. An example of such a setup can be found here: [https://github.com/3box/ceramic-stats](https://github.com/3box/ceramic-stats)

## Developer accounts

---

By default, the CLI is authenticated using the [Key DID Provider](../../authentication/key-did/provider.md). The seed for this DID is stored in `~/.ceramic/config.json`. If this file is not present on startup a new DID will be randomly generated. It's currently not possible to use the Ceramic CLI with other DID methods.

