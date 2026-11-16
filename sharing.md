# File sharing

We are not isolated as human beings, and we recognize that users want to share their data with trusted contacts. In this section we explain how data is shared safely and securely through the following items:

* [Master Key](#master-key)
* [Additional keys](#additional-keys)
* [Grant access](#grant-access)
* [Audit access](#audit-access)
* [Receiving additional keys](#receiving-additional-keys)
* [Reading shared data](#reading-shared-data)
* [Customer data flow step-by-step](#customer-data-flow-step-by-step)


## Master Key

Since every piece of your personal data in the BitDust network is stored on the machines of other users, it is important to know that all data is always encrypted before it leaves your computer - the method of protection is described in detail in the article [Data Security](security.md). Your main private key (here and below we refer to it as the "Master Key") is used to encrypt and decrypt your personal data stored on the suppliers' machines.

For your personal safety, you should make a backup copy of the Master Key and put it in a safe place immediately after installing BitDust on your device. In all other cases, the Master Key should never leave your device and under no circumstances should it be transferred to anyone else. Only the Master Key protects your personal data and provides access to the BitDust network.


## Additional keys

In addition to the Master Key, there are other private keys within BitDust that you can use to manage your data. The BitDust software implements extra logic to use the same cryptographic methods with many different keys.

Using additional private and public keys in BitDust is very similar to what happens in the real world: for instance, you have multiple keys on your keychain to open and close different doors. If a good friend is visiting and you would like to let them stay in one of your guest bedrooms after a night of heavy drinking :), you will probably give them a copy of the key from your keychain so they can unlock this room. But you will still keep the original key for yourself. Thus, both of you will have access to the guest bedroom. However, there might be rooms in your house that you want to keep locked and that only you can open.

Within BitDust you can at any time create a new additional private key for sharing purposes and use it to encrypt some data that will be uploaded to your suppliers' machines. The data is stored in a distributed way, in the same way as when you encrypt data with your Master Key - this is described in detail in the article [Distributed Storage](storage.md). The key must be chosen before the upload: if you want to share a file, switch from your Master Key to an additional key before uploading it. The key of already encrypted data cannot be changed later, so data encrypted with your Master Key cannot be shared.

Later on you can transfer a copy of this additional key to another user whom you trust, and this user will be able to download this particular piece of data and decrypt it on their own device. Thus, we can exchange personal data with each other.

When working with additional keys you can perform the following actions:


* create a new key by giving it a name and storing it locally
* create a new key with a randomly generated name
* delete an existing private key
* back up and store the keys on your suppliers - they will be encrypted with your Master Key
* send the public part of a given key to another "untrusted" user
* send the private part of a given key to another "trusted" user
* view a list of all keys: created by you or received from other users
* perform an "audit" of a given public key and verify that another user also possesses exactly the same copy of that public key
* perform an "audit" of a given private key and verify that another user also possesses exactly the same copy of that private key
* use a private key to encrypt data uploaded to the BitDust network
* use a private key to decrypt data downloaded from the network


You will find your additional private keys in the `.bitdust/keys` folder.
Currently the BitDust software uses the [RSA algorithm](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) for these keys, which is a widely used encryption method; the data itself is encrypted with AES using a session key, which is in turn encrypted with RSA - see [Data Security](security.md#encryption).
These methods were implemented using the [PyCryptodome](https://pypi.org/project/pycryptodomex/) library.

Note that most of these actions are performed automatically. We just want to describe the steps BitDust performs to give you more insight into the inner workings of the software.


## Grant access

When transferring an additional key from one user to another, one of the text files from the `.bitdust/keys` folder is actually sent to the remote recipient's machine. The key is not transmitted in plaintext: before sending, it is encrypted using the public part of the recipient's Master Key - only the "trusted" user will be able to decrypt this text file and get a copy of the transmitted additional key. The BitDust program on their machine will receive the key and save it in a new text file in the `.bitdust/keys` folder. When you share an additional key with another user, you are basically giving that user access to all your data that is encrypted with that particular key. The recipient will automatically see which files you shared in the BitDust program (section "shared files").

It is important to understand that this action is "irreversible"! You cannot "take" the key back after you have handed it to the recipient. In fact, after you have passed the key to another user, both of you are "almost equal" owners of the data that was encrypted with that additional key.

We say "almost equal" ownership because, when you share data, the data is still stored on your suppliers, not on the suppliers of the user you shared it with. So you always have "more rights" in your own distributed storage than anyone else. For example, at any time you can ask your suppliers to delete the files you shared with other users. But be aware that the user you shared data with could have downloaded the file before you requested its deletion from your suppliers.



## Audit access

The most important role of the supplier is to store and provide access to files. To achieve this, each supplier must have specific instructions and a well-defined order of actions that they must perform when you or a "trusted" user request the distributed "shared" data stored on their device. Since all data that the suppliers store is encrypted, even if suppliers sent fragments of your data to third parties you could still remain calm - only those who possess the right key will be able to "read" your files.

However, the BitDust software implements methods for filtering and validating "access requests" for all data uploaded to the network. By default the supplier will "check" that the one who requests a particular piece of data has the correct additional key to decrypt it. The supplier must "audit" the user's key that corresponds to the data in question. To do this, the supplier must keep the public part of the key that was used to encrypt the data. This public key is usually received from the data owner exactly at the moment when you grant access to the data to the "trusted" user.

In short, below is the general plan implemented in the BitDust software, which executes on your device when you transfer one of your additional keys to another user:


1. checking the connection status of the trusted user's computer
2. audit of the master key of the trusted user
3. sending the public part of the additional key to all your suppliers
4. forwarding the private part of the additional key to the trusted user
5. forwarding a list of file names that have already been encrypted with this additional key to the trusted user



## Receiving additional keys

In the previous paragraphs, we described methods for transferring access rights to your data to another user whom you trust. Next, you will find detailed information about what happens on the device of the trusted contact and how they can use the additional key they received from you, the user, to download the "shared" data.

It is assumed that the BitDust program is running on both devices and is connected to other nodes within the BitDust network at the time when the additional key is transferred. If the "donor" or the "recipient" is not connected to the network and does not have the opportunity to exchange service packages directly via the BitDust "channels", the process will fail and no access will be granted at all. In short, both parties must be online to share data. We are currently looking into the ability to re-use existing keys for sharing purposes. This enables offline sharing when you already shared at least one key with each other.

BitDust on the machine of the "trusted" user is constantly in a waiting state to receive incoming service data packets and is always ready to accept new incoming access keys. After the file is received and decrypted with the recipient's own Master Key, it is stored in the `.bitdust/keys` folder. You can always find the full list of all access keys that have been passed to you by other users - it's important to note that each key has a unique global identifier that looks like this:

        share_0780f0d64b017303a0d81865eee009f1$veselin_penev@some-id-hostname.net


The initial part of the identifier before the character `$` is called the "key alias" and serves to identify a particular key among a set of additional keys created by this user. The second part after the `$` character is the unique global identifier of the user who first generated the key. You can read more about the identification of users in the BitDust network in the article [User Identification](identities.md).

This method of identifying access keys always uniquely defines who is the creator of the data and on whose suppliers the encrypted fragments of files in the BitDust network are actually stored. In other words, having received a new access key from someone, by default you will always know where the "shared" files are located.



## Reading shared data

To access the "foreign" files, you need to use the key identifier to determine the "location" of the data. We use the word "location" in a figurative sense, since all data is stored completely distributed on the suppliers' machines. To download fragments of files and decrypt them, it is necessary to determine the currently active suppliers, connect to them and request all the fragments of data related to the given "common" file.

In BitDust, the list of your suppliers is stored and maintained not only on your own device, but also in a distributed hash table - your suppliers automatically support and periodically update these "globally distributed" records. Thanks to this, any user always has the ability to "scan" the list of your suppliers and find out where they need to go to download the files to which you "granted access". The process of "scanning" that list takes some time, depending on the number of suppliers you have. You can read in more detail how BitDust uses these service records in the article [Distributed Hash-Table](dht.md).

After connecting with the "foreign" suppliers, you can request the necessary pieces of data related to a particular "shared" file. If the access key you received from the "donor" is valid, "external" suppliers will accept your request and the BitDust software on your device will receive the needed fragments, decrypt them, combine them into one chunk and create an exact copy of the "shared" file to which the "donor" granted you access.



## Customer data flow step-by-step

Let's try to describe everything that happens with your data step by step at the moment of transferring access to another user.

* The BitDust program is already running on your device
* You have already generated a private key and a new identity for yourself in the network
* In the application you click the "Create a new Share" button and the software creates a new virtual location in your files catalog called a "share"
* A new additional private key is generated automatically and you are able to verify the key in the app
* Via the "Share Key" button in the app you can transfer a copy of the key to another device/person - this is supposed to be done in the most secure way possible
* Your trusted person is also running BitDust and also has an identity in the network
* When they receive a copy of the additional private key of that "share" (never your Master Key), they keep it in their own files catalog as well and are able to see the files listed in this "share"
* Both of you can easily upload & download files into that "shared" location via the app
* Each uploaded data fragment is stored on your own suppliers - on the suppliers hired by the creator of the "share"
* All suppliers also keep in touch with both you and the other user - they are able to notify you both about any updates to the "share"

The software is supposed to handle all of the communications and transfers automatically, as well as store the data as reliably and securely as possible.

This whole process happens in the application peer to peer between devices running the BitDust software and connected to the Internet.

The engine sends and receives signed data packets with encrypted private data via the BitDust network, and the app provides the user interface for a specific platform.

