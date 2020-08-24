---
title: "Complete the setup of a swarm mode cluster, with managers and worker nodes"
layout: post
date: 2020-05-16
categories: docker swarm orchestration
---

# References

* [Docker Certification](https://success.docker.com/certification)
* [Docker Certified Associate Study Guide](https://docker.cdn.prismic.io/docker/3acec17d-0fc1-4a61-80da-c464d0408e72_DCA_study+Guide_v1.2.pdf)

# Domain 1: Orchestration

## Complete the setup of a swarm mode cluster, with managers and worker nodes

Starting from tutorial [Getting started with swarm mode](https://docs.docker.com/engine/swarm/swarm-tutorial/#three-networked-host-machines).

The objectives of the tutorial are:

* initializing a cluster of Docker Engines in swarm mode
* adding nodes to the swarm
* deploying application services to the swarm
* managing the swarm once you have everything running

### [Three networked host machines](https://docs.docker.com/engine/swarm/swarm-tutorial/#three-networked-host-machines)

Create tree t2.micro linux Ubuntu 18.04 instance on AWS with associated key pair, network interface and a VPC with routing 0.0.0.0/0 vs internet gateway.

<img src="/blog/images/20200516-1.png">

For access to an instance select the instance choose to Connect and copy the command (for AWS):

```powershell
ssh -i "key-pair-xxx.pem" ubuntu@xxx.xxx.xxx.xxx
```

### [Docker Engine 1.12 or newer](https://docs.docker.com/engine/swarm/swarm-tutorial/#docker-engine-112-or-newer)

For __each__ instance follow [INSTALL DOCKER ENGINE ON LINUX MACHINES](https://docs.docker.com/engine/swarm/swarm-tutorial/#install-docker-engine-on-linux-machines) then go to [Install Docker Engine](https://docs.docker.com/engine/install/) and follow the instruction in [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/).

### [Install using the repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

##### SET UP THE REPOSITORY

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```bash
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
2. Add Docker’s official GPG key:

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88 #Verify that you now have the key with the fingerprint ..., by searching for the last 8 characters of the fingerprint.

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

3. Use the following command to set up the stable repository.

```bash
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

##### INSTALL DOCKER ENGINE

1. Update the apt package index, and install the latest version of Docker Engine and containerd:

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

2. Verify that Docker Engine is installed correctly by running the hello-world image.

```bash
$ sudo docker run hello-world
```

##### UPGRADE DOCKER ENGINE

To upgrade Docker Engine, first, run sudo apt-get update, then follow the [installation instructions](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), choosing the new version you want to install.

### [The IP address of the manager machine](https://docs.docker.com/engine/swarm/swarm-tutorial/#the-ip-address-of-the-manager-machine)

Open following inbound port in security group associated with the three instances (for AWS).

* TCP port 2377 for cluster management communications
* TCP and UDP port 7946 for communication among nodes
* UDP port 4789 for overlay network traffic

### [Create a swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/)

1. Run the following command to create a new swarm

```bash
$ sudo docker swarm init

Swarm initialized: current node (ofhtc7ui6v80863fedwscq9dq) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1d7zv59cgsu0sw0os03m9hgkcnd1yalmg7wvzsdk2wbpgdfj12-5r8kq67jk8pllfngh8thfktwx xxx.xxx.xxx.xxx:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

### [Add nodes to the swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/add-nodes/)

1. On each worker run the join command

```bash
$ sudo docker swarm join --token SWMTKN-1-1d7zv59cgsu0sw0os03m9hgkcnd1yalmg7wvzsdk2wbpgdfj12-5r8kq67jk8pllfngh8thfktwx xxx.xxx.xxx.xxx:2377
This node joined a swarm as a worker.
```

### Check the swarm from manager node and list the joined nodes

```bash
$ sudo docker info
...
 Swarm: active
  NodeID: ofhtc7ui6v80863fedwscq9dq
  Is Manager: true
...
$ sudo docker node ls
ID                            HOSTNAME                STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
y268dsff9go11ppqcqf3r5bsz     ip-xxx-xxx-xxx--xxx     Ready               Active                                  19.03.8
m6thmiperfbpa3v9b2yg8yowi     ip-xxx-xxx-xxx--xxx     Ready               Active                                  19.03.8
ofhtc7ui6v80863fedwscq9dq *   ip-xxx-xxx-xxx--xxx     Ready               Active              Leader              19.03.8
```
