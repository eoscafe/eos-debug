# Local Wallet Setup

When running a nodeos on a testnet, it is important that you run your wallet daemon (keosd) locally. Make sure that the `wallet_api_plugin` and `wallet_plugin` are disabled in the config.ini of nodeos. The wallet holds valuable keys that gives you permission to control your account. If the wallet is run on nodeos while connected testnet with other nodes, it could be possible for others to access your wallet, leaving the data stored in it vulnerable.

## Starting up keosd

To begin with, let's run keosd.

```console
./keosd
```

It should output something like this:

```console
27049ms thread-0   wallet_plugin.cpp:41          plugin_initialize    ] initializing wallet plugin
27049ms thread-0   http_plugin.cpp:141           plugin_initialize    ] host: 127.0.0.1 port: 8887
27049ms thread-0   http_plugin.cpp:144           plugin_initialize    ] configured http to listen on 127.0.0.1:8887
27049ms thread-0   http_plugin.cpp:213           plugin_startup       ] start listening for http requests
27050ms thread-0   wallet_api_plugin.cpp:70      plugin_startup       ] starting wallet_api_plugin
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/create
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/get_public_keys
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/import_key
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/list_keys
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/list_wallets
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/lock
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/lock_all
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/open
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/set_timeout
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/sign_transaction
27050ms thread-0   http_plugin.cpp:242           add_handler          ] add api url: /v1/wallet/unlock
```

Exit out of wallet using `ctrl-C`

## Wallet configurations

By default, keosd will have created a folder called eosio-wallet in your home directory. This folder is where the config.ini file for keosd is kept, as well as the wallets.

Your config.ini file should look so like this:

```console
# The local IP and port to listen for incoming http connections. (eosio::http_plugin)
http-server-address = 127.0.0.1:<wallet-port>

# Specify the Access-Control-Allow-Origin to be returned on each request. (eosio::http_plugin)
# access-control-allow-origin =

# Specify the Access-Control-Allow-Headers to be returned on each request. (eosio::http_plugin)
# access-control-allow-headers =

# Specify if Access-Control-Allow-Credentials: true should be returned on each request. (eosio::http_plugin)
access-control-allow-credentials = false

# Plugin(s) to enable, may be specified multiple times
# plugin =
plugin = eosio::wallet_api_plugin
plugin = eosio::wallet_plugin
```

Edit the config.ini file so that <wallet-port> is different from the port that nodeos is running on. For example, if nodeos has its `http-server-endpoint` set to port 8888, you could use port 8000 for you wallet. Save and exit the config.ini file.

## Run basic wallet commands

Now, you're ready to use your wallet! Let's run keosd again. We'll assume that we are running nodeos on port 8888, keosd is hosted on localhost, and the wallet port is 8000.

```console
./keosd --http-server-address localhost:8000
```

In another terminal window, we can run a cleos command to test if the wallet is working correctly. Whenever you use a command that requires permission from the wallet, you will need to use the --wallet-host and --wallet-port arguments as show below.

```console
./cleos -H localhost -p 8888 --wallet-host localhost --wallet-port 8000
```

Assuming you are running nodeos, you will need to specify the host and port that your wallet is running on. If you set up the config.ini in the eosio-wallet directory correctly, you should get an output like this:

```
./cleos --wallet-host localhost --wallet-port 8000 wallet list
Wallets:
[]
```

Finally, create your default wallet by running the following command:

```console
./cleos --wallet-host localhost --wallet-port 8000 wallet create
./cleos  --wallet-host localhost --wallet-port 8000 wallet import <private key>
```

This should output a password for your wallet. **Keep this password somewhere safe.** If your system logs are compromised, you can potentially lose access and funds associated with your account. If you lose the password, it will almost be impossible to access it again, which could jeopardize your access to your accounts on the blockchain. For one last sanity check, go to your eosio-wallet directory. You should find a file called default.wallet, which means that your wallet has been created successfully!
