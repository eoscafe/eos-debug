# Smart Contracts

## Requirements
- Have a `nodeos` instance running with `wallet_api_plugin` enabled (or running `keosd` locally).
- Made or received an account along with its public/private key pair.
- Have read through local-wallet.md before proceeding.

If keosd is running locally, most of these commands may require the `--wallet-host <your-wallet-host> --wallet-port <your-wallet-port>` arguments after `cleos` but before the subcommands. For now, we will not be adding them into the commands below.

## Setup

### Make sure wallet is unlocked

`./cleos wallet list`

If your wallet is unlocked, there will be a `*` next to your wallet. If there is no `*`, run:

`./cleos wallet unlock`

This will prompt you to input the password for your wallet, which you should have kept somewhere safe.

You need your wallet unlocked in order to import keys that you generate. Furthermore, you need the appropriate keys for the accounts that you are using imported into your wallet to be able to use them.

### Create keys

`./cleos create key`

This will generate a key pair. The public key is the one that begins with "EOS". You may share this with others, but do *not* share your private key with anyone.

## Token contract

### Create token account to deploy contract

Let's create an account to deploy the contract on. To do this, you must have an existing account name and keys associated with it in your wallet.

`./cleos create account <existing-account-name> token <Public-Key-1> <Public-Key-2> -p <existing-account-name>`

You can change the token account name (which I specified as "token" above) to anything you want as long as it fits the rules for an account name.

If you are connected to a testnet, another node may have claimed an account with the name "token" already, in which case you must use another account name.

### Import key into wallet

`./cleos wallet import <private-key>`

This gives you permission to use the "token" account created above.

### Deploy token contract

You are now ready to deploy the token contract. We are starting with the token contract as it is quite basic, and has similar actions to the other contracts that we will experiment with.

Assuming you are running from your home directory:

`./cleos set contract token /path-to-eos/build/contracts/eosio.token -p token`

Again, if you did not set your token account name as "token", substitute it in for "token" above.

#### Something to keep in mind

When working through the steps below, you may start to notice that most push action commands use this structure: 
```./cleos push action <action-name> <account-where-contract-is-deployed> '["param-1", "param-2", ... , "param-x"]'```  
It can be helpful to remember this syntax beforehand, as you will be using it quite a lot.

There are also two very common errors types that occur when using the commands in the steps below. These are the `unknown key` errors and `permission` related errors. When you receive an `unknown key` error, double check the input of your parameters in your command. It is most likely that something went wrong there. When you receive a `permission` related error, it means that either your wallet isn't open, you didn't call the --wallet-host and/or --wallet-port arguments, or doesn't have the private key of the account you're calling with `-p` imported already. You could also be calling for the permission of the wrong account for that particular situation.

### Token create

Push the `create` action for the token contract using the command below:

`./cleos push action create token '["<your-account-name>", "<maximum-supply>", 0, 0, 0]' -p <your-account-name>`

The first parameter is the issuer of the currency. Later, when you are issuing currency using the token contract, you will need the permission of the issuer account define above. As the name suggest, the maximum supply is the maximum amount of that currency that can be issued. Some examples would be "1000000.0000 DOL" or "300000000.0000 WWW". From experience, "1000000 DOL" is counted as a different currency from "1000000.0000 DOL". However, you can't reuse the exact same currency name using the same contract.

### Token issue

Push the `issue` action for the token contract using the command below:

`./cleos push action token issue '["<your-account-name>", "<quantity>", "memo"]' -p <your-account-name>`
