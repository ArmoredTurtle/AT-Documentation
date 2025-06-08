# Contributing to This Project

Thanks for your interest in contributing! Whether it's a bug report, new feature, or documentation fix, your help is welcome and appreciated.

## Getting Started

1. **Fork the repository** and create your branch from `DEV` branch.
2. Make your changes, following the guidelines below.
3. Submit a [pull request](https://github.com/ArmoredTurtle/AT-Documentation/pulls).

**NOTE** All pull requests should be up to date with the latest changes from the `DEV` branch and PRs should be opened against the `DEV` branch.

The only exception to this would be if the changes are in the current deployed version of the documentation. In that case, the PR should be opened against the `main` branch.

## Guidelines

### Code Style

- Python code should follow [PEP 8](https://peps.python.org/pep-0008/) as closely as possible.
- Use meaningful commit messages.
- Keep changes focused—avoid mixing unrelated fixes or features.

### Scripts

- Bash scripts should be POSIX-compliant where practical.
- Include a `#!/bin/bash` or `#!/usr/bin/env bash` shebang as appropriate.
- Add inline comments to explain non-obvious logic.

### Testing

- If applicable, test your changes with your virtual environment.
- Make sure your code doesn't introduce errors or break existing functionality.
- While formal unit tests may not be in place yet, please validate your changes manually.

### Pull Requests

- Clearly describe what your PR does and why it’s useful.
- Link to any relevant issues.
- Smaller PRs are easier to review and merge—break large changes into smaller steps if you can.

## Bug Reports & Feature Requests

- Use GitHub [Issues](https://github.com/ArmoredTurtle/AT-Documentation/issues) to report bugs or request features.
- When reporting a bug, please include:
  - A description of the issue
  - Steps to reproduce
  - Any relevant logs or configuration

## Virtual environment

It is recommended to set up a python virtual environment to keep installed dependencies isolated from your system dependencies.

You can do this by running the following:

## Set up environment (venv)

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

## Run locally:
```bash
mkdocs serve
```

## Build site:
```bash
mkdocs build
```

This repo also utilizes the `AFC-Klipper-Add-On` as a submodule. Ensure that it is properly initialized and updated.