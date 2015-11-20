# BitDust
[bitdust.io](http://bitdust.io)

BitDust is a distributed on-line storage, in which only data owner has an access to his data.

Imagine a situation when two friends and you agree to help each other to store the data. On your computer you store the data that both of your friends uploaded to you via Internet, and you in turn can use the free space on their machines to save your files. This creates [redundancy](storage#data-redundancy), but allows storing important data in a safer way. It is also a fair deal, because all three are interested in the maximum safety and availability of stored information.

BitDust network is a voluntary association of people [sharing resources](storage#suppliers-and-customers) of their personal computers. Their machines run the same copies of BitDust program, which enables device communication via Internet when data are transmitted directly from one user to another and stored on user hard disk drives. 

Uploaded input data is divided into blocks and fragments, [encrypted](security) and uploaded on other users computers. The system is designed to perform continuous monitoring of each node, which stores your data.

The BitDust program constantly keeps a state, which enables you to download your data back to your machine at any moment. The mechanism of [automatic data restoring](rebuilding) allows dynamic reassembling of the data fragments uploaded into the network on the new nodes without any action from the user. 

BitDust program is written in [Python](http://python.org) using [Twisted](http://twistedmatrix.com/) Framework and is distributed in open source code - we are still deciding about the license type. 

The project is in the stage of deep development, but the [demo version](screenshots) will be available in the near future. 

We invite all who are interested in this direction - primarily developers and enthusiasts. Join our team, or just [support us](donate). Together we will better realize our plans and create a fairer alternative to the existing global networks.


## Beginning

+ [Introduction](intro.md)
+ [Concept](concept.md)
+ [How Does It Work?](principle.md)
+ Download
+ Installation
+ Program Commands
+ User Settings


## Working Principles

+ [Glossary](glossary.md)
+ [User Identification](identities.md)
+ [Distributed Storage](storage.md)
+ [Data Security](security.md)
+ [Automatic Rebuilding](rebuilding.md)
+ [Distributed Hash-Table](dht.md)
+ Transport Protocols
+ File System
+ [Monetization](monetization.md)


## Development

+ [Finite State Machines](automats.md)
+ Network Servers
+ API Interface
+ [News](news.md)
+ [Change Log](changelog.md)
+ [Roadmap](roadmap.md)
+ [Donation / Your Contribution](donate.md)

