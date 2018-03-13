# pyrrha
Root Pandora Pyrrha repo for deployment of all of the pyrrha repos

## Volumes folders
```
/mnt/rinkeby      # geth node data
/mnt/ipfs/data    # IPFS server database
/mnt/ipfs/export  # folder for exporting of local files into IPFS server (sharing folder)
/logs/webclient   # pyrrha_webclient logs
/logs/boxproxy    # pyrrha_boxproxy logs
/logs/boxplorer   # pyrrha_boxplorer logs
/logs/nginx       # nginx proxy server logs
```

## IPFS CORS config
```sh
docker exec ipfs_host ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin "[\"*\"]"
docker exec ipfs_host ipfs config --json API.HTTPHeaders.Access-Control-Allow-Credentials "[\"true\"]"
docker exec ipfs_host ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods "[\"PUT\", \"GET\", \"POST\"]"
```
After config changes will be done you have to restart container.
```sh
docker restart ipfs_host
```
