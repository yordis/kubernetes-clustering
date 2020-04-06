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

### Installing Kubernetetes Dependencies

```shell
ansible-playbook ./provision/kube-dependencies.yml -i ./provision/inventories/vagrant/hosts.yml
```

### Setting Up the Master Node

```shell
ansible-playbook ./provision/master.yml -i ./provision/inventories/vagrant/hosts.yml
```

### Setting Up the Worker Nodes

```shell
ansible-playbook ./provision/workers.yml -i ./provision/inventories/vagrant/hosts.yml
```