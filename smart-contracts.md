# Smart Contracts

We will assume that you already have an instance nodeos running, and wallet_api_plugin enabled (or running keosd locally). We will also assume that you have made or received an account along with its keys. We strongly advise that you read through local-wallet.md first before going on.

If keosd is running locally, most of these commands may require the `--wallet-host <your-wallet-host> --wallet-port <your-wallet-port>` arguments after `cleos` but before the subcommands. For now, we will not be adding them into the commands below.

### Make sure wallet is unlocked

`$ cleos wallet list`

If your wallet is unlocked, there will be a `*` next to your wallet. If there is no `*`, run:

`$ cleos wallet unlock`

This will prompt you to input the password for your wallet, which you should have kept somewhere safe.

You need your wallet unlocked in order to import keys that you generate. Furthermore, you need the appropriate keys for the accounts that you are using imported into your wallet to be able to use them.

### Create keys

`$ cleos create key`

This will generate a key pair. The public key is the one that begins with "EOS". You may share this with others, but do *not* share your private key with anyone.

### Create token account to deploy contract

Let's create an account to deploy the contract on. To do this, you must have an existing account name and keys associated with it in your wallet.

`cleos create account <existing-account-name> token <Public-Key-1> <Public-Key-2> -p <existing-account-name>`

You can change the token account name (which I specified as "token" above) to anything you want as long as it fits the rules for an account name. 

If you are connected to a testnet, another node may have claimed an account with the name "token" already, in which case you must use another account name.

### Import key into wallet

`$ cleos wallet import <private-key>`

This gives you permission to use the "token" account created above.

### Deploy token contract

You are now ready to deploy the token contract. We are starting with the token contract since it is quite basic, and has similar actions as the other contracts that we will experiment with.

**Note:** Two very common errors that occur when using the commands below are `unknown key` errors and `permission` related errors. When you receive an `unknown key` error, double check the input of your parameters in your command. It is most likely that something went wrong there. When you receive a `permission` related error, it means that either your wallet isn't open or doesn't have the private key of the account you're calling with `-p` imported already. You could also be calling for the permission of the wrong account for that particular situation.
