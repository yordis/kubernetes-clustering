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
ansible-playbook ./provision/1.initial.yml -i ./provision/inventory/hosts.yml
```

- Creates the non-root user `ubuntu`.
- Configures the `sudoers` file to allow the `ubuntu` user to run sudo commands
  without a password prompt.
- Adds the public key in your local machine (usually `~/.ssh/id_rsa.pub`) to the
  remote `ubuntu` user’s authorized key list. This will allow you to SSH into
  each server as the `ubuntu` user.

### Installing Kubernetetes Dependencies

```shell
ansible-playbook ./provision/2.kube-dependencies.yml -i ./provision/inventory/hosts.yml
```

- Installs Docker, the container runtime.
- Installs `apt-transport-https`, allowing you to add external HTTPS sources to
  your APT sources list.
- Adds the Kubernetes APT repository’s apt-key for key verification.
- Adds the Kubernetes APT repository to your remote servers’ APT sources list.
- Installs `kubelet` and `kubeadm`.

### Setting Up the Master Node

```shell
ansible-playbook ./provision/3.master.yml -i ./provision/inventory/hosts.yml
```

- The first task initializes the cluster by running `kubeadm init`. Passing the
  argument `--pod-network-cidr=10.244.0.0/16` specifies the private subnet that
  the pod IPs will be assigned from. Flannel uses the above subnet by default;
  we’re telling kubeadm to use the same subnet.
- The second task creates a `.kube` directory at `/home/ubuntu`. This directory
  will hold configuration information such as the admin key files, which are
  required to connect to the cluster, and the cluster’s API address.
- The third task copies the `/etc/kubernetes/admin.conf` file that was generated
  from `kubeadm init` to your non-root user’s home directory. This will allow
  you to use `kubectl` to access the newly-created cluster.
- The last task runs `kubectl apply` to install Flannel. The `kube-flannel.yml`
  file contains the descriptions of objects required for setting up Flannel in
  the cluster.

### Setting Up the Worker Nodes

```shell
ansible-playbook ./provision/4.workers.yml -i ./provision/inventory/hosts.yml
```

- The first play gets the join command that needs to be run on the worker nodes.
  This command will be in the following format: `kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>`. Once
  it gets the actual command with the proper token and hash values, the task
  sets it as a fact so that the next play will be able to access that info.
- The second play has a single task that runs the join command on all worker
  nodes. On completion of this task, the two worker nodes will be part of the
  cluster.

## Deploying an application

SSH into the master node:

```shell
ssh ubuntu@192.168.50.4
```

Use `kubectl` to interact with the cluster:

```shell
kubectl create deployment nginx --image=nginx
kubectl expose deploy nginx --port 80 --target-port 80 --type NodePort
```

Now you could visit the cluster using worker nodes:

- 192.168.50.5
- 192.168.50.6

```text
http://192.168.50.5:nginx_port
```

Use the following command to find the `nginx_port`:

```shell
kubectl get services
```
