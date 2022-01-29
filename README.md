# Subgraph OpenZepplin

Subgraph Template for ERC-1155, ERC-20, ERC-721

Source: https://docs.openzeppelin.com/subgraphs/0.1.x/

## Set up Repo

clone and install

```
npm install
```

edit `configs/config.json`

```json
{
  "output": "generated/oz-erc1155-template0.",
  "chain": "rinkeby",
  "datasources": [{ "address": "0xF37eE89bb34Cf993457bf52d7598d4085e0721eD", "startBlock": 10072962, "module": ["erc1155", "ownable", "accesscontrol"] }]
}
```

Compile

`npx graph-compiler --config configs/config.json --include node_modules/@openzeppelin/subgraphs/src/datasources --export-schema --export-subgraph`

## Deploy to Hosted Service

Visit https://thegraph.com/hosted-service/dashboard

Copy Access Token and Paste

![](https://user-images.githubusercontent.com/59702430/151644044-ba03a2e8-81e0-471d-88eb-23a7c605b94b.png)

Click `Add Subgraph`

![](https://user-images.githubusercontent.com/59702430/151643815-925d25fc-6cbb-4bb2-bbe0-b866f175eeba.png)


Name should match `config.json`

![](https://user-images.githubusercontent.com/59702430/151643848-b0a5c254-0c6f-4d1e-9174-49cb12bbd074.png)

`graph deploy --ipfs https://api.thegraph.com/ipfs/ --node https://api.thegraph.com/deploy/ ln-state/oz-erc1155-template0 ./generated/oz-erc1155-template0.subgraph.yaml`

query the graph

```graphql
{
  account(id: "0xdd4c825203f97984e7867f11eecc813a036089d1") {
    ERC1155balances {
      valueExact
      token {
        identifier
      }
    }
  }
}
```

![](https://user-images.githubusercontent.com/59702430/151644243-9305b270-ea6a-49e1-bd9f-9f6e31cf9ff0.png)

## Local Node

`git clone https://github.com/graphprotocol/graph-node/`

`cd graph-node/docker`

edit `docker-compose.yaml` to correct RPC

```yaml
ethereum: 'mainnet:http://172.18.0.1:8545'
to
ethereum: 'mainnet:https://stardust.metis.io/?owner=588'
```

`./setup.sh`

`docker-compose up`

compile subgraph

`npx graph-compiler --config configs/config.json --include node_modules/@openzeppelin/subgraphs/src/datasources --export-schema --export-subgraph`

create

`graph create generated/erc1155-test --node http://127.0.0.1:8020`

deploy locally

`graph deploy --ipfs http://localhost:5001 --node http://localhost:8020 generated/erc1155-test ./generated/erc1155-test.subgraph.yaml`

visit

http://localhost:8000/subgraphs/name/generated/erc1155-test/graphql

query

```
{
  account(id: "0xdd4c825203f97984e7867f11eecc813a036089d1") {
    ERC1155balances {
      valueExact
      token {
        identifier
      }
    }
  }
}
```