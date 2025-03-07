# Full Node

## OS requirements

We highly recommend installing MyTonCtrl using the supported operating systems:
* Ubuntu 20.04
* Ubuntu 22.04
* Debian 11

## Hardware requirements

> :warning: **Node usage on personal local machine**
>
> You shouldn't run any type of node on your personal local machine for long, even if it satisfies the requirements. Nodes actively use disks and can damage them quickly.

### With validator

- 16 cores CPU
- 128 GB RAM
- 1TB NVME SSD _OR_ Provisioned 64+k IOPS storage
- 1 Gbit/s network connectivity
- Public IP address (_fixed IP address_)
- 64 TB/month traffic (100 TB/month on peak load)

> Be ready for peak loads
>
> Typically you'll need at least a 1 Gbit/s connection to reliably accommodate peak loads (the average load is expected to be approximately 100 Mbit/s).

### Port Forwarding

All types of nodes require a static external IP address, one UDP port to be forwarded for incoming connections and all outgoing connections to be open - the node uses random ports for new outgoing connections. It is necessarily for the node to be visible to the outside world over the NAT.

This can be done with your network provider or by [renting a server](#recommended-providers) to run a node.

> You can find out which UDP port is open using the `netstat -tulpn` command.

### Recommended Providers

The TON Core recommends the following providers for running a Validator:

| Cloud Provider   | Instance Type         | CPU            | RAM    | Storage           | Network      | Public IP                         | Traffic       |
|------------------|-----------------------|----------------|--------|-------------------|--------------|-----------------------------------|---------------|
| GCP              | `n2-standard-16`      | `32 vCPUs`     | `128GB`| `1TB NVMe SSD`    | `16 Gbps`    | Reserve a static external IP      | `64 TB/month`|
| Alibaba Cloud    | `ecs.g6.4xlarge`      | `32 vCPUs`     | `128GB`| `1TB NVMe SSD`    | `Up to 10 Gbps` | Bind an Elastic IP           | `64 TB/month`|
| Tencent Cloud    | `M5.4XLARGE`          | `32 vCPUs`     | `128GB`| `1TB NVMe SSD`    | `Up to 10 Gbps` | Associate an Elastic IP       | `64 TB/month`|
| Vultr            | `bare metal Intel E-2388G` | `16 Cores / 32 Threads` | `128GB` | `1.92TB NVMe SSD` | `10 Gbps` | Fixed IP address included with instance | `64 TB/month`|
| DigitalOcean     | `general purpose premium Intel` | `32 vCPUs` | `128GB` | `1TB NVMe SSD` | `10 Gbps` | Fixed IP address included with instance | `64 TB/month`|
| Latitude         | `c3.medium.x86`       | `16 Cores / 32 Threads` | `128GB` | `1.9TB NVMe SSD` | `10 Gbps` | Fixed IP address included with instance | `64 TB/month`|
| AWS              | `i4i.8xlarge`         | `32 vCPUs`     | `256GB`| `2 x 3,750 AWS Nitro SSD (fixed)` | `Up to 25 Gbps` | Bind an Elastic IP | `64 TB/month`|

> **Note:** Prices, configurations, and availability may vary. It is advisable to always check the official documentation and pricing pages of the respective cloud provider before making any decisions.

## Run a Node

### Switch to non-root user

> :warning: This step is **required** to successfully install and use mytonctrl — don't ignore **non-root user creation**. Without this step, there will be no errors during installation, but mytonctrl will not work properly.

If you don't have **non-root** user, you can create one with the following steps (otherwise skip first two steps and go to the third).

1. Login as root and create new user:

```bash
sudo adduser <username>
```

2. Add your user to the sudo group:

```bash
sudo usermod -aG sudo <username>
```

3. Log into the new user (if you are using ssh, **you will need to stop current session and reconnect with correct user**)

```bash
ssh <username>@<server-ip-address>
```

### Install the MyTonCtrl

Download and run the installation script from the **non-root** user account with **sudo** privileges:

Ubuntu: 
  ```bash
  wget https://raw.githubusercontent.com/ton-blockchain/mytonctrl/master/scripts/install.sh
  sudo bash install.sh
  ```

Debian:
  ```bash
  wget https://raw.githubusercontent.com/ton-blockchain/mytonctrl/master/scripts/install.sh
  su root -c 'bash install.sh'
  ```


* `-d` - **mytonctrl** will download a [dump](https://dump.ton.org/) of the latest blockchain state.
This will reduce synchronization time by several times.
* `-c <path>` - If you want to use not public liteservers for synchronization. _(not required)_
* `-i` - Ignore minimum requirements, use it only if you want to check compilation process without real node usage.
* `-m` - Mode, can be `validator` or `liteserver`.
* `-t` - Disable telemetry.

**To use testnet**, `-c` flag should be provided with `https://ton.org/testnet-global.config.json` value.

Default `-c` flag value is `https://ton-blockchain.github.io/global.config.json`, which is default mainnet config.

### Run the mytonctrl

1. Run `MyTonCtrl` console **from the local user account used for installation**:

  ```sh
  mytonctrl
  ```

2. Check the `MyTonCtrl` status using the `status` command:

  ```sh
  status
  ```


**In testnet** `status fast` command must be used instead of `status`.


The following statuses should be displayed:

* **mytoncore status**: Should be in green.
* **local validator status**: Should also be in green.
* **local validator out of sync**: Initially, a `n/a` string is displayed. As soon as the newly created validator connects with other validators, the number will be around 250k. As synchronization progresses, this number decreases. When it falls below 20, the validator is synchronized.

Example of the **status** command output:

![status](../../img/nodes-validator/mytonctrl-status.png)

> :warning: **Make sure you have same output for status**
> 
> For all nodes type **Local Validator status** section should appear.
Otherwise, [check troubleshooting section](/nodes/nodes/nodes-troubleshooting.md#status-command-displays-without-local-node-section) and [check node logs](/nodes/nodes/full-node.md#check-the-node-logs).

Wait until `Local validator out of sync` becomes less than 20 seconds.

When a new node have been started, even from dump, **it's needed to wait up to 3 hours before out of sync number starts to decrease**. This is because the node still needs to establish its place in the network, propagate its addresses via DHT tables, etc.

### Uninstall mytonctrl

Download script and run it:

```bash
sudo bash /usr/src/mytonctrl/uninstall.sh
```

### Check mytonctrl owner

Run:

```bash
ls -lh /var/ton-work/keys/
```

## Tips & Tricks

### List of available commands

- You can use `help` to get a list of available commands:

![Help command](/../../img/full-node/help.jpg)

### Check the mytonctrl logs

- To check **mytonctrl** logs, open `~/.local/share/mytoncore/mytoncore.log` for a local user or `/usr/local/bin/mytoncore/mytoncore.log` for Root.

![logs](/../../img/nodes-validator/manual-ubuntu_mytoncore-log.png)

### Check the node logs

Check the node logs in case of failure:

```bash
tail -f /var/ton-work/log.thread*
```

## Support

Contact technical support with [@mytonctrl_help](https://t.me/mytonctrl_help).
