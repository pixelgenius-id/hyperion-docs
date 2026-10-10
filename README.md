# hyperion-docs (Vexanium)

Documentation for [Hyperion History API](https://github.com/pixelgenius-id/hyperion-history-api) on the Vexanium blockchain.

Hyperion was created by [EOS Rio](https://eosrio.io/) and is further developed for the Vexanium blockchain by [Pixelgenius](https://github.com/pixelgenius-id). This repository is a fork of [eosrio/hyperion-docs](https://github.com/eosrio/hyperion-docs).

Public URL: https://pixelgenius-id.github.io/hyperion-docs/

### Install dependencies

```shell
sudo apt install python3 python-is-python3 python3-pip libcairo2-dev libfreetype6-dev libffi-dev libjpeg-dev libpng-dev libz-dev pngquant python3.12-venv
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

### Windows

```cmd
python -m venv .venv
.\.venv\Scripts\activate.bat
```
Troubleshoot installing cairo on Windows:
https://squidfunk.github.io/mkdocs-material/plugins/requirements/image-processing/#cairo-graphics-windows

### Run development server

```bash
source .venv/bin/activate
mkdocs serve
# Alternatively
npm run start
```

### Build static site

```bash
source .venv/bin/activate
mkdocs build
# Alternatively
npm run build
```

### Deploy

Every push to `master` builds the site and publishes it to GitHub Pages through `.github/workflows/docs.yml`.
