# Build a new node from scratch

### Clone into new folder
```console
cd /opt
git clone https://github.com/eosio/eos --recursive
```

### Checkout master branch and update submodules
```console
cd eos
git checkout master
git pull
git submodule update --init --recursive
```

### Build and install
```console
./eosio_build.sh
cd build
make install
```

Master branch may have breaking changes, if you want a more stable version, you can use `git checkout dawn-v3.0.0
` or `git checkout DAWN-2018-04-23-ALPHA`. If EOS built with no errors, then you may skip the **Build Error** step.

### Build error

After running `./eosio_build.sh`, you could receive a CMake error. There are typically two workarounds to this problem.

#### 1. Manual Build

If you cloned the eos folder into your home directory, you can run the following commands exactly. Otherwise, you may have to edit the directories in the commands as necessary.

```console
cd ~
mkdir -p ~/eos/build && cd ~/eos/build
cmake -DBINARYEN_BIN=~/binaryen/bin -DWASM_ROOT=~/wasm-compiler/llvm -DOPENSSL_ROOT_DIR=/usr/local/opt/openssl -DOPENSSL_LIBRARIES=/usr/local/opt/openssl/lib ..
make -j$( nproc )
```

#### 2. Clean Install Dependencies (again)

The commands for manually installing the dependencies can be found below.

https://github.com/EOSIO/eos/tree/dawn-v3.0.0#clean-install-ubuntu-1604--linux-mint-18

After completing the steps, go back to the **Manual Build** step.

### Validate after build (replace \<user> with your user)
```console
/home/<user>/opt/mongodb/bin/mongod -f /home/<user>/opt/mongodb/mongod.conf &
cd /home/<user>/opt/eos/build; make test
```

**Credits to Bohdan from [Cryptolions][f51cb96f]**

[f51cb96f]: http://cryptolions.io/ "Website"
