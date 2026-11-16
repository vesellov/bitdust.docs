# BitDust Concept


## Basic Principles

The BitDust project explores the possibilities of introducing new technologies in society to protect private information on the network.

We are aiming for a worldwide distributed computer network designed to increase the safety of private communications, data storage, exchange, publishing, etc.

The BitDust project is based on the following principles that are followed throughout the whole development process:

  * Decentralization
  * Security
  * Fairness
  * Pseudonymity
  * Usability
  * Scalability
  * Openness


## Decentralization

BitDust is a holistic ecosystem, the basic idea of which is to strive for complete decentralization of information flows and stored data - the project does everything possible to achieve it.
Wherever possible, instead of the usual "client-server" interaction model the BitDust network uses "user-user" links.
Even the ID servers, which publish the identity files of users, are not central servers: there are many of them in the network owned by different people, every identity file is stored on several ID servers at once and any user can start their own [ID server](identity_server.md).

Your private information is transmitted from peer to peer in encrypted form - by default via a third node (proxy-router), which is not able to read the encrypted data, or directly if routing is disabled (see [User Connections behind NAT](principle.md#user-connections-behind-nat)). User data is broken up into multiple fragments, which are distributed among the user's suppliers: the suppliers themselves are selected randomly among the nodes of the network, and every supplier stores exactly those fragments which correspond to its position (see [Distributed Storage](storage.md#suppliers-and-customers)).


## Security

The personal security of each user and of the system as a whole is achieved by the BitDust architecture itself: by encrypting all private data before it leaves your device and by digitally signing all traffic, see [Data Security](security.md#data-transfer).

Unlike the widely known existing online services, BitDust operates in a completely different way: users are identified by a nickname and a cryptographic key, BitDust never asks for and does not collect any personal data of the user, so there is no central database that links the account to a real person.


## Fairness

BitDust will benefit the collaborative economy by allowing people to provide their hard disk space to the network - for free at the moment, and in the future in exchange for BitDust coins. Every user is consuming resources from the network and, by default, also donating storage/network/CPU from their own devices to other users - some users consume more and some donate more, and both sides must benefit from that (see [Sharing resources](storage.md#sharing-resources)). In short, the BitDust software is totally free and open source, but consuming resources & services within BitDust may become billable in the future.

The BitDust software already includes its own blockchain with a built-in wallet for BitDust coins, identity registration and storage contracts between customers and suppliers - at the moment this does not affect data storage yet, and the function can be disabled in the program settings, see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts). The blockchain accounts for the work carried out (storage contracts and payments), not the personal data or the activity of the users, and in the future it will be used to compensate supporters within the network and provide open and fair tracking of the internal economy within BitDust.


## Pseudonymity

BitDust does not collect any personal data of its users. When using the BitDust software you do not need to provide your real name, address, email or phone number - you are known in the network only by a nickname you choose and your public key. In other words, BitDust is a pseudonymous system.

All your private data is encrypted using RSA & AES. Only you can read data encrypted with your [Master Key](glossary.md#master-key), which never leaves your device. If you want to share some data, encrypt it from the start with a separate [Additional Key](glossary.md#additional-key) and give a copy of that key to a trusted contact - the key of already encrypted data cannot be changed later, see [Sharing](sharing.md). Nobody else, including suppliers and the BitDust team, can read your data.

Note that your identity file publishes a network address (IP address and port) through which other nodes are able to connect to you. By default this is the address of your proxy-router, and the router itself knows your real IP address; if you disable routing, the address of your own device is published (see [User Connections behind NAT](principle.md#user-connections-behind-nat)). Hiding the physical location of the user is not a goal of the BitDust project - other options to reduce the disclosure of your network address are described in [Your privacy](identities.md#your-privacy).


## Usability

The BitDust project aims to create a user-friendly environment in which a user has full control over most of the existing elements within the application.

With an advanced level of security and privacy, all BitDust users will have an opportunity to gain private access to a fully distributed online storage network. Via an easy-to-use application you will be able to upload files, share them with trusted contacts and chat privately, without providing any personal data. In short, we want to make it so easy that your grandmother could use it.


## Scalability

The software must be able to work both for a network of one hundred users and for a network of millions.

The more supporters join the BitDust network, the more storage can be offered to consumers and the more stable and "bulletproof" the network becomes.


## Openness

Unlike many corporate products, we are aiming for community-driven development where decisions, problems and achievements are shared and available for public review.

We firmly believe that open source code, proper documentation and visualization of all operating processes are necessary conditions for the creation of a quality product, thereby maximizing the security level of users' personal data.

