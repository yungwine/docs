# Performance Monitoring

## Monitoring TON server performance


Tools like `htop`, `iotop`, `iftop`, `dstat`, `nmon` and others are good for measuring real-time performance, but they lack functionalities when it comes to troubleshooting past performance.

This guide recommends and explains how to use the Linux sar (System Activity Report) utility for TON server performance monitoring.

> This guideline helps to identify if your server experiences any resource shortage, not if validator-engine performs badly.

### Installation
#### SAR Installation

```bash
sudo apt-get install sysstat
```

#### Enable automatic statistics gathering

```bash
sudo sed -i 's/false/true/g' /etc/default/sysstat
```

#### Enable the service

```bash
sudo systemctl enable sysstat sysstat-collect.timer sysstat-summary.timer
```


#### Start the service

```bash
sudo systemctl start sysstat sysstat-collect.timer sysstat-summary.timer
```

### Usage

By default sar gathers statistics every 10 minutes and shows statistics for the current day, starting at midnight. You can check it by running sar without parameters:

```bash
sar
```


If you want to see statistics of the previous day or two days before, pass the number as an option:

```bash
sar -1   # previous day
sar -2   # two days ago
```

For the exact date, you should use the f option to point to the sa file of a given day within a month. Thus, for the September 23rd it would be:

```bash
sar -f /var/log/sysstat/sa23
```

What sar reports to run and how to read them in order to identify performance issues?

Below is the list of sar commands that can be used to gather different system statistics. You can supplement them with the above options to quickly get the reports for the required date.



### Memory report

```bash
sar -rh
```

Since the TON validator-engine utilizes jemalloc feature it caches a lot of data, this is the reason why sar -rh command most of the time returns a low number in column `%memused`.

At the same time, there always be a high number in the column `kbcached`. For the same reason, you should not worry about the low number of free RAM shown in column `kbmemfree`. The important indicator however is the number that comes from the `%memused` column.

If it goes above 90% you should consider adding more RAM and keep an eye if your validator engine is not stopping abnormally due to OOM (out of memory) reason - the best way to check that is to grep `/var/ton-work/log` file for Signal messages.



Swap usage

```bash
sar -Sh
```

If you notice that swap is used you should consider adding more RAM. The general recommendation from the TON Core team is to have swap disabled.



### CPU report

```bash
sar -u
```

If your server utilizes CPU on average up to 70% (see '%user' column), this should be considered as good.



### Disk Usage report

```bash
sar -dh
```

Watch the '%util' column and react accordingly if it stays above 90% for a particular disk.



### Network report

```bash
sar -n DEV -h
```

or

```bash
sar -n DEV -h --iface=<interface name>
```

if you want to filter results by network interface name.

Check out the result of column `%ifutil` - it shows the usage of your interface considering its maximum link speed.

You can check what speed is supported by your NIC by executing the command below:

```bash
cat /sys/class/net/<interface>/speed
```

> This is not the link speed your provider provided you.

Consider upgrading your link speed if `%ifutil` shows above 70% usage, or columns rxkB/s and txkB/s reporting values close to a bandwidth provided by your provider.



### Reporting a performance issue

Before reporting any performance issues, please ensure you are fulfilling the minimal requirements for the node. Then execute the following commands:

```bash
sar -rudh | cat && sar -n DEV -h --iface=eno1 | cat > report_today.txt
```

For yesterday's report execute:

```bash
sar -rudh -1 | cat && sar -n DEV -h --iface=eno1 -1 | cat > report_yesterday.txt
```

Also, stop the TON node and measure your disk IO and network speed.

```bash
sudo fio --randrepeat=1 --ioengine=io_uring --direct=1 --gtod_reduce=1 --name=test --filename=/var/ton-work/testfile --bs=4096 --iodepth=1 --size=40G --readwrite=randread --numjobs=1 --group_reporting
```

Look for value at `read: IOPS=` and send it with the report. A value above 10k IOPS should be considered good.

```bash
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 -
```
Download and Upload speeds above 700 Mbit/s should be regarded as good.

When reporting please send the SAR report as well as IOPS and network speed results to [@mytonctrl_help_bot](https://t.me/mytonctrl_help_bot).
