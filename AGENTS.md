# AGENTS.md

Static public-data repo for the Warthog network, served at `data.warthog.network`. **Part of a multi-repo project hub** — see `../HUB.md` (at the hub root) for the full sub-repo layout, public APIs, and cross-repo source-of-truth mapping.

## What this repo is

`master` holds source CSVs + an `index.html` + a `CNAME`. Every push to `master` triggers `.github/workflows/deploy.yml`, which validates assets, converts CSVs to JSON, and force-pushes the result to the `gh-pages` branch.

**Never edit generated `*.json` files or the `gh-pages` branch directly** — they are overwritten by CI. Edit the CSVs in `data/` and open a PR against `master`.

## This repo is the canonical source for `data.warthog.network`

The JSON served from `data.warthog.network` is the **canonical source of truth** for the following 7 endpoints (per HUB.md `### Public Data API`):

| Endpoint | CSV source | JSON shape |
|----------|-----------|------------|
| `legacy-nodes.json` | `data/legacy-nodes.csv` | `{"nodes": [{"url", "name"}, ...]}` |
| `defi-nodes.json` | `data/defi-nodes.csv` | `{"nodes": [{"url", "name"}, ...]}` |
| `addresses.json` | `data/addresses.csv` | `{"addresses": [{"address", "tag"}, ...]}` |
| `sha256t-hashrates.json` | `data/sha256t-hashrates.csv` | `[{manufacturer, model, hashrate_mh_s}, ...]` |
| `verushash2_2-hashrates.json` | `data/verushash2_2-hashrates.csv` | `[{manufacturer, model, hashrate_mh_s}, ...]` |
| `assets.json` | `data/assets/` directories | `[{hash, name, ticker}, ...]` |
| `assets/<hash>/info.json` | `data/assets/<hash>/info.json` | `{hash, name, ticker, description, website?, telegram?, discord?, twitter?}` |

The endpoint list is mirrored in HUB.md — if you add/remove an endpoint, update both.

## Network state (legacy vs DeFi nodes)

Warthog runs two networks with different feature sets. This repo keeps separate node lists:

| File | Network | Branch |
|------|---------|--------|
| `legacy-nodes.csv` | **Mainnet** (wartTransfer only) | `core/master` |
| `defi-nodes.csv` | **Testnet** (all 7 DeFi tx types, FBM, pools) | `core/defi` |

Do not mix entries between the two files. A node running `core/defi` belongs in `defi-nodes.csv`; a node running `core/master` belongs in `legacy-nodes.csv`.

## Key constants (for context)

These are the constants the data in this repo relates to. Update if they change in `core/defi/meson.build` or related sources.

- **Chain ID:** `0x539` (decimal `1337`)
- **Block version:** `4`
- **Currency:** `1 WART = 100,000,000 E8` (8 decimals)
- **Block time:** `20s`
- **Initial reward:** `3 WART`

## Known inconsistencies (per HUB.md)

Node URL lists live in **five divergent locations** across the project. This repo's CSVs are the canonical source for the live `data.warthog.network` JSON, but other repos keep their own copies that can drift:

- `warthog-ts/src/KNOWN_NODES` (used by SDK clients)
- `mobile-wallet` (uses KNOWN_NODES via warthog-ts)
- `node-gui` (uses KNOWN_NODES via warthog-ts)
- `public-data/data/legacy-nodes.csv` (this repo — mainnet)
- `public-data/data/defi-nodes.csv` (this repo — testnet)

If a node URL is wrong, fix the CSV here **and** check that the change is consistent with the consumers. Some entries here are marked with `comment` values like `Dead 1` — these are visual hints only, the `comment` column is stripped on conversion.

## Consumers of this data

The JSON files served by `data.warthog.network` are consumed by:

- **`warthog-ts` SDK** — used as fallback node lists.
- **`mobile-wallet`** (React Native) — node picker.
- **`node-gui`** (dashboard) — node picker.
- **`website`** (`warthog.network`) — hero/market sections, links page.
- **`docs/guides/public-data/index.md`** — documents the API shape; **must match what this repo serves**.

If you change a JSON shape here, update the docs page and any consumer that parses the field. The Python in `deploy.yml` is the single source of truth for the conversion.

## Layout

```
data/
  legacy-nodes.csv         # public legacy wallet/RPC nodes (mainnet)
  defi-nodes.csv           # public DeFi nodes (testnet)
  addresses.csv            # WART address tags
  sha256t-hashrates.csv    # GPU hashrates for Janushash
  verushash2_2-hashrates.csv # CPU hashrates for Janushash
  assets/<64-hex-hash>/
    info.json              # required: hash, name, ticker, description; optional: website, telegram, discord, twitter
    logo.<ext>            # optional, image/png or image/jpeg, exactly 250x250 px
    banner.<ext>          # optional, image/png or image/jpeg, exactly 600x200 px
index.html                 # landing page + Janushash calculator
CNAME                      # data.warthog.network (do not delete or rename)
.github/workflows/deploy.yml
```

## CSV schemas (exact headers)

`legacy-nodes.csv`, `defi-nodes.csv`: `url,name,comment`
`addresses.csv`: `address,tag,comment`
`sha256t-hashrates.csv`, `verushash2_2-hashrates.csv`: `manufacturer,model,hashrate mh/s`

- The hashrate header literally is `hashrate mh/s` (space + slash). The deploy script reads it as `row['hashrate mh/s']` and parses with `float()` — values must be numeric.
- The `comment` column is stripped on conversion, so use it freely for maintainer notes (e.g. `Dead 1`). It does not affect the API output and there is no convention for filtering on it.
- Trim whitespace; do not leave stray quotes.

## Assets

- Each asset directory must be named with the full 64-hex-character asset hash and must contain `info.json` with non-empty `hash`, `name`, `ticker` (max 5 chars per README), `description`; `website`, `telegram`, `discord`, `twitter` are optional URLs (placeholders `https://`, `https://t.me/`, `https://discord.com/`, `https://x.com/` respectively). The `hash` field equals the directory name.
- Asset hashes must match the on-chain asset registration. The hash is a 32-byte value (64 hex chars), inherited from `GenericHash` per `core/defi/src/shared/src/defi/token/asset.hpp`.
- Asset hashes must be registered with the team on Discord before adding. Do not invent hashes.
- CI hard-fails the deploy if any `data/assets/*/` directory is missing `info.json`, has invalid JSON, or is missing a required field. See `validate_asset` in `.github/workflows/deploy.yml`.

## Generated JSON shapes (for reference)

- `legacy-nodes.json`, `defi-nodes.json` → `{"nodes": [{"url":..., "name":...}, ...]}`
- `addresses.json` → `{"addresses": [{"address":..., "tag":...}, ...]}`
- `sha256t-hashrates.json`, `verushash2_2-hashrates.json` → flat array `[{manufacturer, model, hashrate_mh_s}, ...]`
- `assets.json` → `[{hash, name, ticker}, ...]`
- `assets/<hash>/info.json` → `{hash, name, ticker, description, website?, telegram?, discord?, twitter?}`

These shapes are set by the Python in `deploy.yml`; if you change them, update the script and any consumers in lockstep.

## Brand identity

`index.html` and any local styling should use the Warthog brand:

- **Primary yellow:** `#FDB913` (Pantone `1375 C`). Logo SVG tokens: `st0` `#F8F8F9`, `st1` `#FDB913`, `st2` `#231F20`, `st3` `#FFFFFF`.
- **Logos:** pick from `../brand-kit/logo/<Variant> <ColorScheme>.svg` — flat directory, no subfolders. Variants: `Full`, `Circle`, `Short`, `Stacked`, `Ticker`. Color schemes: `Black`, `Yellow`, `White` (Short/Ticker only), `BW`, `Negative`, `Negative Yellow`.
- **Font:** Montserrat only. The brand-kit provides `fonts/Montserrat.zip`.
- **Full reference:** `../brand-kit/AGENTS.md` and `../brand-kit/README.md` (palette, logo variants, SVG tokens, naming conventions). V01.A, 2023·2024, by BalkyBot. See HUB.md `## Brand Kit` for the cross-repo summary.

## Local verification

There is no test/lint/typecheck. To preview the conversion locally, run the same Python the workflow runs:

```bash
python3 .github/workflows/deploy.yml  # not actually executable; copy the heredoc
```

Easier: open `.github/workflows/deploy.yml`, copy the `python3 << 'EOF' ... EOF` block into a temp file, run it from the repo root, and inspect the produced `*.json` files. Then `git checkout --orphan preview` (or just `rm` them) before committing — they must not end up on `master`.

For a quick CSV sanity check:

```bash
python3 -c "import csv,json; [print(json.dumps(list(csv.DictReader(open('data/sha256t-hashrates.csv')))))][:1]"
```

## PR / deploy flow

- Branch from and target `master`. Push to `master` triggers `.github/workflows/deploy.yml`, which:
  1. validates every `data/assets/*/info.json`,
  2. converts the five CSVs to JSON,
  3. force-pushes `index.html`, `CNAME`, the JSON outputs, and `assets/` to `gh-pages`.
- No release process, no versioning, no package manager.
- Discussion / new-asset registration: Discord `https://discord.com/invite/QMDV8bGTdQ`.

## Janusscore formula

Defined in `index.html` and must stay in sync with the Warthog docs:

```
Janusscore = round(gpu * 10 * ((0.005 + cpu/gpu)**0.3 - 0.005**0.3) / 3)
```

Both inputs are in mh/s. Do not change without coordinating with the docs site.
