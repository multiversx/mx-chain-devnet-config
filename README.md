<div style="text-align:center">
  <img
  src="https://raw.githubusercontent.com/multiversx/mx-chain-go/master/multiversx-logo.svg"
  alt="MultiversX">
</div>
<br>

<br>

[![](https://img.shields.io/badge/made%20by-MultiversX-blue.svg?style=flat-square)](http://multiversx.com/)
[![](https://img.shields.io/badge/project-MultiversX%20Devnet-blue.svg?style=flat-square)](http://multiversx.com/)

# mx-chain-devnet-config for the official devnet (developer testnet)

MultiversX devnet configuration files used in conjunction with mx-chain-go project. 
For more info how to connect to the devnet, please check [docs.multiversx.com](https://docs.multiversx.com/validators/nodes-scripts/config-scripts/)

## run an MultiversX observer/validator with docker

### build docker image
```docker image build . -t mutltiversx-node-image-last -f ./docker/Dockerfile```

### run node with docker
```
CONFIG_FOLDER=path/to/folder/with/pem/file
docker run --mount type=bind,source=${CONFIG_FOLDER}/,destination=/data mutltiversx-node-image-last --validator-key-pem-file="/data/validatorKey.pem" --log-level *:DEBUG
```

