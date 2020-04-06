# Kubernetes From Scratch

## Requirements

Install the following tools in your compute:

- **Vagrant:** https://www.vagrantup.com/
- **VirtualBox:** https://www.virtualbox.org/
- **Ansible:** https://www.ansible.com/

```shell
brew cask install vagrant
brew cask install virtualbox
pip3 install --user ansible
```

## Get Started

### Create virtual machines

```shell
vagrant up
```

### Creating a Non-Root User on All Remote Servers

```shell
ansible-playbook ./provision/initial.yml -i ./provision/inventories/vagrant/hosts.yml
```
