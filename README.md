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
