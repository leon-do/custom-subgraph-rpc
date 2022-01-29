# Subgraph OpenZepplin

Subgraph Template for ERC-1155, ERC-20, ERC-721

Source: https://docs.openzeppelin.com/subgraphs/0.1.x/

## Steps

clone and install

```
npm install
```

edit `configs/config.json`

```json
{
  "output": "generated/token.",
  "chain": "rinkeby",
  "datasources": [{ "address": "0xA3B26327482312f70E077aAba62336f7643e41E1", "module": ["erc20"] }]
}
```

compile

`npm run compile`
