# Warthog Public Data

This repository is a collection of public data on Warthog, including:
- Public nodes for wallet usage without running your own node
- Address tag information

The public nodes are run by the community. You are welcome to add your node to help the network.

## Data Files

The following data files are exported to API endpoints on [data.warthog.network](https://data.warthog.network:

| File | Exported Endpoint | Description |
|------|-------------------|-------------|
| [data/legacy-nodes.csv](data/legacy-nodes.csv) | [/legacy-nodes.json](https://data.warthog.network/legacy-nodes.json) | Legacy public node URLs
| [data/defi-nodes.csv](data/defi-nodes.csv) | [/defi-nodes.json](https://data.warthog.network/defi-nodes.json) | DeFi public node URLs
| [data/addresses.csv](data/addresses.csv) | [/addresses.json](https://data.warthog.network/addresses.json) | WART address tags
| [data/sha256t-hashrates.csv](data/sha256t-hashrates.csv) | [/sha256t-hashrates.json](https://data.warthog.network/sha256t-hashrates.json) | GPU hashrates for Janushash
| [data/verushash2_2-hashrates.csv](data/verushash2_2-hashrates.csv) | [/verushash2_2-hashrates.json](https://data.warthog.network/verushash2_2-hashrates.json) | CPU hashrates for Janushash

## Contributing

This repository hosts public data for the Warthog community. Everyone is welcome to make a PR.

Discussion: https://discord.com/invite/QMDV8bGTdQ

### Adding a Node

1. Fork this repository
2. Edit the relevant CSV file in `data/`:
   - `legacy-nodes.csv` - for legacy nodes
   - `defi-nodes.csv` - for DeFi nodes
3. Add a new line: `url,name,comment`
4. Submit a PR to `master`

### Adding an Address Tag

1. Fork this repository
2. Edit `data/addresses.csv`
3. Add a new line: `address,tag,comment`
4. Submit a PR to `master`
