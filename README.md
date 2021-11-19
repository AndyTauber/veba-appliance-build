# About

Github Action to build the VEBA Appliance from a self-hosted runner using `BASH`.

# Usage

## Simple with Defaults

```yaml
name: Build VEBA Appliance

on:
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: runs-on: [self-hosted, linux]
    steps:
      - name: Check WIP in PR Title
        uses: AndyTauber/veba-appliance-build@v1
```
