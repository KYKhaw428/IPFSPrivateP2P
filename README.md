# IPFSPrivateP2P
P2P IPFS Private Network Setup Configurations and Steps. Setup steps for private network across different organization, entity, individual, etc.

## Prerequisites
1) Have git installed
2) Have IPFS installed
3) Have Go (programming language) installed
4) Have IPFS nodes initialized

## Setup steps

#### 1) Generate swarm key
To do this, run:
```
go get -u github.com/Kubuxu/go-ipfs-swarm-key-gen/ipfs-swarm-key-gen
```
And then
```
./go/bin/ipfs-swarm-key-gen > ~/.ipfs/swarm.key
```
Note: Change the /.ipfs/ if you've initialized the IPFS node as a different IPFS_PATH name to run this private network instead of using the original .ipfs PATH.

Once the swarm key has been generated in that ipfs node folder, replicate that file into every other peer/nodes.

<img src="guideImages/Swarm%20Key.png">

#### 2) Enable DHCP & Port forwarding for bootstrap nodes
Enabling DHCP so the private address of the boostrap node won't change.
To enable DHCP, you need to use the boostrap's node internal IP address as shown below:

<img src="guideImages/Setting%20up%20DHCP.png">

Port forwarding in order for network outside of LAN connection to be able to access the bootstrap.
To enable port forwarding, you need to use the bootstrap's node internal IP address as below:

<img src="guideImages/Setting%20up%20Port%20Forwarding.png">

#### 3) Edit config files for each node/peers
Open the config file in each ipfs node folder, and change the API and Gateway to the internal IP address of the respective node.

<img src="guideImages/Peer%20IP%20config.png">

**Important note: If you are running more than 1 node on the same machine, the TCP should be different. For example, the API's tcp should be 5002 for the second node on the same machine. The same applies to the Gateway and Swarm TCP.**

In the same file, search for the "Bootstrap" section and remove all the existing default bootstrap (because those are public IPFS bootstraps) and add your private bootstrap IP address as shown below:

<img src="guideImages/Boostrapping%20config.png">

To obtain your ipfs peer id, run the same command for each node ```ipfs id```

**Important note: You can only use your public IP address for the bootstrapping nodes, or the other nodes will be unable to locate the bootstrap if internal IP address is being used.**

#### 4) Check for peers and connect them
######		1. Run ```ipfs daemon``` on all the peers/nodes.
######		2. Check every nodes to see if they are connected to all the peers by running ```ipfs swarm peers```. 
###### 		3. If there are certain peers missing in that particular node, go to the bootstrap node and run the same command 					```ipfs swarm peers``` to get all the connected peers. 
######		4. Then proceed to connect any missing peers in the particular node by running 														```ipfs swarm connect <replace this to multi addr of missing peers>```. 
######		5. Lastly, try running ```ipfs swarm peers``` for the particular node again to check if the missing peers were added.








