# Warthog Public Data

This repository is a collection of public data on Warthog, including:
- Public nodes for wallet usage without running your own node
- Address tag information
- Asset metadata for tokens on the Warthog chain
- Hashrates from the community ([google sheets](https://docs.google.com/spreadsheets/d/1cQqlGm0wrrnlcPfjf0Cbsp-WEmtVAqVK_btHhbnhWao/) and [this post](https://telegra.ph/Sochetanie-hehshrejtov-CPUGPU-Warthog-mnogo-Xeonov-iz-moego-opyta-11-26)).

The public nodes are run by the community. You are welcome to add your node to help the network.

## Data Files

The following data files are exported to API endpoints on [data.warthog.network](https://data.warthog.network):

| File | Exported Endpoint | Description |
|------|-------------------|-------------|
| [data/legacy-nodes.csv](data/legacy-nodes.csv) | [/legacy-nodes.json](https://data.warthog.network/legacy-nodes.json) | Legacy public node URLs |
| [data/defi-nodes.csv](data/defi-nodes.csv) | [/defi-nodes.json](https://data.warthog.network/defi-nodes.json) | DeFi public node URLs |
| [data/addresses.csv](data/addresses.csv) | [/addresses.json](https://data.warthog.network/addresses.json) | WART address tags |
| [data/sha256t-hashrates.csv](data/sha256t-hashrates.csv) | [/sha256t-hashrates.json](https://data.warthog.network/sha256t-hashrates.json) | GPU hashrates for Janushash |
| [data/verushash2_2-hashrates.csv](data/verushash2_2-hashrates.csv) | [/verushash2_2-hashrates.json](https://data.warthog.network/verushash2_2-hashrates.json) | CPU hashrates for Janushash |
| [data/assets/{asset_hash}/](data/assets/) | [/assets.json](https://data.warthog.network/assets.json) | List of all assets for autocompletion |
| [data/assets/{asset_hash}/info.json](data/assets/) | [/assets/{hash}/info.json](https://data.warthog.network/assets/0000000000000000000000000000000000000000000000000000000000000000/info.json) | Asset metadata |

## API Endpoints

Once deployed, the following JSON endpoints are available:

| Endpoint | Description |
|----------|-------------|
| `/legacy-nodes.json` | Legacy public nodes with URL and name |
| `/defi-nodes.json` | DeFi public nodes with URL and name |
| `/addresses.json` | WART address tags |
| `/sha256t-hashrates.json` | GPU hashrates for Janushash |
| `/verushash2_2-hashrates.json` | CPU hashrates for Janushash |
| `/assets.json` | List of all assets with hash, name, ticker for autocompletion |
| `/assets/{hash}/info.json` | Asset metadata (name, ticker, description, url) |

### Example Response

**GET /assets.json**
```json
[
  {"hash": "0000000000000000000000000000000000000000000000000000000000000000", "name": "WART", "ticker": "WART"}
]
```

**GET /assets/0000000000000000000000000000000000000000000000000000000000000000/info.json**
```json
{
  "name": "WART",
  "ticker": "WART",
  "description": "WART is the native token in the Warthog ecosystem. Fees are paid in it and every asset is traded against WART.",
  "url": "https://warthog.network"
}
```

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

### Adding an Asset

Contact the team on Discord to have your asset hash registered.

Once registered, you can add your token info:

1. Fork this repository
2. Create directory: `data/assets/{asset_hash}/`
3. Add `info.json` with the following structure:
   ```json
   {
       "name": "Asset Long Name",
       "ticker": "TICK",
       "description": "Description of the asset."
   }
   ```
   - `name`: Long name (required)
   - `ticker`: Short ticker symbol (required, max 5 characters)
   - `description`: Asset description (required)
   - `url`: Website URL (optional)
4. Optionally add `image.png` (valid PNG file)
5. Submit a PR to `master`