# Local Hyperledger Besu Blockchain

## Basic Setup

1. Generate the keys and the basic genesis.json file for each node.

```bash
docker-compose up -f docker-compose.setup.yml
```

1. Copy the key pairs from the newly created /networkFiles/keys/{node eth adress} folder to the directory of each node.
2. Copy the "extraData":{} portion of /networkFiles/genesis.json to ./setupfiles/genesis.json. and replace the existing "extraData":{}. This data contains the eth addresses of the initial validator nodes in the network. So each time you generate new keys you have to update this value
3. Spin up the entire stack and note the *enode url* of node-1. It will be posted in the console at startup.

```bash
docker-compose up
```

1. Each node has a config file in its directory. Paste the enode url in the bootnode section of the config.toml file in each directory. It should look something like this:

```bash
bootnodes=["enode://bbdf05994421e01d21d5a8cc5c00bf568e393ea7cf3473aae687ddaef378397dfaa1e2ce607b2e6e6de8b8d6db474dc4fcb2375b4442f6f42b0d4a07a309e96e@127.0.0.1:30303"]
```

1. Restart the stack. So it knows the bootnode of the network is node-1.

> This is where stuff stops working. Perhaps the network section of the config file has issues. Or the config file itself is wrong. Something there i guess. Or the docker-compose file has the wrong ports or something idk.

## Modifying parameters

### /setupFiles/tempateGenesis.json

This file is the template for generating the "extradata" for the main genesis file. It however has a bunch of other parameters that aren't carried over for some reason. This is why we only copy the "extradata" from the generated file to our own Genesis file.

### /setupFiles/genesis.json

This is the genesis file for each node on the network. It declares the settings of the blockchain such as well as pre-funded accounts and the consensus mechanism used by the nodes.

**Prefunding accounts**

If you want to pre-fund some accounts you can do so by modifying the "alloc" section of the genesis file. Simply copy the account address of the account you want to fund and paste it without the "0x" prefix in the file. It should look something like this:

```json
"alloc" : {
    "815e4B005Bf2601A886756d8da7A508E526E5AFa": {
      "balance":"1000000000000000000000000"
    }
  },
```

**Other variables**

The meaning and use of the other variables in the genesis file can be found here:

(**disclaimer!** This is not the Official Besu documentation so some commands might not work outside of the genesis file.)

[Private Networks](https://geth.ethereum.org/docs/interface/private-network)

The original documentation for hyperledger Besu can be found here:

[Hyperledger Besu Enterprise Ethereum client - Hyperledger Besu](https://besu.hyperledger.org/en/stable/)

### /{node-name}/config.toml

This config file sets the environment variables for each node. The reference for this file can be found here:

[Options - Hyperledger Besu](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/)

The reference initially displays the settings in command format. However you can select the *example configuration file* setting above the command to show it in config file format.