# Common Errors

#### Error:

```console
Peer network version does not match expected 805 but got -28948
```  

#### Solution:

Error indicates that some nodes have networks mismatch set to 1 in config.ini. Stop and restart your node.

#### Dice-of-death Error

While experimenting with the dice contract, the members of the Jungle Testnet realized that trying to push an action would crash most of the nodes.

#### Solution:

~~Don't use that stupid contract~~ Update to a more recent tag (must be higher than dawn-v3.0.0) or `master` branch.
