# Build Your First Network on Kubernetes using Minikube.

The directions for using this are documented in the Hyperledger Fabric
["Build Your First Network"](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html) tutorial.

*NOTE:* After navigating to the documentation, choose the documentation version that matches your version of Fabric

## Problem definition

So first, we need to define exactly what we want to do, why and how it diverge from the tutorial.

Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day. On mac, it can be installed using the docker client, the installation guide is available [here](https://docs.docker.com/docker-for-mac/#kubernetes).

Hyperledger Fabric is a blockchain framework implementation. Hyperledger Fabric provides a modular architecture, which allows components such as consensus and membership services to be plug-and-play. Hyperledger Fabric is allowing entities to conduct confidential transactions without passing information through a central authority. This is accomplished through different channels that run within the network, as well as the division of labor that characterizes the different nodes within the network. Lastly, Hyperledger Fabric supports permissioned deployments.
A good introduction of the architecture is available on Edx, LFS171x chapter 9.

### WHy
Since minikube is targeted for the development environment, we probably want to create a low footprint version of an Hyperledger Fabric network that should be easy to use in a daily workflow.

### What
At the end of this exercice, we should be able to : 
* build a HF network on a clean machine with kubernetes install (equivalent of docker-compose up --build)
* up and down the network at will (equivalent of docker-compose up|down)
* clean the environment and all the associated data (equivalent of docker-compose down -v)

### How
We need to adapt the tutorial available in the repo to run on kubernetes (its currently running with docker-compose with a lot of [prerequisites](https://hyperledger-fabric.readthedocs.io/en/release-1.3/prereqs.html#prerequisites)). We can use an alternative sample such as the one provided by IBM [here](https://github.com/IBM/blockchain-network-on-kubernetes) for hyperledger v1.2.1 as a starting point.

## Architecture Description

There are three different types of roles within a Hyperledger Fabric network:
* Clients
	Clients are applications that act on behalf of a person to propose transactions on the network.
* Peers
	Peers maintain the state of the network and a copy of the ledger. There are two different types of peers: endorsing and committing peers. However, there is an overlap between endorsing and committing peers, in that endorsing peers are a special kind of committing peers. All peers commit blocks to the distributed ledger.
	- Endorsers simulate and endorse transactions
	- Committers verify endorsements and validate transaction results, prior to committing transactions to the blockchain.
* Ordering Service 
	The ordering service accepts endorsed transactions, orders them into a block, and delivers the blocks to the committing peers.
	The Fabric v1.0 architecture has been designed such that the specific implementation of "ordering" becomes a pluggable component. The default ordering service for Hyperledger Fabric is Kafka. Solo is suitable for development purpose but we want an iso prod environment so we will use Kafka.

During this exercice, we will probably be able to use a single endorsing peer.

We will also need 
* couchdb to store the current state data that represents the latest values for all assets in the ledger. 
* kafka for the ordering
* zookeeper for kafka

We will be using Fabric 1.2.1 version.

## Planning
* Run and fix the tutorial
    - Fix "Waiting for container of copy artifact pod to run" when pod already succeeded
    - First issue, the cryptogen container can't chmod on the /shared folder.
* Migrate network configuration from first-network
* Bump to Fabric 1.3.0

## Run

Install kubernetes and kubectl.

```bash
cd blockchain-network-on-kubernetes

## macOS
sed -i '' s#unix:///host/var/run/docker.sock#tcp://docker:2375# configFiles/peersDeployment.yaml

## Linux
sed -i s#unix:///host/var/run/docker.sock#tcp://docker:2375# configFiles/peersDeployment.yaml

chmod +x setup_blockchainNetwork.sh
./setup_blockchainNetwork.sh
```