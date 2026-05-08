# Warthog Public Nodes

**WARNING**: Public nodes are provided by community members. You can use each node for different wallets. However, we advise users against mining through public nodes.

## Data Files

- `data/nodes.csv` - Public node URLs and owners
- `data/addresses.csv` - WART addresses with tags

## API Endpoints

Once deployed, the following JSON endpoints are available:

| Endpoint | Description |
|----------|-------------|
| `/nodes.json` | Public nodes with URL and owner |
| `/addresses.json` | WART addresses with tags |

### Example Response

**GET /nodes.json**
```json
{
  "nodes": [
    {"url": "http://65.87.7.86:3001", "owner": "pumbaa"},
    {"url": "http://185.209.228.16:3001", "owner": "blu & EU"}
  ]
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