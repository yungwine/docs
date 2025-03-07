# MyTonCtrl status

Here is an explanation of `status` command output.

![status](/../../img/mytonctrl/status.png)

## TON network status

### Network name

Possible values: `mainnet`, `testnet`, `unknown`. `unknown` mustn't be printed ever.

### Number of validators

There are two values: one green and one yellow. Green is the number of online validators, yellow is a number of all
validators.
Must be an integer greater than 0, MyTonCtrl get it with command `getconfig 34`,
check [this (param 32-34-and-36) section](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-32-34-and-36) for more.

### Number of shardchains

Must be an integer greater than 0, has green color.

### Number of offers

There are two values: one green and one yellow. Green is a number of `new offers`, yellow is a number of `all offers`.

### Number of complaints

There are two values: one green and one yellow. Green is a number of `new complaints`, yellow is a number
of `all complaints`.

### Election status

Can be green text `open` or yellow `closed`.

## Local validator status

### Validator Index

If the validator index is higher or equal to 0 (and it should be so if validator mode is active), it must be green, else - red.

### ADNL address of local validator

Just ADNL address.

### Local validator wallet address

The address used for staking must be a valid TON address. 

### Local validator wallet balance

The balance of the wallet.

### Load average

Has format `[int]: int, int, int`. First one is a number of cpus, others represent the system load average for the last 1, 5 and 15 minutes.

### Network load average

Three integers, same logic as for `load average`: system load average for the last 1, 5 and 15 minutes.

### Memory load

Two pairs of integers, absolute and relative memory usage of ram and swap.

### Disks load average

Same as for `memory load`, but for all disks.

### Mytoncore status

Should be green, tells how much time Mytoncore is up.

### Local validator status

Should be green, tells how much time the local validator is up.

### Local validator out of sync

Integer should be less than 20 (it will be green so).

### Local validator last state serialization

Shows the number of out-of-service masterchain blocks

### Local validator database size

The absolute load should be less than 1000 GB, a relative load should be less than 80%.

### Version mytonctrl

Hash of commit and name of branch.

### Version validator

Hash of commit and name of branch.

## TON network configuration

### Configurator address

Configurator address, check [this param 0](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-0) for more.

### Elector address

Elector address, check [this param 1](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-1) for more.

### Validation period

Validation period in seconds, check [this param 15](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-15) for more.

### Duration of elections

Duration of elections in seconds, check [this param 15](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-15) for more.

### Hold period

Hold period in seconds, check [this param 15](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-15) for more.

### Minimum stake

Minimum stake in TONs, check [this param 17](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-17) for more.

### Maximum stake

Maximum stake in TONs, check [this param 17](https://docs.ton.org/v3/documentation/network/configs/blockchain-configs#param-17) for more.

## TON timestamps

### TON Network Launch

The time when the current network (mainnet or testnet) was launched.

### Start of the Validation Cycle

The timestamp for the start of the validation cycle; it will be green if it represents a future moment.

### End of the Validation Cycle

The timestamp for the end of the validation cycle; it will be green if it represents a future moment.

### Start of Elections

The timestamp for the start of the elections; it will be green if it represents a future moment.

### End of Elections

The timestamp for the end of the elections; it will be green if it represents a future moment.

### Beginning of the Next Elections

The timestamp for the start of the next elections; it will be green if it represents a future moment.
