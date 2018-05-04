# Cleos Script

If you have a local wallet running and multiple nodes running for different testnets (and a little lazy), you can save a lot of time by writing a cleos script. This can save you from typing the node and wallet url arguments into the command line whenever you want to run a command using cleos.

### Set up local wallet

Check the local-wallet [guide] (https://github.com/eoscafe/eos-debug/blob/master/local-wallet.md) to set up keosd, the wallet daemon.

### Create script

First, go into the testnet folder for your node, and create the file for your script:

```console
cd /path/to/testnet/folder
nano cleos.sh
```

Then, copy and paste the script below:

```console
#!/bin/bash
NODEOSBINDIR=/home/jae/eos-april/eos/build/programs
WALLETHOST="<your-wallet-host>"
NODEHOST="<your-node-domain-or-IP>"
NODEPORT="<your-node-port>"
WALLETPORT="<your-wallet-port>"

$NODEOSBINDIR/cleos/cleos -u http://$NODEHOST:$NODEPORT --wallet-url http://$WALLETHOST:$WALLETPORT "$@"
```

Make sure that you edit in the correct information for your wallet and node. As a final step, give cleos.sh execution rights.

```console
sudo chmod 777 cleos.sh
```

### Run cleos.sh

Check if cleos.sh is working by running the following command:

```console
./cleos.sh wallet list
```

If everything works correctly, this should output the wallets that are currently open on your wallet daemon.
