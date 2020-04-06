# Ubuntu with Virtualbox

This is an example on how to create a Kubernetes cluster using Ubuntu and
Virtualbox.

## Get Started

Make sure to run all the commands from the right folder

```shell
cd <repo-root>/clusters/ubuntu-virtualbox
```

### Create virtual machines

We will use Vagrant to setup 3 virtual machines, 1 master node and 2 worker
nodes.

```shell
vagrant up
```

Now you have 3 virtual machines available:

- **192.168.50.4:** Master Node
- **192.168.50.5:** Worker Node 1
- **192.168.50.6:** Worker Node 2

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
