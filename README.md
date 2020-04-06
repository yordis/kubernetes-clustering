# Kubernetes Clustering

Example of setting up Kubernetes clusters.

## Requirements

- MacOS: Operation System
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

## Get Started

### Create virtual machines

```shell
vagrant up
```

### Add IPs to ssh known hosts

```shell
ssh-keyscan -H 192.168.50.4 >> ~/.ssh/known_hosts
ssh-keyscan -H 192.168.50.5 >> ~/.ssh/known_hosts
ssh-keyscan -H 192.168.50.6 >> ~/.ssh/known_hosts
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

## Deploying an application

```shell
ssh ubuntu@192.168.50.4
```

```shell
kubectl create deployment nginx --image=nginx
kubectl expose deploy nginx --port 80 --target-port 80 --type NodePort
kubectl get services
```

Visit

```text
http://worker_ip:nginx_port
```
