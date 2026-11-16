# Roadmap

In the section below you can find a more detailed description of the future roadmap.
You can find more in-depth information about certain features within other parts of the wiki.

The section is split into two parts. The first lists features we have already put on the roadmap.
The second lists features we are thinking about putting on the roadmap in the future.
Please be aware that the timelines are only an indication.



## 2023 Stable Beta version

The BitDust contributors have already created an alpha version which makes it possible
to install the user client on your local machine. This alpha version is already available
and can be installed for Windows, Linux and Mac. The basic features include the following:

* Create an account
* Upload/download of data
* Share data with trusted friends/contacts
* Find friends and add them
* Encrypted chat with friends

We have come far already, but we need to make the alpha release more stable to
turn it into a beta version. In addition, we will make the UI more user-friendly.



## 2024 Mobile BitDust application

We spend a tremendous amount of time on our mobile phones. We use mobile phones for everything. All of the major
systems have a mobile integration. The BitDust contributors see going mobile as a huge untapped potential.
It is a device that is always online and will only become faster and get more storage in the years to come. We want
not only to provide users with the possibility of accessing their files and chatting via their mobile phone, but
also to give them the opportunity to become a mini supplier, providing storage back to other users.



## 2024 Create an Alpha for supporting roles within the BitDust ecosystem

Currently, if you want to perform any of the supporting roles within the BitDust ecosystem, you
need to do it from the command line. Unfortunately we don't yet have an easy-to-use graphical interface
for supporting roles. We would like to create one so that anyone with basic computer knowledge can support
the BitDust network.



## 2024 Implement Blockchain

In order to track the different states of the network we are implementing an open source
Python-based blockchain, based on the [Bismuth](https://github.com/bismuthfoundation/Bismuth) project. The miner will play a big role in validating the changes in the
network. You can read more about the way we want to approach this in the [Crypto Contracts](crypto_contracts.md)
and [Roles and Coins](roles_coins.md) sections. The blockchain, a built-in wallet, identity registration and storage contracts
already exist in the software, but at the moment do not affect data storage yet and can be disabled in the settings,
see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts). In short, the BitDust blockchain will:

* Give users the ability to rent space from suppliers with BitDust coins.
* Reward suppliers with BitDust coins for the service they provide in storing users' data.
* Provide mining rewards in the form of BitDust coins to miners for maintaining the BitDust blockchain.
* Provide the ReBuilder with a reward in BitDust coins for rebuilding data and finding a new supplier.
* Provide a rating system for suppliers to pick out only the best ones for data storage.
* Provide suppliers with the ability to publish their rates and available space.


## 2025 The ReBuilder

The BitDust software already implements a mechanism for automatic data rebuilding,
which reconstructs missing data fragments when a lost supplier is replaced.
The whole process is fully automatic and does not require any actions from a user in order
to maintain uploaded data in the BitDust network. However, currently it runs on the user's own
machine and works only while the BitDust software is running.

We want to transfer this functionality onto other nodes that will support this rebuilding process -
we call them the ReBuilders. This role is not implemented yet. You can read more details about
automatic rebuilding in the [Automatic Rebuilding](rebuilding.md) section.

An interesting feature of the proposed approach is that the ReBuilders will not have access
to users' data, since they always operate with encrypted data.
This will allow you to safely authorize any nodes within the BitDust network to rebuild your data,
which will greatly increase storage reliability and speed up uploading and downloading data.
Once the ReBuilder role is implemented, the whole process of remote automatic rebuilding will occur
entirely without the participation of the user - after uploading your data to the network,
you will be able to shut down your computer, or simply stop the BitDust software.

There are different aspects to making sure the ReBuilders can perform their tasks properly:

* The process of rebuilding. This is currently in development.
* The moment when a ReBuilder should start or stop their work.
* The reward the ReBuilder receives in BitDust coins for performed work.


## Ongoing: Upgrading the graphical interface

We will continuously upgrade the graphical user interface. Not only from a look and feel perspective,
but also from a feature perspective. We are currently thinking of potentially implementing the following
features in the future:

* Ability to create folders.
* Ability to share and alter data.
* Ability to chat offline.
* Ability to share offline.



# Distant possible features


## Chat/Video/Conferencing

We have already implemented simple functionality to transfer encrypted private messages between nodes.
They are transmitted peer-to-peer from sender to receiver - by default via proxy-routers, which are not able to read them -
protected with the digital signature of the sender and encrypted with the public key of the recipient before being sent to the network.

In parallel with the development of the graphical user interface, we have developed a method
for the simple exchange of short messages just like in a typical chat system.
The history can be stored on a user's suppliers so it can be accessible at any time.

We are also thinking of introducing streaming voice and video transmission
and implementing fully distributed and safe conferencing in the future.



## Decentralized Mail

It would be convenient to provide users with a familiar environment for personal
correspondence.
The proposal is to use the SMTP / POP3 / IMAP mail client that the user usually uses for
sending/receiving e-mails.
This does not require a user to install anything else.

In the BitDust software we have already implemented basic functionality for the direct transfer
of protected messages from one node to another.
Our plan is to use the standard SMTP, POP3 and IMAP protocols to
create a universal interface between the user and other nodes within the BitDust network.

When the client software starts, we could create a couple of local mail servers for incoming and
outgoing mail. These servers are pretty small and act as an
intermediate layer between the user's email client and the BitDust program.

After switching on that functionality in the BitDust software, 
the user will only need to configure the mail client that is most convenient for them.

At the moment there is no final decision on the format of mailboxes
within the BitDust network. Probably they will be formed based on the user's IDURL, for example
http://p2p-id.ru/veselin.xml will be translated to veselin@p2p-id.ru.



## Pseudonymous logon

User identification in the BitDust network does not require any personal data
and uses only short text identifiers (nicknames) and cryptographic keys.
There is no requirement to provide personal data like your email, phone number, etc.
BitDust does not collect personal data and BitDust contributors have no detailed information about the users of the software.

A network address is published in the user's identity file, so that other nodes are able to connect to them - by default the address of the user's proxy-router, or the user's own IP address if routing is disabled.
BitDust is a pseudonymous network - hiding the physical location of the user is not a goal of the project, see [Your privacy](identities.md#your-privacy).



## Global search

We would like to add a search engine within the BitDust software.
This will give users the ability to find data in the global space to which full read access has been granted -
similar to torrents.
You will be able to add keywords that make up the global search index.

In a few simple steps the owner of any uploaded data can prepare the necessary information
and put it into a global registry, which is stored in the distributed hash table.
Special "crawlers" sooner or later will visit the newly created topic,
validate it and make the appropriate changes in the global index.

This functionality will provide completely safe and independent data distribution over
the BitDust network, without censorship and without any possibility for third parties to exert pressure on users.
This is a significant advantage in comparison with classical torrent trackers.
