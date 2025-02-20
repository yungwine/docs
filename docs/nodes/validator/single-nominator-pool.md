# How to use Single Nominator Pool

> It's recommended to get acquainted with [Single Nominator Pool Specification](https://docs.ton.org/v3/documentation/smart-contracts/contracts-specs/single-nominator-pool) before reading this tutorial.

### Set up single-nominator

> :warning: 
> Before start make sure you have topped up and [activated](nodes/validator/validator-node#activate-the-wallets) validator's wallet.


1. Enable single nominator mode

```bash
MyTonCtrl> enable_mode single-nominator
```

2. Check if single-nominator mode is enabled.

```bash
MyTonCtrl> status_modes
Name              Status             Description
single-nominator  enabled   Orbs's single nominator pools.
```

3. Create pool

```bash
MyTonCtrl> new_single_pool <pool-name> <owner_address>
```

If you have already created pool it's possible to import it:

```bash
MyTonCtrl> import_pool <pool-name> <pool-addr>
```

4. Type `pools_list` to display pool addresses

```bash
MyTonCtrl> pools_list
Name       Status  Balance  Version   Address
test-pool  empty   0.0      spool_r2  kf_JcC5pn3etTAdwOnc16_tyMmKE8-ftNUnf0OnUjAIdDJpX
```

5. Activate the pool

```bash
MyTonCtrl> activate_single_pool <pool-name>
```

After successfully activated pool:

```bash
MyTonCtrl> pools_list
Name       Status  Balance  Version   Address
test-pool  active  0.99389  spool_r2  kf_JcC5pn3etTAdwOnc16_tyMmKE8-ftNUnf0OnUjAIdDJpX
```

Now you can work with this pool via mytonctrl like with a standard nominator pool.

> If the pool's balance is enough to participate in both rounds (`balance > min_stake_amount * 2`) then MyTonCtrl will automatically participate in both rounds using `stake = balance / 2`, unless user sets stake manually using command `set stake`. This behaviour is different from using a nominator pool but similar to staking using validator wallet.
