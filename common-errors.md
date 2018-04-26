# Common Errors

#### IO Stream
```
2356017ms main.cpp:63 main ] 13 St9exception: basic_ios::clear: iostream error
basic_ios::clear: iostream error:
{"my->genesis_file.generic_string()":"/usr/eos/build/genesis.json","what":"basic_ios::clear: iostream error"}
chain_plugin.cpp:257 plugin_startup
```

#### Solution:
```
./nodeos --resync-blockchain
```

#### Peer Network Error:

```console
Peer network version does not match expected 805 but got -28948
```  

#### Solution:

Error indicates that some nodes have networks mismatch set to 1 in config.ini. Stop and restart your node.

#### Dice-of-death Error

While experimenting with the dice contract, the members of the Jungle Testnet realized that trying to push an action would crash most of the nodes.

#### Solution:

~~Don't use that stupid contract~~ Update to a more recent tag (must be higher than dawn-v3.0.0) or `master` branch.


## Reminder
Github tracks issues which you can access for EOS at https://github.com/EOSIO/eos/issues, always use that as your first search for errors.

