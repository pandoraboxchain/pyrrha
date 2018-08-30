# Pandora Pyrrha release guide

## Common repositories rules

### Branches
- master (production releases)
- develop (development branch)
- staging (staging releases)
- feature-`<feature_name>` (new features development)
- hotfix-`<hotfix_name>` (fixes)

### Versions
Pandoraboxchain is following the [semantic versioning](https://semver.org/).  
Staging releases versions are marking by "`staging`" postfix.

### Types of releases
- production  
- staging

## Steps
1) [Contracts deplyment](#contracts_deployment)
2) [Server configuration](#server_configuration)
3) [Webclient deployment](#webclient_deployment)
4) [Boxproxy deployment](#boxproxy_deployment)
5) [Boxplorer deployment](#boxplorer_deployment)
6) [Pynode & workers deployment](#pynode_deployment)

## Before of the each step
- All features and hotfixes related to the release should be merged into `develop` branch, tested (`npm run test`) and pushed to the repository
- Dependencies versions should be updated and fixed to its tested versions (mainly latest)

## <a name="contracts_deployment"></a>Contracts deployment
Pandoraboxchain contracts repository: [https://github.com/pandoraboxchain/pyrrha-consensus](https://github.com/pandoraboxchain/pyrrha-consensus)  
- Contracts code should following the ["Solidity style guide"](https://solidity.readthedocs.io/en/v0.4.24/style-guide.html). Code linter (`solium`) can be started by the command: `npm run lint`
- Each release should contain:  
    - Fresh build of contracts docs (`npm run docs`)
    - Fresh build of contracts (`npm run compile`)
    - Updated package version (`package.json`)
    - Updated `version` property in the root contract (`./contracts/pandora/Pandora.sol`). This version number should be equal to the number defined in the package.json

Before the contracts deployment should be checked the amount of funds available at the address `0x567Eb9E8D8A43C24c7bac4cb4b51ca806cFE8996` (Rinkeby network). Amount should not be less then 3 ETH.  

All required changes should be merged and pushed into a proper repository branch (master or staging) with proper version tag (`git tag <version_tag>`).

Continuous integration (CI) system is configured to make a proper type of deployment in depending on the version tag. Production versions are looks like `v0.5.0`, and staging versions look like `v0.4.7-staging`.  

Contracts deployment will start automatically after a new tag is pushed to the repository with command `git push --tags`.  

Deployment process can be monitored at [https://travis-ci.org/pandoraboxchain/pyrrha-consensus](https://travis-ci.org/pandoraboxchain/pyrrha-consensus).

After a deployment process will finished new root contracts addresses automatically will be published as an issue in the depended repository and looks like:
```
Pandora:"0xf23f45caa5c697c54d2e92ecbee48855233040e1",   
PandoraMarket:"0xd1feab2687c6beea17d8c1f728aaf54aed5c0ea9"  
```

## <a name="server_configuration"></a>Server configuration  
Pyrrha repository: [https://github.com/pandoraboxchain/pyrrha](https://github.com/pandoraboxchain/pyrrha)  

Two branches are available for each type of deployment: master (is for production) and staging (is for staging). These are branches can not be merged and should be supported separately from each other.

Contracts addresses in the file `./containers/docker-compose.yaml` should be replaced with new addresses obtained during [previous step](#contracts_deployment).

Changes should be pushed to the repository as regular (no tags required).  

Physical servers are can be accessed via ssh:
- `ssh <username>@pandora.network`
- `ssh <username>@staging.pandora.network`


## <a name="webclient_deployment"></a>Webclient deployment  
Webclient repository: [https://github.com/pandoraboxchain/pyrrha-webclient](https://github.com/pandoraboxchain/pyrrha-webclient)

In the proper branch (master or staging) check a required `pyrrha-consensus` submodule configuration (file `./.gitmodules`). For master branch a submodule branch usually is `master` (or a specific tagged branch). For staging branch can be used other configuration.

Do not forget update submodule with command `git submodule update --recursive --remote`  

Contracts addresses in files `./Dockerfile` and `./src/config/index.js` (`rinkeby` and `rinkebyinfura` sections) should be replaced with new addresses obtained during [contracts deployment step](#contracts_deployment).  

All required changes should be merged and pushed into a proper repository branch (master or staging) with a proper version tag (`git tag <version_tag>`).

Continuous integration (CI) system is configured to make a proper type of deployment in depending on the version tag. Production versions are looks like `v1.6.0`, and staging versions look like `v1.5.9-staging`.  

Deployment will start automatically after a new tag is pushed to the repository with command `git push --tags`.  

Deployment process can be monitored at [https://travis-ci.org/pandoraboxchain/pyrrha-webclient](https://travis-ci.org/pandoraboxchain/pyrrha-webclient).


## <a name="boxproxy_deployment"></a>Boxproxy deployment  
Boxproxy repository: [https://github.com/pandoraboxchain/pyrrha-boxproxy](https://github.com/pandoraboxchain/pyrrha-boxproxy)  

Similarly to the [previous step](#webclient_deployment) `pyrrha-consensus` submodule should be properly configured and updated.  

Contracts addresses in the file `./config/index.js` (`rinkeby` and `rinkebyinfura` sections) should be replaced with new addresses obtained during [contracts deployment step](#contracts_deployment).  

All required changes should be merged and pushed into a proper repository branch (master or staging) with a proper version tag (`git tag <version_tag>`).

Continuous integration (CI) system is configured to make a proper type of deployment in depending on the version tag. Production versions are looks like `v1.6.0`, and staging versions look like `v1.5.9-staging`.  

Deployment will start automatically after a new tag is pushed to the repository with command `git push --tags`. 

Deployment process can be monitored at [https://travis-ci.org/pandoraboxchain/pyrrha-boxproxy](https://travis-ci.org/pandoraboxchain/pyrrha-boxproxy)

## <a name="boxplorer_deployment"></a>Boxplorer deployment  
Boxplorer repository: [https://github.com/pandoraboxchain/pyrrha-boxplorer](https://github.com/pandoraboxchain/pyrrha-boxplorer)

All required changes should be merged and pushed into a proper repository branch (master or staging) with a proper version tag (`git tag <version_tag>`).

Continuous integration (CI) system is configured to make a proper type of deployment in depending on the version tag. Production versions are looks like `v1.6.0`, and staging versions look like `v1.5.9-staging`.  

Deployment will start automatically after a new tag is pushed to the repository with command `git push --tags`.  

Deployment process can be monitored at [https://travis-ci.org/pandoraboxchain/pyrrha-boxplorer](https://travis-ci.org/pandoraboxchain/pyrrha-boxplorer)

## <a name="pynode_deployment"></a>Pynode & workers deployment  
[TBD]

## Continous integration configuration
CI control panel: [https://travis-ci.org/pandoraboxchain](https://travis-ci.org/pandoraboxchain)  