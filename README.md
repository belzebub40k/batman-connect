# batman-connect

This script makes it easy to connect regular Linux PCs to a [batman-adv](https://www.open-mesh.org/projects/batman-adv/wiki) mesh-network via [fastd](https://projects.universe-factory.net/projects/fastd/wiki).

## General

The configuration is done in seperate file named `.batman-connect` which is then sourced by the script.

An example configuration for [Freifunk Mainz](https://freifunk-mainz.de) and [Freifunk Wiesbaden](https://www.wiesbaden.freifunk.net) are included this repository. Please keep in mind that you have access to elevated rights (e.g. root) in order to run the script. By default the configuration needs to be placed in the same directory as the script.

## Prerequisites

You need to install [batman-adv](https://www.open-mesh.org/projects/batman-adv/wiki) and [fastd](https://projects.universe-factory.net/projects/fastd/wiki) first to establish a connection with the fastd-server. 

### Ubuntu
- Fast and Secure Tunneling Daemon (fastd):
```
apt install apt-transport-https
apt-key adv --keyserver pgp.mit.edu --recv-keys 0x16EF3F64CB201D9C
echo "deb https://repo.universe-factory.net/debian/ sid main" | sudo tee -a /etc/apt/sources.list
apt update
apt install fastd
```

- B.A.T.M.A.N. Advanced:
```
apt update
apt install batctl
```

## Usage
### Generate a private and public key with `fastd`:

`fastd --generate-key`

Example output:
```
root@ubuntu:~/batman-connect$ fastd --generate-key
2016-11-19 15:12:02 +0100 --- Info: Reading 32 bytes from /dev/random...
Secret: 58b125e2f2c147eb3304714e662e5a58582ac99536cd58f33baca315ce702b54
Public: a7bb4b5988153a2d39d321f49d155b9c63756d5860d24321cc1dab5502072cec
```
As you can see you will get 2 strings where the private one will go into your `.batman-connect` configfile and the publickey has to be added in our case to a Github repository.

### Start the script:

`batman-connect [connection_prefix]`

## Configfile

You can define as many connections in `.batman-connect` as you like. Every connection needs to consist of the following variables prefixed by the connection name followed by an _ (underscore). To prevent unusual errors do not use other characters than `[0-9a-zA-Z]`.

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

One connection can have multiple peers. To define them multiply the variables `prefix_peer_x_(host|port|pub)` by the number you defined in `prefix_peers` and replace `x` with ascending numbers starting at `1`.

As `.batman-connect` is a bash-script you can use additional variables as shown in the example file to prevent redundancies.