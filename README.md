# About

Github Action to build the VEBA Appliance from a self-hosted runner using `BASH`.  This
is intended to be used to build the VEBA Appliance in a private lab environment via a self-hosted runner.  The lab environment will also need an ESX host and vCenter.  The output of this action will be the VEBA appliance OVA copied to the runner.

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

# Self-hosted Runner Build Notes
Built on Ubuntu 20.04

## Install open-vmtools
  - apt update
  - apt upgrade
  - apt install open-vm-tools
  - reboot

## Install ssh
  - apt install openssh-server
  - systemctl enable ssh
  - systemctl start ssh

## Create veba user to run self hosted runner from
  - adduser veba
  - add to sudoers and other groups
  - install the self-hosted runner
  - start the self-hosted runner as a service
    https://docs.github.com/en/actions/hosting-your-own-runners/configuring-the-self-hosted-runner-application-as-a-service
  - create key and add public to esx host authorized keys file

## Install curl
  - apt install curl

## Install ovftool
  - chmod +755 VMware-ovftool-4.4.3-18663434-lin.x86_64.bundle
  - sudo ./VMware-ovftool-4.4.3-18663434-lin.x86_64.bundle --console --required --eulas-agreed

## Install Docker
  - https://docs.docker.com/engine/install/ubuntu/

## Install OpenFaas Client
  - curl -sSL https://cli.openfaas.com | sudo sh

## Install packer
  - curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
  - sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
  - sudo apt-get update && sudo apt-get install packer

## Install jq
  - sudo apt install -y jq

## Install powershell on runner
  - wget -q https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
  - sudo dpkg -i packages-microsoft-prod.deb
  - apt-get update -y
  - sudo apt-get install powershell -y

## Install PowerCLI (logged into powershell console)
  - Install-Module -Name VMware.PowerCLI
  - Set-PowerCLIConfiguration -InvalidCertificateAction Ignore

## Create Auth file
Make a directory /home/veba/auth and put a copy of photon-builder.json in it with the correct
ESX host and vCenter information.  This will be read by the runner and used to build the
VEBA appliance.
