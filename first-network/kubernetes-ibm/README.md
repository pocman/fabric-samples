# Build Your First Network on Kubernetes using Minikube.

## Problem definition

So first, we need to define exactly what we want to do, why and how it diverge from the tutorial.

Minikube is a tool that makes it easy to run Kubernetes locally. 
Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day. 
On mac, it can be installed using the docker client, the installation guide is available [here](https://docs.docker.com/docker-for-mac/#kubernetes).

Hyperledger Fabric is a blockchain framework implementation. 
Hyperledger Fabric provides a modular architecture, which allows components such as consensus and membership services to be plug-and-play. 
Hyperledger Fabric is allowing entities to conduct confidential transactions without passing information through a central authority. 
This is accomplished through different channels that run within the network, as well as the division of labor that characterizes the different nodes within the network. 
Lastly, Hyperledger Fabric supports permissioned deployments.
A good introduction of the architecture is available on Edx, LFS171x chapter 9.

### Why
We want to improve the development tooling and processes and increase collaboration between data scientists, software developers, and quality engineers.
To do so, we want to provide an easy way to run Hyperledger Fabric on a laptop.

### What
At the end of this exercice, we should be able to : 
* build a HF network on a clean machine with kubernetes install (equivalent of docker-compose up --build)
* up and down the network at will (equivalent of docker-compose up|down)
* clean the environment and all the associated data (equivalent of docker-compose down -v)

### How
We need to adapt the tutorial available in the repo to run on kubernetes (its currently running with docker-compose with a lot of [prerequisites](https://hyperledger-fabric.readthedocs.io/en/release-1.3/prereqs.html#prerequisites)). 
As a squeleton, we can use an the amazing sample provided by IBM [here](https://github.com/IBM/blockchain-network-on-kubernetes) for hyperledger v1.2.1.
The goal is to follow step by step the tutorial availabile [here](https://hyperledger-fabric.readthedocs.io/en/release-1.3/build_network.html#).

## TASKS
* Run and fix the IBM tutorial
    - [DONE] Fix "Waiting for container of copy artifact pod to run" when pod already succeeded
    - [DONE] First issue, the cryptogen container can't chmod on the /shared folder.
* Migrate configuration from first-network to IBM tutorial
    - [DONE] Copy artifacts 
    - [DONE] Modify blockchain-services.yaml (Setup 2 peers per Org)
    - [DONE] Modify peersDeployment (Start 2 peer for Org 1 and 2)
    - [DONE] Modify create_channel (Create a Channel Configuration Transaction and Create channel)
    - [DONE] Join the channel
    - [DONE] Update the anchor for org 1
    - [TODO] Join channel with peer0org2 and set anchor for org2
    - [TODO] Enable TLS (set ORDERER_GENERAL_TLS_ENABLED=true)
* Install & Instantiate Chaincode
    ...

## Run

Install kubernetes and kubectl.

```bash
## macOS
sed -i '' s#unix:///host/var/run/docker.sock#tcp://docker:2375# configFiles/peersDeployment.yaml

## Linux
sed -i s#unix:///host/var/run/docker.sock#tcp://docker:2375# configFiles/peersDeployment.yaml

chmod +x setup_blockchainNetwork.sh
./setup_blockchainNetwork.sh
```

## Clean

```bash
chmod +x setup_blockchainNetwork.sh
./deleteNetwork.sh
```