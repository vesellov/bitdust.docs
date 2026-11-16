# Distributed Storage

In this section you can find information on how your data is stored within the BitDust network.

* [Introduction](#intro)
* [Suppliers / Customers](#suppliers-and-customers)
* [Sharing resources](#sharing-resources)
* [Blockchain and storage contracts](#blockchain-and-storage-contracts)
* [Upload to the Network](#upload-to-the-network)
* [Data Redundancy](#data-redundancy)
* [Download from the Network](#download-from-the-network)
* [Hardware Resources](#hardware-resources)



## Intro

The BitDust network consists of many equal and self-governing nodes, whose owners provide a part of their hardware resources to the network. By consuming data resources from the BitDust network, users get access to more private, secure and fully independent storage to protect their data, and the ability to chat securely with their contacts. This is secure because data stored within the BitDust network is always encrypted.



## Suppliers and Customers 

In short: suppliers are selected randomly among the nodes of the network, but fragments of your data are placed on them deterministically - every supplier has its own position in the combination and stores only those fragments which correspond to that position.

User data is allocated on a fixed combination of nodes chosen beforehand; these nodes are called "suppliers". The user at the same time becomes a "customer" for all of these nodes – figuratively speaking a user "rents" a portion of the hard disk drive from each of their suppliers. However, the user is free to "fire" any of their suppliers at any moment and find another supplier within the BitDust network.

Before the user can start their first data upload, the BitDust program on the user's device has to find and connect to the user's suppliers and request "needed space" for their data. At any time, in the program settings, a user can adjust two key parameters of their distributed storage environment:

+ a volume of "needed" space
+ a desired number of suppliers

Based on this data, the system calculates the amount of space that should be provided by every node:

    <supplier quota> = ( <needed space> / <number of suppliers> ) * 2

In the formula we have to multiply the final quota by 2 because of redundancy: data stored on the supplier's machine takes twice as much space as the original file. In some cases it might be less, because files are compressed before they are encrypted.

The number of suppliers can be one of these values:
`2`, `4`, `7`, `13`, `18`, `26`, `64`. Below you will find a detailed description of how the data allocation is done and how the combinations of suppliers are formed.

The number of customers each supplier can support is theoretically not limited. However, by supporting too many customers the maximum number of sessions can be exceeded – this parameter depends on the operating system running the given BitDust node and on the local network limitations.

Apart from that, with simultaneous uploads/downloads the network channel available to each customer can be severely restricted – only nodes with very high bandwidth will be able to support a large number of nodes.



## Sharing resources

"Customer" and "supplier" are roles of a node, not different groups of people. By default both roles are enabled in the BitDust software, so every participant of the network:

+ stores its own encrypted data on the machines of other nodes - acts as a "customer"
+ provides a portion of its own hard disk drive to other nodes - acts as a "supplier"

The exchange of resources happens with the whole network, not in pairs: your suppliers are selected randomly among the nodes of the network, and the nodes which store their data on your machine are, in general, other nodes. Nobody who stores your data is obliged to store anything on your machine, and vice versa.

Each of the roles can be disabled in the program settings at any moment:

+ a user who only wants to support the network can disable the customer role (`services/customer/enabled`) - read more on the [Running BitDust supplier node](supplier_node.md) page
+ a user who does not want to provide their own disk space can disable the supplier role (`services/supplier/enabled`)

Participation in the BitDust network is always voluntary - every user decides whether to enable the supplier role and how much disk space to provide. There are two models of how resources are exchanged:

+ __now__: resources are provided to the network for free ("donated") - providing resources is not rewarded and consuming resources is not charged; the storage contracts and payments described below already exist in the software, but do not affect data storage yet
+ __in the future__: resources can be exchanged for BitDust coins - in other words, suppliers will be able to "rent out" their disk space. A user who provides more resources than they consume will be able to use all services of the network for free, and a user who consumes more than they provide will pay the difference with BitDust coins - read more in the [Crypto Contracts](crypto_contracts.md) section.

In both models the user voluntarily decides to take part in the exchange - "voluntary association" and "renting out disk space" do not exclude each other.



## Blockchain and storage contracts

The BitDust software already includes its own blockchain, based on a modified copy of the [Bismuth](https://github.com/bismuthfoundation/Bismuth) project, and the following functions are enabled by default:

+ a built-in wallet for BitDust coins
+ registration of your identity in the blockchain: a public transaction containing your nickname and public key is recorded, and a small registration bonus in coins is sent to your wallet
+ digitally signed storage contracts between a customer and their suppliers: the duration of each next contract grows, and completed contracts are paid automatically with BitDust coins from the customer's wallet

At the moment this does not affect data storage yet: storing data remains free, and suppliers do not reject customers who have not paid. The function can be disabled in the program settings:

        bitdust set services/bismuth-blockchain/enabled false

The services that depend on it - the wallet, identity registration, customer and supplier contracts - will not be started either. Mining services also exist in the software, but are disabled by default.



## Upload to the Network

Each of the suppliers receives an equal portion of the total volume of uploaded user data. Each fragment of uploaded data is marked with the position number of a supplier in the combination. When one supplier is replaced by another, the new node gets exactly the data that corresponds to its position within the combination.
 
For a regular and stable upload, the input data is archived and divided into blocks. This phase is practically identical to creating a regular archive from the content of a folder or a file on the hard disk drive and splitting it into volumes.

Afterwards each block is encrypted with one of the user's private keys: by default with the [Master Key](glossary.md#master-key), or with an [Additional Key](glossary.md#additional-key) chosen before the upload if you want to share this data with another user. More information can be found in the [File Sharing](sharing.md) and [Data Security](security.md) sections.

Furthermore, your encrypted data will be divided into fragments of equal size, which are temporarily saved on your local hard disk drive. At the final stage these local files, which are ready for sending, are uploaded to the remote suppliers’ nodes. There they are stored as they are (in encrypted state), but are fully distributed over the whole combination of suppliers – each supplier receives exactly those fragments which were prepared for their position within the combination. 

Here is the general sequence of actions to create a new distributed copy of your data:

1. reading input data from a folder or file on the user’s local hard disk drive
2. data archivation and optional compression with `gzip`
3. division of the single array of binary data into blocks of the same size
4. encryption of each block with one of the user’s secret keys
5. running RAID procedures to create a combination of fragments from each block
6. forming data portions from fragments and storing them temporarily on the local disk drive
7. uploading all portions to the assigned suppliers’ nodes
8. deleting temporary files from the local disk drive (optional)

In the settings the user can set the maximum and desired block size for all data backups – this will influence the final portion size, which will be uploaded one by one and stored at your suppliers.

When uploading data that consists of big blocks, the amount of loss increases critically and this can lead to low data reliability. On the other hand, with small block sizes the total number of fragments increases, which affects the overall program performance and the amount of memory required on the customer's machine.


## Data Redundancy

In the section [Automatic Data Rebuilding](rebuilding.md) the method of converting the input data before allocating it on the suppliers' nodes is explained in more detail. Data uploaded to the network comes with double redundancy and is organized in a RAID array so that it can be rebuilt in case of loss.

There are two parallel layers: Data and Parity. They have the same size, but different content:

+ The Data layer consists entirely of input data
+ The Parity layer has modified content, built with the help of a RAID procedure

To generate the Parity layer during backup processing, the data is taken byte by byte and an XOR operation is applied between the Data fragments of your other suppliers. The final RAID array is organized in a way that each Parity fragment allows restoring the corresponding Data fragment onto the machine of a new supplier, which replaces the lost one.

When building a RAID array, so-called ECC codes are used: these are the combinations of Data and Parity allocation on the suppliers' machines. For each of the possible supplier combinations the optimal ECC code was calculated beforehand - it basically defines the framework of the mutual bracing of the Data and Parity fragments in the layers and the maximum number of simultaneous errors that may occur without losing the block.



## Download from the Network

To download existing data from the BitDust network, the software performs the following steps:

1. sending requests to download existing data fragments from suppliers
2. restore each block from the received fragments of data
3. decrypt each restored block and check its integrity with the digital signature
4. assemble all restored blocks into a single archive
5. extract data from the archive to the user’s hard disk drive
6. delete temporary files and the archive file

If some of the fragments were lost, for example due to connection problems with some of your suppliers, the missing Data fragments are reconstructed from the available Parity fragments during the restore of each block - see step 2 above. The same RAID procedure is used for [automatic data rebuilding](rebuilding.md).


## Hardware Resources 

All nodes in the BitDust network run compatible software which follows the same BitDust protocol, are equal in their possible roles - any node can take any of the supporting roles - and are self-governed.

Each and every user always voluntarily starts the BitDust program on their own device and possesses full control over the BitDust software process. So everyone has the opportunity to consume the hardware resources of other nodes on the one hand, and to donate part of their own PC resources to other users in the network on the other. This means that besides being a consumer you can also fulfill a supporting role within the BitDust network, depending on your hardware. The BitDust software should take care of all operations going on on your device, and all processes are/will be fully automated.

Finally, you can request and consume the following hardware resources from other users:

+ space on the hard disk drive of your "suppliers"
+ CPU time of your "ReBuilders" (a planned role, not implemented yet)
+ network bandwidth of your "routers"

In the program settings a user can set the following key data:

+ storage quota requested from others for your own use
+ desired number of suppliers for your distributed storage

These program settings are from a user's perspective. We will be working on an easy-to-use application for all the supporting roles within the BitDust network. You can read more about these supporting roles, and how to set them up from the command line, in the [Introduction](intro.md) and [Running BitDust supplier node](supplier_node.md) sections.

