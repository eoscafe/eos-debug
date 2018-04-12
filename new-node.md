# Build a new node from scratch

**Replace \<user> with your user account (e.g. ubuntu)**
### Clone into new folder
```shell
cd /opt
git clone https://github.com/eosio/eos --recursive
```

### Checkout master branch and update submodules
```shell
cd eos
git checkout master
git pull
git submodule update --init --recursive
```

### Build and install
```shell
./eosio_build.sh
cd build
make install
```

### Validate after build (replace <user> with your user)
```shell
/home/<user>/opt/mongodb/bin/mongod -f /home/<user>/opt/mongodb/mongod.conf &
cd /home/<user>/opt/eos/build; make test
```

**Credits to Bohdan from [Cryptolions][f51cb96f]**

[f51cb96f]: http://cryptolions.io/ "Website"
