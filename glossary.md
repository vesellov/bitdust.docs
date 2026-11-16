# Glossary


#### BitDust
A global computer network for secure distributed data storage, sharing, publishing, digital communications, etc.


#### BitDust software
Open source software developed and distributed by [BitDust contributors](https://bitdust.io/#team)
under [GNU Affero General Public License v3](https://www.gnu.org/licenses/agpl-3.0.en.html).


#### Node
A computer connected to the Internet, which has the [BitDust software](#bitdust-software) installed and running.


#### BitDust Network
A voluntary association of people using their personal computers to run and maintain [BitDust nodes](#node), which interact with each other independently.


#### Data
Any information which can be stored on a computer disk drive as a single file or a folder.


#### Backup Copy
An exact copy of [data](#data), encrypted by a [node](#node) at some point in time.


#### Folder Backup Copy
A [backup copy](#backup-copy) of data obtained by archiving all files stored in a given folder on the computer's hard disk drive, possibly including all sub-folders.


#### Distributed Copy
A [backup copy](#backup-copy) stored on different [nodes](#node) in the [BitDust network](#bitdust-network).


#### Block
A file on a hard disk drive containing only a part of a [backup copy](#backup-copy) and having an order number assigned at the moment of creation.


#### Fragment
A part of a [block](#block), prepared in the process of creating a [backup copy](#backup-copy) and having an order number – a position in the block; may also be called a sector.
The position of a fragment matches the position of the [supplier](#supplier) in the user's combination of suppliers: every supplier stores only fragments with its own position number.


#### Size of Backup Copy
The total size of all [blocks](#block) related to a given [backup copy](#backup-copy).


#### Packet
#### Service Packet
Useful information transferred from one [node](#node) to another in the [BitDust network](#bitdust-network)
and containing the [digital signature](#digital-signature) of the sender.
Service information of the packet (command, sender, recipient) is not [encrypted](#encryption); private [data](#data) is carried inside it in encrypted form,
see [Data Security](security.md#data-transfer).


#### Data Fragment
A [fragment](#fragment) of the first RAID layer, which contains a piece of an [encrypted](#encryption) [block](#block).
Every [supplier](#supplier) stores the Data fragments with its own position number, see [Automatic Data Rebuilding](rebuilding.md#data-redundancy).


#### Parity Fragment
A [fragment](#fragment) of the second RAID layer, calculated by an XOR operation between several [Data fragments](#data-fragment) of the same [block](#block) placed on other [suppliers](#supplier).
Parity fragments make it possible to [rebuild](#rebuilding) lost Data fragments, see [RAID Transformation](rebuilding.md#raid-transformation).


#### Data Packet
#### Data Portion
A [packet](#packet) containing an [encrypted](#encryption) [fragment](#fragment) of [data](#data).


#### BitDust Data Storage
A method of [data](#data) processing that involves source data [archivation](#archivation),
division into [blocks](#block), [encryption](#encryption) of each block,
splitting of encrypted blocks into [fragments](#fragment) and placing them on different [nodes](#node).


#### Restore
A process of inverse transformation of [data](#data) from divided [fragments](#fragment) and reconstruction of the source data from an existing [backup copy](#backup-copy).
Performed on the user's device when data is [downloaded](#download-from-the-bitdust-network) from the network.
Not to be confused with [rebuilding](#rebuilding).


#### Rebuilding
A process of reconstructing lost [fragments](#fragment) of a [distributed copy](#distributed-copy) from the fragments still available at other [suppliers](#supplier)
and placing them on a new supplier, which replaced the one that went offline.
Currently performed automatically by the BitDust software on the [customer's](#customer) device, see [Automatic Data Rebuilding](rebuilding.md).
Not to be confused with [restore](#restore).


#### Upload to the BitDust Network
A process of creating a [distributed copy](#distributed-copy) of given [data](#data),
[portions](#data-portion) of which are located on the [suppliers'](#supplier) machines.


#### Download from the BitDust Network
A process of [data](#data) [restore](#restore) from a
[distributed copy](#distributed-copy)
located on the [suppliers'](#supplier) machines.


#### Supplier
A [node](#node) in the [BitDust network](#bitdust-network), whose owner voluntarily agreed to store other users' [data](#data) on the hard disk drive of their personal computer.
Currently without reward; in the future may be compensated with BitDust coins, see [Sharing resources](storage.md#sharing-resources) and [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts).


#### Customer
A [node](#node) in the [BitDust network](#bitdust-network), whose owner uses the space on the HDDs of other [nodes](#node) for storing their own [data](#data).


#### Supporter
A user who maintains the [BitDust network](#bitdust-network) by running one or more supporting roles on their [node](#node): [supplier](#supplier), [proxy-router](#proxy-router), [ID-server](#id-server) or [DHT seed node](#dht-seed-node).
These are roles, not a separate group of people: by default every node is both a [customer](#customer) and a supplier, see [Sharing resources](storage.md#sharing-resources).


#### Proxy-router
#### Router
A [node](#node) which routes the traffic of other nodes, so that they can receive incoming connections even when they are behind NAT and have no public IP address (the "proxy-server" network service, disabled by default).
Routing via a randomly selected proxy-router is enabled by default (the "proxy-transport" network service): the [identity](#identity) file of the user then contains the network address of the router instead of the user's own address, and the router knows the user's real IP address.
The router cannot read the user's private [data](#data) or change the service information of the [packets](#packet) passing through it, see [User Connections behind NAT](principle.md#user-connections-behind-nat).


#### Distributed Hash-Table
#### DHT
A decentralized key-value storage maintained jointly by all [nodes](#node) of the [BitDust network](#bitdust-network), which is used to store service information, to find other nodes (for example, candidates for new [suppliers](#supplier)) and to support connections between users, see [Distributed Hash-Table](dht.md).


#### DHT Seed Node
A [node](#node) with a well-known network address, which helps other nodes to join the [DHT](#dht), see [Running BitDust seed node](seed_node.md).


#### ReBuilder
A planned supporting role, not implemented yet: a [node](#node) which will perform [rebuilding](#rebuilding) of lost [fragments](#fragment) on behalf of the [customer](#customer).
Until then, rebuilding is performed by the BitDust software on the customer's own device.


#### Miner
A planned supporting role: a [node](#node) which will maintain the [BitDust blockchain](#bitdust-blockchain).
Mining services already exist in the software, but are disabled by default. An alternative approach is described in [BitDust Merchants](merchants.md).


#### Customers folder
A local folder on the hard disk drive of the [supplier's](#supplier) machine, where the [data](#data) of their [customers](#customer) is stored.


#### Shared Supplier Space
The maximum possible volume of the [customers folder](#customers-folder).


#### Used Supplier Space
The current volume of the [customers folder](#customers-folder).


#### Allocated Customer Space
An amount of storage space previously agreed upon with the user, which states the maximum total volume of all [data](#data) that the [customer](#customer) can place on the given [node](#node).


#### Free Supplier Space 
The difference between the [shared supplier space](#shared-supplier-space) and the total volume of [allocated customer space](#allocated-customer-space) for all the [customers](#customer) of the user.


#### Used Customer Space
The total volume of all [distributed copies](#distributed-copy) already created by a user.


#### Requested Customer Space
An upper bound of [used customer space](#used-customer-space)
for a given user – in other words, the maximum total volume of [data](#data), which can be allocated and provided by the [suppliers](#supplier).


#### Free Customer Space
The difference between [requested](#requested-customer-space) and 
[used](#used-customer-space) [customer](#customer) space.


#### Command
A [packet](#packet) of [data](#data) containing a service code, a [message](#message) or any other information, used for interaction between two [nodes](#node) in the [BitDust network](#bitdust-network).


#### Message
An [encrypted](#encryption) text message addressed to a specific user.


#### BitDust Blockchain
A public blockchain built into the [BitDust software](#bitdust-software) (based on a modified copy of the Bismuth project), which records [identity registrations](#identity), [storage contracts](#storage-contract) and payments in BitDust coins between [nodes](#node).
It does not contain personal data of the users. At the moment it does not affect data storage yet and can be disabled in the program settings, see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts).


#### BitDust Coins
The internal currency of the [BitDust blockchain](#bitdust-blockchain), kept in a wallet built into the [BitDust software](#bitdust-software).
In the future it will be used to compensate [supporters](#supporter) for the resources they provide to the network.


#### Storage Contract
A digitally signed agreement between a [customer](#customer) and a [supplier](#supplier), recorded in the [BitDust blockchain](#bitdust-blockchain), which accounts for the storage space provided; completed contracts are paid with [BitDust coins](#bitdust-coins).
At the moment storing data remains free and suppliers do not reject customers who have not paid, see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts).


#### Encryption
A [data](#data) transformation for the purpose of its protection.


#### Archivation
A [data](#data) transformation for the purpose of creating a unified stream and decreasing the total size of the stored information.


#### Private Key
#### Secret Key
The secret part of an asymmetric key pair, which is generated on the user's device. The [Master Key](#master-key) never leaves the device; a copy of an [Additional Key](#additional-key) can be given to a trusted user.
It is used to decrypt [data](#data) addressed to the user and to create [digital signatures](#digital-signature).
You are welcome to read a [detailed article](https://en.wikipedia.org/wiki/Public-key_cryptography) on Wikipedia.


#### Public Key
The open part of an asymmetric key pair, which is published in the user's [identity](#identity) file.
It is used by other [nodes](#node) to encrypt data addressed to the user and to verify the user's [digital signatures](#digital-signature).


#### Master Key
The main [private key](#private-key) of the user, generated on the user's device when the [identity](#identity) is created.
It is used by default to [encrypt](#encryption) the user's [data](#data) and provides access to the [BitDust network](#bitdust-network).
The Master Key never leaves the user's device (except for a backup copy made by the user) and is never transferred to anyone else,
so data encrypted with the Master Key can be read only by its owner. See [Master Key](sharing.md#master-key).


#### Additional Key
A [private key](#private-key) other than the [Master Key](#master-key), created by the user, for example to make some data shareable.
To share some [data](#data), the user must [encrypt](#encryption) it with an Additional Key instead of the [Master Key](#master-key) before [uploading](#upload-to-the-bitdust-network).
A copy of the Additional Key can then be transferred to a trusted user, who becomes able to download and decrypt that data.
The key of already encrypted data cannot be changed later: data encrypted with the Master Key cannot be shared. See [Additional keys](sharing.md#additional-keys).


#### Digital signature
A value calculated from the content of a [packet](#packet) or file with the help of the [private key](#private-key) of its creator.
Anyone who has the corresponding [public key](#public-key) can verify that the content was created by the owner of that key and was not modified.


#### Authentication
Verification of the [digital signature](#digital-signature) of a [packet](#packet) or [identity](#identity) file with the [public key](#public-key) of its creator.
In BitDust it is performed independently by every [node](#node), there is no login procedure and no authentication server.
See [How authentication works](identities.md#how-authentication-works).


#### Identity
A publicly readable file containing a [public key](#public-key), 
[digital signature](#digital-signature), contact details and other information belonging to a specific user.


#### Nickname
A name chosen by the user when the [identity](#identity) is created. It is part of the [IDURL](#idurl) and, together with the [public key](#public-key), the only thing by which the user is known in the [BitDust network](#bitdust-network) - no real name, email or phone number is required.


#### ID-server
#### Identity server
A [node](#node) in the [BitDust network](#bitdust-network) which receives, stores and publishes copies of [identity](#identity) files of other nodes.
Despite the word "server" in the name, it is not a central server: there are many ID-servers in the network owned by different people,
every identity file is stored on several ID-servers at once and any user can start their own ID-server. See [Start new Identity Server](identity_server.md).


#### Identity URL
#### IDURL
#### User Identifier
A unique address of a specific [identity](#identity) file on the Internet and in the [BitDust network](#bitdust-network).


#### Network join
A process of creating a new [identity](#identity) file, delivering that file to several [ID-servers](#id-server) and acquiring a unique [IDURL](#idurl) address for a given user.


#### Identity Recovery
A process of downloading an [identity](#identity) file from one of the [ID-servers](#id-server) (via a known [IDURL](#idurl)) and then checking that the file content matches the user's [private key](#private-key).


#### Automat
#### Finite State Machine
A mathematical model designed to specify any algorithm in which all possible [states](#state) of the prototype process are salient and transitions between states are specified.


#### State
One of the stages of an algorithm, identified by careful and meaningful analysis of the modeled process.


#### Transition Graph
A visual representation of a [finite state machine](#finite-state-machine) which displays: states, transitions, events, conditions, actions and text descriptions.

