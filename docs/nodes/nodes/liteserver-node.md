# Liteserver Node

> Read about [Full Node](/nodes/nodes/full-node.md) before this article

When an endpoint is activated in a full node, the node assumes the role of a **Liteserver**. This node type can field and respond to requests from Lite Clients, allowing for seamless interaction with the TON Blockchain.

## Hardware requirements

Compared to a [validator](/nodes/nodes/full-node.md#hardware-requirements), a liteserver mode requires less resources. However, it is still recommended to use a powerful machine to run a liteserver.

- at least 16 cores CPU
- at least 128 GB RAM
- at least 1TB GB NVMe SSD _OR_ Provisioned 64+k IOPS storage
- 1 Gbit/s network connectivity
- 16 TB/month traffic on peak load
- public IP address (_fixed IP address_)

### Recommended Providers

Feel free to use cloud providers listed in the [Recommended Providers](/nodes/nodes/full-node.md#recommended-providers) section.

Hetzner and OVH are forbidden to run a validator, but you can use them to run a liteserver:

- __Hetzner__: EX101, AX102
- __OVH__: RISE-4

## Installation of liteserver

If you don't have mytonctrl, install it with `-m liteserver` flag:

Ubuntu:
  ```bash
  wget https://raw.githubusercontent.com/ton-blockchain/mytonctrl/master/scripts/install.sh
  sudo bash ./install.sh -m liteserver
  ```

Debian: 
  ```bash
  wget https://raw.githubusercontent.com/ton-blockchain/mytonctrl/master/scripts/install.sh
  su root -c 'bash ./install.sh -m liteserver'
  ```

* `-d` - **mytonctrl** will download a [dump](https://dump.ton.org/) of the latest blockchain state.
  This will reduce synchronization time by several times.
* `-c <path>` - If you want to use not public liteservers for synchronization. _(not required)_
* `-i` - Ignore minimum requirements, use it only if you want to check compilation process without real node usage.
* `-m` - Mode, can be `validator` or `liteserver`.

**To use testnet**, `-c` flag should be provided with `https://ton.org/testnet-global.config.json` value.

Default `-c` flag value is `https://ton-blockchain.github.io/global.config.json`, which is default mainnet config.

If you already have mytonctrl installed, run:

```bash
user@system:~# mytonctrl
MyTonCtrl> enable_mode liteserver
```

## Check the firewall settings

First, verify the Liteserver port specified in your `/var/ton-work/db/config.json` file. This port changes with each new installation of `MyTonCtrl`. It is located in the `port` field:

```json
{
  ...
  "liteservers": [
    {
      "ip": 1605600994,
      "port": LITESERVER_PORT
      ...
    }
  ]
}
```

If you are using a cloud provider, you need to open this port in the firewall settings. For example, if you are using AWS, you need to open the port in the [security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html).

Below is an example of opening a port in the bare metal server firewall.

### Opening a port in the firewall

We will use the `ufw` utility ([cheatsheet](https://www.cyberciti.biz/faq/ufw-allow-incoming-ssh-connections-from-a-specific-ip-address-subnet-on-ubuntu-debian/)). You can use the one you prefer.

1. Install `ufw` if it is not installed:

```bash
sudo apt update
sudo apt install ufw
```

2. Allow ssh connections:

```bash
sudo ufw allow ssh
```

3. Allow the port specified in the `config.json` file:

```bash
sudo ufw allow <port>
```

4. Enable the firewall:

```bash
sudo ufw enable
```

5. Check the firewall status:

```bash
sudo ufw status
```

This way, you can open the port in the firewall settings of your server.

