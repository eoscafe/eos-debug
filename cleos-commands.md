# Cleos Commands
### To get tokens balance:  
```console
./cleos get currency balance token <acc_name>
```

### To transfer token:
```console
./cleos push action token transfer '["<from>", "<to>", "1.0000 EOS", "memo"]' -p <from>
```

### To transfer EOS:  (Does not show in balance)
```console
./cleos transfer <from> <to> 10000 'memo'
```
