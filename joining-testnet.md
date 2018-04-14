# Join a Testnet

### Requirements
- Have `keosd` running (look at local-wallet guide)
- Have a local node built and installed (look at new-node guide)
- Have `git` installed
- Have a computer with decent specifications

## **Jungle Testnet:**
1. Generate EOS key pair from `cleos` and save it privately in a safe place:
     ```console
    ./cleos create key
     ```
2. Set up data directory
     ```console
      git clone https://github.com/CryptoLions/EOS-Jungle-Testnet.git /opt/JungleTestnet
      cd /opt/JungleTestnet
      chmod 777 start.sh stop.sh
     ```
3. Open up `start.sh` and replace the directory with your EOS node directory (e.g. /opt/eos)
   ```console
   NODEOSBINDIR="/home/eos-dawn-v3.0.0/eos/build/programs/nodeos"
   ```
4. Head on over to http://jungle.cryptolions.io:9898/monitor/ and come up with a jungle related animal name not taken (no capitals/spaces, only lower case alphabetical names or `.`).

5. Using your new jungle animal and the local-wallet.md guide:  
    a. Create account in `cleos` with the jungle animal name (for example `lion`)  
    b. Create new public:private key pair  
    c. Add your public:private key pair to your new account.
6. Add newly created account to wallet in `keosd`.
7. Open up `config.ini` and do the following:
   * Replace line 85
      ```console
      producer-name = YOUR_PB_NAME
      ```
      with your own jungle animal name like
      ```console
      producer-name = lion
      ```

   *  Delete line 84
      ```console
      private-key = ["YOUR_PUBKEY","YOUR_PRIVKEY"]
      ```
   * Replace line 16
      ```console
      p2p-server-address = your-domain:9876
      ```
     with YOUR OWN ip address or domain name like
     ```console
     p2p-server-address = eoscalgary.com:9876
     ```

8. Open up ports 8888 and 9876 on your router/cloud setup

9. Start your node with the command:
   ```console
   ./start.sh
   ```
10. Open up http://your_website:your_http_port/v1/chain/get_info to see if you get a JSON response. If so, your node has been correctly configured.
11. Once your node is syncing, message Bohdan at https://t.me/jungletestnet with this information:

| Server Location | Organisation | node ip/domain, | Port (http) |  Port (p2p) | producer name | your public key|
|-----------------|--------------|-----------------|-------------|-------------|---------------|----------------|



### Reading log messages
Use this command to have a stream of log messages
```console
tail -f stderr.txt
```
