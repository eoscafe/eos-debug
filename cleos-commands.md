# Cleos Commands
### To get tokens balance:  
`./cleos get currency balance token <acc_name>`

### To transfer token:
`./cleos push action token transfer '["<from>", "<to>", "1.0000 EOS", "memo"]' -p <from>`

### To transfer EOS:  (Does not show in balance)
`./cleos transfer <from> <to> 10000 'memo'`
