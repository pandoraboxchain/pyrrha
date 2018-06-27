FROM ethereum/client-go

ENTRYPOINT geth --rinkeby --cache 1024 --syncmode "fast" --identity pandora --rpc --rpcaddr 0.0.0.0 --rpcapi eth,net,web3,personal,debug --rpccorsdomain "*" --rpcvhosts "*" --ws --wsaddr 0.0.0.0 --wsapi eth,net,web3,personal --wsorigins "*"
