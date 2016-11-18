# batman-connect

This script makes it easy to connect regular Linux PCs to a [batman-adv](https://www.open-mesh.org/projects/batman-adv/wiki) mesh-network via [fastd](https://projects.universe-factory.net/projects/fastd/wiki).

## General

The configuration is done in seperate file named `~/.batman-connect` which is then sourced by the script.

An example configuration for Freifunk Mainz and Freifunk Wiesbaden are included this repository. Please keep in mind that the configuration need to be placed in /root if you start the script via sudo.

## Usage

`batman-connect [connection_prefix]`

## Configfile

You can define as many connections in `.batman-connect` as you like. Every connection needs to consist of the following variables prefixed by the connection name follwed by an _ (underscore). To prevent unusual errors do not use other characters than `[0-9a-zA-Z]`.

| Variable | Description |
| --- | --- |
| prefix_secret | fastd private key *(secret)* |
| prefix_method | Encryption method used for fastd |
| prefix_mtu | MTU used for fastd |
| prefix_peer_limit | amount of fastd connection to initialize |
| prefix_peers | count of defined peers |
| prefix_peer_1_host | hostname of 1. fastd peer |
| prefix_peer_1_port | port of of 1. fastd peer |
| prefix_peer_1_pub | public key of 1. fastd peer |

One connection can have multiple peers. To define them multiply the variables `prefix_peer_x_(host|port|pub)` by the number you defined in `prefix_peers` and replace `x` with ascendending numbers starting at `1`.

As `.batman-connect` is a bash-script you can use additional variables as shown in the example file to prevent redundancies.
