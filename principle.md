# How does it work?

To explain how BitDust generally works, some key aspects are described below to highlight the features of the network. 


## Decentralized peer-to-peer network

BitDust strives for full decentralization and does everything possible to achieve it. All nodes in the network run compatible software which follows the same BitDust protocol, every device can act as a client and a server for others at the same time and any node can take any of the supporting roles.

The user is able to safely interact with others using their ID and their own private key: every packet sent by the user is digitally signed with the user's private key and other nodes verify that signature with the public key from the user's identity file - see [How authentication works](identities.md#how-authentication-works).


## Distributed Data Storage

Each BitDust user is able to store data on the machines of other users, which are called suppliers. By default every user also acts as a supplier for other users, see [Sharing resources](storage.md#sharing-resources).

Uploaded data is first archived and divided into blocks, then each block is encrypted and split into fragments organized into a RAID array, so that lost fragments can be rebuilt when some of the suppliers go offline.


## Automatic Data Rebuilding

Each supplier gets two types of fragments of your encrypted data: "Data" fragments, which contain
pieces of the encrypted blocks, and "Parity" fragments, which are calculated from the Data fragments
of other suppliers using a RAID procedure. Parity fragments make it possible to rebuild lost Data
fragments when some of the suppliers go offline and place them on new suppliers. The whole process is done automatically by the BitDust
software running on the user's device and does not need any action from the user, see [Automatic Data Rebuilding](rebuilding.md).


## User Information Protection

Every packet is digitally signed. Your private data inside the packets (files, messages, keys) is encrypted before it leaves your device.
Service information needed to deliver a packet - command, sender, recipient - is signed but not encrypted, see [Data Security](security.md#data-transfer).
This makes sure that suppliers or other unauthorized users do not have access to your data. Only with a valid private key can you restore the uploaded data or read incoming messages.


## Pseudonymous Network Access

To join the network you do not need to pass through any traditional registration or login procedure, meaning that you are not obliged to provide personal information of any sort (your name, address, email, phone number, etc.). BitDust does not collect personal data of its users - you are known in the network only by your nickname and public key.

All you need is a nickname and a personal private key to access the network. Only you have access to your private key because it is generated directly on your device
when you start the software for the first time. There is no centralized entity within the BitDust network that stores your private key.


## Uses Distributed Hash-Table

A distributed hash-table is used for storing service information, supporting connections between users and maintaining of the network as a whole.


## Transmits Data by TCP

Data is transmitted using your active Internet connection. At the moment only the TCP transport is enabled: the HTTP transport is not finished yet and the UDP transport is temporarily disabled (`services/http-transport/enabled` and `services/udp-transport/enabled` are `false` by default). By default, incoming traffic reaches your node via a third node (a proxy-router); if routing is disabled, other nodes connect to your device directly - see below.


## User Connections behind NAT

There are situations when a network connection between nodes that are behind NAT cannot be established in a peer-to-peer way - receiving direct
incoming connections in the regular manner is not always possible in the Internet era. That is why other nodes, called proxy-routers, are used to route the traffic.

Routing via proxy-routers is enabled by default (the "proxy-transport" network service):

+ your node connects to a randomly selected proxy-router, and your identity file contains the network address of the router instead of your own - other nodes reach you through the router
+ the router itself knows your real IP address
+ your node is able to receive incoming connections even if it is behind NAT and has no public IP address

You can disable routing in the program settings (`services/proxy-transport/enabled`). We plan that the application will ask the user about it at the first start. If routing is disabled, your own IP address and ports are published in your identity file and other nodes connect to your device directly. In that case you must make sure in advance that your node is able to accept incoming connections: configure port forwarding on your NAT router and open the ports used by BitDust (by default TCP port `7771` and UDP port `14441` for the DHT). BitDust tries to configure port forwarding automatically via UPnP if your router supports it. Otherwise your node will not be able to receive incoming connections.

The whole packet is additionally encrypted for the proxy-router, and the router adds its own encryption and digital signature when forwarding it. A proxy-router is not able to read your private data or to change the service information passing through it, see [Data Security](security.md#data-transfer).


## Managed by Finite State Machines

The BitDust project is developed based on the principles of automata-based programming and 
[open project documentation](http://is.ifmo.ru/articles_en/).
This is a programming paradigm in which a program or its fragment is represented as a model of 
a finite state machine.

