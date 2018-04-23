# Smart Contracts

## Requirements
- Have a `nodeos` instance running with `wallet_api_plugin` enabled (or running `keosd` locally).
- Made or received an account along with its public/private key pair.
- Have read through local-wallet.md before proceeding.

If keosd is running locally, most of these commands may require the `--wallet-host <your-wallet-host> --wallet-port <your-wallet-port>` arguments after `cleos` but before the subcommands. For now, we will not be adding them into the commands below.

## Setup

### Make sure wallet is unlocked

```console
./cleos wallet list
```

If your wallet is unlocked, there will be a `*` next to your wallet. If there is no `*`, run:

```console
./cleos wallet unlock
```

This will prompt you to input the password for your wallet, which you should have kept somewhere safe.

You need your wallet unlocked in order to import keys that you generate. Furthermore, you need the appropriate keys for the accounts that you are using imported into your wallet to be able to use them.

### Create keys

```console
./cleos create key
```

This will generate a key pair. The public key is the one that begins with "EOS". You may share this with others, but do *not* share your private key with anyone.

## Token contract

### Create token account to deploy contract

Let's create an account to deploy the contract on. To do this, you must have an existing account name and keys associated with it in your wallet.

```console
./cleos create account <existing-account-name> token <Public-Key> <Public-Key> -p <existing-account-name>
```

You can change the token account name (which I specified as "token" above) to anything you want as long as it fits the rules for an account name.

If you are connected to a testnet, another node may have claimed an account with the name "token" already, in which case you must use another account name.

### Import key into wallet

```console
./cleos wallet import <Private-Key>
```

This gives you permission to use the "token" account created above.

### Deploy token contract

You are now ready to deploy the token contract. We are starting with the token contract as it is quite basic, and has similar actions to the other contracts that we will experiment with.

Assuming you are running from your home directory:

```console
./cleos set contract token /path-to-eos/build/contracts/eosio.token -p token
```

Again, if you did not set your token account name as "token", substitute it in for "token" above.

### Something to keep in mind

When working through the steps below, you may start to notice that most push action commands use this structure:
```console
cleos push action <account-where-contract-is-deployed> <action-name> '["param-1", "param-2", ... , "param-x"]'
```
It can be helpful to remember this syntax beforehand, as you will be using it quite a lot.

There are also two very common errors types that occur when using the commands in the steps below. These are the `unknown key` errors and `permission` related errors. When you receive an `unknown key` error, double check the input of your parameters in your command. It is most likely that something went wrong there. When you receive a `permission` related error, it means that either your wallet isn't open, you didn't call the --wallet-host and/or --wallet-port arguments, or doesn't have the private key of the account you're calling with `-p` imported already. You could also be calling for the permission of the wrong account for that particular situation.

### Token create

Push the `create` action for the token contract using the command below:

```console
./cleos push action token create '["<your-account-name>", "<maximum-supply>", 0, 0, 0]' -p token
```

The first parameter is the issuer of the currency. Later, when you are issuing currency using the token contract, you will need the permission of the issuer account define above. As the name suggest, the maximum supply is the maximum amount of that currency that can be issued. Some examples would be "1000000.0000 DOL" or "300000000.0000 WWW". From experience, "1000000 DOL" is counted as a different currency from "1000000.0000 DOL". However, you can't reuse the exact same currency name while using the same contract.

### Token issue

Push the `issue` action for the token contract using the command below:

```console
./cleos push action token issue '["<account-to-issue>", "<quantity>", "<memo>"]' -p <your-account-name>
```

Note that in order to use the `issue` action, you need the permission of the issuer account, which was defined when using the `create` action above. Using the `issue` command, you can generate currency for any account specified, as long as it exists on the blockchain.

It is easy to get confused between the `create` and `issue` actions. Keeping the following in mind will help:

##### Create
* Defines the "issuer". You need the permission of the issuer to use the issue action.
* Defines the namespace of your currency.
* Sets the maximum amount of the currency that can be generated.
* Requires the permission of the account that the token contract has been deployed on (in this case, "token").

##### Issue
* Actually generates the currency you defined using create.
* Requires the permission of the issuer account defined using create.

### Token transfer

Now, push the `transfer` action for the token contract with this command:

```console
./cleos push action token transfer '["<account-from>", "<account-to>", "<quantity>" "<memo>"]' -p <account-from>
```

The transfer action allows you to transfer currency between accounts. It is required to have permission for the account that you are sending the balance from. The quantity can vary, as long as you are using an existing currency namespace, and you have enough balance in the account specified.

If you have encountered any errors along the way, refer to the **Something to keep in mind** step. Coming from our experience, most of the errors stemmed from the problems mentioned in that section.

### Check balance

After playing around with these actions, you may be wondering how to check how much balance is remaining in your account. You can check using the following command.

```console
./cleos get currency balance token <account-name>
```

A sample output could look like this:
```console
950000.0000 RHINO
1000000 RHINO
999900.0000 CAFE
159302.1935 HKEOS
```

Note that the command outputs any positive amount of currency associated with the token contract for your account. In this case, it seems like the user created a version of the RHINO currency with the decimal points, and another using whole numbers. As mentioned above, the contract will assume that they are different currencies. It is also possible to check the balance on any account on the blockchain using this command (note that there was no permission required for the command).

If everything has been successful up to this point, you are now ready to move on to exploring the other smart contracts!

## Exchange contract

The exchange contract is quite similar to the token contract; it also contains the basic `create`, `issue`, and `transfer` actions that can be used very similar commands. We wil simply list the commands below, as the logic behind them has already been explained.

### Create another pair of keys

```console
./cleos create key
```

### Create account to deploy exchange contract
```console
./cleos create account <existing-account-name> exchange <Public-Key> <Public-Key> -p <existing-account-name>
```

### Import key into wallet

```console
./cleos wallet import <Private-Key>
```

### Deploy exchange contract

```console
./cleos set contract exchange /path-to-eos/build/contracts/exchange -p exchange
```

### Exchange create

```console
./cleos push action exchange create '["<your-account-name>", "<maximum-supply>", 0, 0, 0]' -p exchange
```

### Exchange issue

```console
./cleos push action exchange issue '["<account-to-issue>", "<quantity>", "<memo>"]' -p <your-account-name>
```

### Exchange transfer

```console
./cleos push action exchange transfer '["<account-from>", "<account-to>", "<quantity>" "<memo>"]' -p <account-from>
```

### Check balance

```console
./cleos get currency balance exchange <account-name>
```

Note that you can check balance for any contract by inputting the account name where the contract was deployed.
