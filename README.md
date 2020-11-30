# Kubernetes Clustering

Example of setting up Kubernetes clusters.

> This is not an intent to be production ready, please consider
> the usage of https://github.com/ilkilab/agorakube in
> production instead.

## Requirements

These may be the list of required tools to run all the examples:

- **MacOS:** Operation System
- **Homebrew:** https://brew.sh/ install multiple tools
- **Vagrant:** https://www.vagrantup.com/ virtual machine provisioning
- **VirtualBox:** https://www.virtualbox.org/ for virtualization
- **Ansible:** https://www.ansible.com/ for Operation System provisioning

### Install Homebrew

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### Install Homebrew packages

```shell
brew cask install vagrant
brew cask install virtualbox
```

### Install Ansible

```shell
pip3 install --user ansible
```

## What is next?

Check the some of the examples of how to setup a Kubernetes cluster:

- [Ubuntu and VirtualBox](./clusters/ubuntu-virtualbox/README.md)
