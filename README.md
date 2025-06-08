## Useful links:

Mkdocs: https://www.mkdocs.org/user-guide/writing-your-docs/  
Material for Mkdocs: https://squidfunk.github.io/mkdocs-material/

## Set up environment (venv)

Run these from inside the `_afc` dir:

Install `uv`

https://github.com/astral-sh/uv

Create venv

```bash
uv venv
source .venv/bin/activate
```

Sync lockfile

```bash
uv sync
```

To init the git submodules, run the following:

```bash
git submodule init
git pull --recurse-submodules
git submodule update --recursive --remote
```

This needs to be done before building the documentation to ensure that the latest `AFC-Klipper-Add-On` stuff is 
present. 

The documentation is available in the live site at https://www.armoredturtle.xyz/docs/.
## Alternative setup

Alternatively, you can use `mise` to set up the environment. A `mise.toml` file is provided with the repo for this purpose.

## Run locally:
```bash
mkdocs serve
```

## Build site:
```bash
mkdocs build
```

This repo also utilizes the `AFC-Klipper-Add-On` as a submodule. Ensure that it is properly initialized and updated.