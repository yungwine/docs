# FAQ

## MyTonCtrl Directory Usage

MyTonCtrl is a wrapper that stores its files in two places:

1. `~/.local/share/mytonctrl/` - Long-term files such as logs are stored here.
2. `/tmp/mytonctrl/` - Temporary files are stored here.

MyTonCtrl also includes another script, mytoncore, which in turn stores files in the following locations:

1. `~/.local/share/mytoncore/` - Permanent files, the main configuration will be stored here.
2. `/tmp/mytoncore/` - Temporary files, parameters used for elections will be saved here.

MyTonCtrl downloads the source code for itself and the validator into the following directories:

1. `/usr/src/mytonctrl/`
2. `/usr/src/ton/`

MyTonCtrl compiles the validator components into the following directory:

1. `/usr/bin/ton/`

MyTonCtrl creates a working directory for the validator here:

1. `/var/ton/`

---

## If MyTonCtrl was installed as root

The configurations will be stored differently:

1. `/usr/local/bin/mytonctrl/`
2. `/usr/local/bin/mytoncore/`

---

## How to remove MyTonCtrl

Run the script as an administrator and remove the compiled TON components:

```bash
sudo bash /usr/src/mytonctrl/scripts/uninstall.sh
sudo rm -rf /usr/bin/ton
```

During this process, ensure you have sufficient permissions to delete or modify these files or directories.


## Directory Changes with MyTonCtrl

### Changing Validator Working Directory Pre-installation

If you wish to change the working directory of the validator prior to installation, there are two ways to do so:

1. **Fork the project** - You can fork the project and make your changes there. Learn how to fork a project with `man git-fork`.
2. **Create a symbolic link** - You can also create a symbolic link with the following command:

    ```bash
    ln -s /opt/ton/var/ton
    ```
This command will create a link `/var/ton` that points to `/opt/ton`.

### Changing Validator Working Directory Post-installation

If you want to change the working directory of the validator from `/var/ton/` after installation, perform the following steps:

1. **Stop services** - You will need to stop the services with these commands:

    ```bash
    systemctl stop validator.service
    systemctl stop mytoncore.service
    ```

2. **Move validator files** - You then need to move the validator files with this command:

    ```bash
    mv /var/ton/* /opt/ton/
    ```

3. **Update configuration paths** - Replace the paths in the configuration located at `~/.local/share/mytoncore/mytoncore.db`.

4. **Note on experience** - There is no prior experience with such a transfer, so consider this when moving forward.

Remember to make sure you have sufficient permissions to make these changes or run these commands.

## Understanding Validator Status and Restarting Validator in MyTonCtrl

This document will help you understand how to confirm if MyTonCtrl has become a full validator and how to restart your validator.

## Restarting Your Validator

If you need to restart your validator, you can do so by running the following command:

```bash
systemctl restart validator.service
```

Ensure you have sufficient permissions to execute these commands and make necessary adjustments. Always remember to back up important data before performing operations that could potentially affect your validator.
