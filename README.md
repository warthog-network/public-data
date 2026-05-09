# Warthog Public Data

This repository is a collection of public data on Warthog, including:
- Public nodes for wallet usage without running your own node
- Address tag information

The public nodes are run by the community. You are welcome to add your node to help the network.

## Data Files

- [data/legacy-nodes.csv](data/legacy-nodes.csv) - Legacy public node URLs and owners
- [data/defi-nodes.csv](data/defi-nodes.csv) - DeFi public node URLs and owners
- [data/addresses.csv](data/addresses.csv) - WART address tags
- [data/sha256t-hashrates.csv](data/sha256t-hashrates.csv) - GPU hashrates for Janushash
- [data/verushash2_2-hashrates.csv](data/verushash2_2-hashrates.csv) - CPU hashrates for Janushash

## API Endpoints

Once deployed, the following JSON endpoints are available:

| Endpoint | Description |
|----------|-------------|
| `/legacy-nodes.json` | Legacy public nodes with URL and owner |
| `/defi-nodes.json` | DeFi public nodes with URL and owner |
| `/addresses.json` | WART address tags |

### Example Response

**GET /legacy-nodes.json**
```json
{
  "nodes": [
    {"url": "http://65.87.7.86:3001", "owner": "pumbaa"},
    {"url": "http://185.209.228.16:3001", "owner": "blu & EU"}
  ]
}
```

**GET /defi-nodes.json**
```json
{
  "nodes": []
}
```

**GET /addresses.json**
```json
{
  "addresses": [
    {"address": "95ae6efb2f4fe5e4fd3a5b21df7f755f878383610505fe64", "tag": "Herominers"}
  ]
}
```

## Janushash Algorithm

Janushash combines GPU (sha256t) and CPU (verushash2_2) hashrates using the formula:

```
Janusscore = round(gpu * 10 * ((0.005 + cpu/gpu)^0.3 - 0.005^0.3) / 3)
```

Where:
- `gpu` = sha256t hashrate (mh/s)
- `cpu` = verushash hashrate (mh/s)

An interactive calculator is available at the root URL.

## Contributing

This repository hosts public data for the Warthog community. Everyone is welcome to make a PR.

Discussion: https://discord.com/invite/QMDV8bGTdQ

### Adding a Node

1. Fork this repository
2. Edit the relevant CSV file in `data/`:
   - `legacy-nodes.csv` - for legacy nodes
   - `defi-nodes.csv` - for DeFi nodes
3. Add a new line: `url,owner,comment`
4. Submit a PR to `master`

### Adding an Address Tag

1. Fork this repository
2. Edit `data/addresses.csv`
3. Add a new line: `address,tag,comment`
4. Submit a PR to `master`