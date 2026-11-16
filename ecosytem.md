# Ecosystem

The BitDust network is a complex ecosystem.
This ecosystem consists of different roles that either use or support the BitDust network. In this section
we want to provide a short overview of the different roles that make up the BitDust ecosystem. You can click on the
different roles to see more in-depth information and, more importantly, how to take on that particular role.

The BitDust ecosystem consists of customers that use the BitDust network for encrypted storage and chat,
and of different supporting roles that make it possible for customers to use the BitDust network.
Currently we have 4 supporting roles that are active and 2 roles that are still being developed. Once all 6
supporting roles are live, the full power of the BitDust network will be unleashed.



## Becoming a customer of the BitDust network

A customer/user of the BitDust network can install BitDust on their laptop/PC. Once it is installed,
the user can create an identity without providing any personal data and start using BitDust to be in full control of their data.
Every piece of your private data that goes through the
network is encrypted before it leaves your device - by default with your Master Key, which never leaves your device. Nobody else has access unless you share it:
data you want to share must be encrypted from the start with a separate Additional Key, a copy of which you give to a trusted contact (see [File Sharing](sharing.md)). For now we offer the
following features:

* Creating a pseudonymous BitDust account without providing any personal data
* Uploading and downloading encrypted data
* Sharing data with trusted contacts
* Adding friends/contacts to share files and chat with
* Fully encrypted chat between friends/contacts

All of the above features can be accessed via a simple program that you can install on your laptop/PC. We have versions
for Mac, Windows, Linux and Raspberry Pi.

Curious about how to become a user of the BitDust network? Read more about it on the [How to become a customer](customer_role.md) page.



## Becoming a supporter in the BitDust network
To make the BitDust network function, multiple roles are needed to support the network. The supplier role is enabled by default
on every node, so every user already supports the network with a portion of their disk space. For now all of the other roles
can only be set up using the command line. This basically means that you don't have an easy-to-use interface to set up
your supporting role, like you have when you use BitDust as a customer/user. We will be making this available in the
future so everybody can join. For now we focus on making it possible to set up the entire BitDust ecosystem, and afterwards we will
focus on usability, so that not only developers can fulfill a supporting role, but also grandma with her laptop :).

We currently have the following supporting roles, which are live and which you can set up using the command line. They are described
briefly below. Click on the role to get more detailed information and learn how to set this role up on your local machine.

## Supporting roles that you can become right now

Supplier:
As a supplier you support the BitDust ecosystem by providing storage to customers. At the moment you provide your hard disk space
to customers for free; in the future you will be able to rent it out and get BitDust coins back as compensation (storage contracts and payments already exist
in the software, but at the moment do not affect data storage yet), see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts). As a supplier you store encrypted data of customers.
Being a good supplier also means that you keep the customers' data stored on your local machine and are always connected to the
Internet. Interested in becoming a supplier? Read more about it on the [Running BitDust supplier node](supplier_node.md) page.

ID Server:
As a customer you need to create an identity to use the BitDust network. This identity is captured in an identity file and
is distributed across the network, making sure that as a customer you can connect with other customers or other supporting
roles within the BitDust network. To make this possible, you as an ID server hold copies of identity files and make it
possible for customers to connect and communicate across the BitDust network. Despite the word "server" in the name, an ID
server is not a central server: there are many ID servers in the network owned by different people and every identity file is
stored on several of them at once. Interested in becoming an ID server? Read more about it on the
[Start new Identity Server](identity_server.md) page.

Distributed Hash Table Seed:
As a Distributed Hash Table Seed you maintain service information within the Distributed Hash Table. The Distributed Hash Table
stores service information about everybody who is connected to the network. For instance, we store your nickname in the
Distributed Hash Table, or the suppliers a specific customer has. Interested in becoming a Distributed Hash Table Seed?
Read more about it in the [Distributed Hash-Table](dht.md#support-bitdust-network) section.

Router:
As a router you help other nodes to reach each other within the BitDust network. You help other nodes to send and 
receive encrypted packets. Interested in becoming a Router? Read more about it in the [User Connections behind NAT](principle.md#user-connections-behind-nat) section.



## Supporting roles that you can become in the future

There are 2 roles that are not fully implemented yet, but will support the BitDust ecosystem in the future. When these 2 roles
are finished, the BitDust network will be fully operational.

ReBuilder:
Suppliers can go offline due to circumstances. To ensure that your data has high availability, it is the ReBuilder's job to
rebuild the data that was lost at one supplier and move it to another supplier. Until this role is implemented, the rebuilding
is performed by the BitDust software on the customer's own device. Read more in detail about our approach in the
[Automatic Rebuilding](rebuilding.md) section.

Miner:
There are a lot of complex relationships within the BitDust ecosystem and it is important to be transparent about them. We use
blockchain technology to accomplish this. A miner validates transactions within the BitDust blockchain. Transactions are the
sending and receiving of BitDust coins from one user to another. BitDust coins will be used to reward supporting roles within
the network. For instance, a customer will need BitDust coins to store data at suppliers. The BitDust blockchain, wallet and storage contracts
already exist in the software, but at the moment do not affect data storage yet and can be disabled in the settings; mining services are disabled by default,
see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts). Read more about the original
approach in the [Roles and Coins](roles_coins.md) draft. An alternative approach without its own blockchain, where payments
are handled by the Merchants, is described in the [Merchants](merchants.md) draft proposal.

In the future we may add roles in order to create a fairer and more sustainable ecosystem. For now these are the roles which we will 
be implementing. Currently all of these roles can only be set up if you have the proper hardware and are familiar with setting things up
via the command line. In the future we will be developing an easy-to-use application for all supporting roles. This way, even if you
are not tech-savvy, you will still be able to support the BitDust network with your laptop/PC. Every role has a more in-depth section 
which provides you with a how-to in order to set that role up on your local machine. If you like what you are reading and want
to contribute in other ways, please email us at info@bitdust.io. In addition, we are always happy to help you set things up on
[Telegram](https://t.me/bitdust).  
