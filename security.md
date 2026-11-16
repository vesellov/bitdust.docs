# Data Security

Security is one of the most important principles for BitDust. In this section you can read more about the different measures we take to make sure your data is safe and secure. It consists of the following items:

* [Brief Summary](#brief-summary)
* [Private Key](#private-key)
* [Public Key](#public-key)
* [Digital signature](#digital-signature)
* [Service Traffic](#service-traffic)
* [Data Transfer](#data-transfer)
* [Encryption](#encryption)
* [Storage](#storage)
* [Data Integrity](#data-integrity)
* [Full Control](#full-control)


## Brief Summary 

* Data encryption algorithm: `AES` (CBC mode)
* Session key size: `16` bytes (128 bits)
* Session key encryption algorithm: `RSA` (PKCS#1 OAEP)
* Possible RSA key sizes: `1024`, `2048` and `4096` bits
* Hash-function for digital signature: `SHA1`


## Private Key

Data security in the BitDust system is based on public-key cryptography. For data encryption and digital signature creation this asymmetric algorithm uses a key consisting of two parts: a public key and a private key. The private key is secret, and only this key protects the authenticity and the personal data of the user.

The private key is created the first time you start the BitDust program. The original key is stored on the hard disk drive in the folder `.bitdust/metadata/mykeyfile`.

Each time the BitDust program starts, the key is loaded into the computer's memory and used to protect user data. After the program stops, this temporary copy of the private key is automatically deleted when the memory is cleared.


#### ATTENTION!
    It is highly recommended to make several backup copies of your private key using different methods.
    Hide your private key in safe places right after the secret key is generated.
    Only this secret key gives access to user data!
    Losing the secret key means that you lose all of your data and you will never be able to access it!!!
    Never reveal your secret key in open or untrusted channels! 
    The reliability of your personal information security is completely dependent
    on the safety of your private key.


## Public Key

The scheme of user authentication within the BitDust network is described in the section [How authentication works](identities.md#how-authentication-works).

The public part of a user key, or in other words the "open" key, is stored in the [identity](glossary.md#identity) file, which is open to everyone and serves as a kind of personal passport. These files can be freely placed on the Internet and have a unique address – IDURL.

A public key is used to encrypt incoming data that needs to be transmitted via the Internet from a remote machine to the user's machine. Only with the help of the secret key, which is stored only on the user’s machine, will you be able to decrypt the data.

Thus a public key must be forwarded to all the nodes with which you want to maintain a safe connection. For this purpose the BitDust program has a function for distributing identity files of the user through the network.


## Digital signature

All user identity files are secured with a digital signature. 
When a user identity is created or changed, a digital signature is generated – it is bound to the file content and protects it against unauthorized changes.

The digital signature is calculated from the file content via a hash function, which requires the user's private key stored only on the user's machine. Thus only the user, and only when having the secret key, is able to make changes to the publicly available "passport".

The general principle of protecting the user identity from changes is based on the fact that all other nodes check the correspondence between the digital signature and the file content each time they receive a new copy of the identity – this standard method in cryptography is called VerifySignature.


## Service Traffic

A digital signature is used not only to protect a user identity file from modifications, but also to protect all service data that is forwarded from one node to another in the BitDust network. 

All users exchange so-called "service packages", which include a digital signature and are safe from modifications in the process of forwarding via the Internet. This protects data from tampering, as all received packages are checked for the correspondence between their content and the digital signature.

The service information of a package is forwarded in unencrypted form, but it is signed; private data is carried inside the package only in encrypted form (see [Data Transfer](#data-transfer)). The structure of a signed package is the following:

* `Command` – BitDust protocol command name
* `OwnerID` – package owner IDURL 
* `CreatorID` – IDURL of the creator, whose key signed the package 
* `PacketID` – package identifier 
* `Date` – creation date and time  
* `Payload` – useful data; private data is placed here only in encrypted form 
* `RemoteID` – IDURL of addressee 
* `Signature` – digital signature


## Data Transfer

To hide the data, another class of packages is used, called an "encrypted block".
These data blocks are forwarded inside the service packages and the field `Command` has the value "Data" – which means that the given package contains the encrypted data block.

You can view this mechanism as two layers, where the internal encrypted layer is "put" inside the external, or service, one:

1. the external layer allows forwarding the data, which is secured from modifications with the digital signature
2. the internal layer allows hiding the data with the help of encryption

When the traffic is routed via a proxy-router (the "proxy-transport" network service, enabled by default, see [User Connections behind NAT](principle.md#user-connections-behind-nat)), a third layer is added:

3. the whole service package is encrypted for the proxy-router; the router adds its own encryption and digital signature when forwarding the package further. The router is able to see the service information needed for delivery, but it is not able to read your private data (internal layer) or to change the service information (protected by your digital signature in the external layer)

In short: every package is digitally signed, your private data inside the packages is always encrypted, and the service information needed for delivery - command, sender, recipient - is signed but not encrypted, except while it travels between your device and a proxy-router.

The structure of an encrypted block is the following:

* `CreatorID` – IDURL of data block creator
* `BackupID` – data identifier
* `BlockNumber` – block number in the chain
* `SessionKeyType` – algorithm used for encryption
* `EncryptedSessionKey` – encrypted session key
* `Length` – actual length of not encrypted data in that block
* `LastBlock` – last block in the data chain marker
* `EncryptedData` – encrypted useful data
* `Signature` – digital signature


## Encryption

At the moment the encryption of data recorded in the field `EncryptedData` is done by the AES algorithm in CBC mode.

This symmetric algorithm uses a so-called session key to encrypt the data – anyone who knows the key can decrypt it. The session key is generated for each new data block and is used to encrypt only this block. Data security is based on hiding the key itself – it is encrypted with the receiver’s public key. As a result, the encrypted package contains not the session key itself but its encrypted copy, which is recorded in the field `EncryptedSessionKey`. Encrypted data is recorded in the field `EncryptedData`.

Thus the RSA algorithm encrypts only the session key, while the data itself is secured with the AES algorithm. This approach increases the performance of the BitDust program, as symmetric AES encryption works much faster than RSA. In other words, a simple combination of two algorithms is used:

+ symmetric AES encryption
+ asymmetric RSA encryption

The field `SessionKeyType` stores the name of the symmetric algorithm used for a given block. Earlier versions of BitDust used the DES3 algorithm - the software is still able to decrypt such blocks, but all new data is encrypted with AES.


## Storage

The session key is not available to anyone except the user – only the user can decrypt the field `EncryptedSessionKey` with the help of the private key and extract the session key. Thus user data that gets to the supplier’s machine is kept in encrypted form and cannot be read by any supplier.

A supplier receives data portions inside service packages, in the field `Payload`, and writes the data to separate files on their hard disk drive. As a result, the encrypted block portions are stored on the supplier’s node – the field `EncryptedData` is kept the same as when it was created on the machine of the user.

When a user requests their data back from a supplier's machine, it comes back in its original encrypted form, but again wrapped in the service package structure. Afterwards the user decrypts the field `EncryptedSessionKey` (with their private key) and extracts the session key, which then decrypts the source data block from the field `EncryptedData`.


## Data Integrity

You can upload some data into the BitDust network and immediately remove the source file from your computer - now only you can restore it again using your [Master Key](glossary.md#master-key) (or, if the data was encrypted with an [Additional Key](glossary.md#additional-key), also the trusted contacts you gave a copy of that key to). From that moment, that same file (or its copies) will no longer exist in the Universe!

The information uploaded to the BitDust network loses its integrity, as each supplier stores only encrypted blocks on their machine. Nodes storing your data can be identified, but they are located in different parts of the world, and reading your data is only possible with the private key.

One might say that data uploaded to the network goes through a process of "data disintegration" – it becomes absolutely useless for everyone except the data owner. Only the user has the private key and can reassemble the data, provided that the minimum required number of fragments in each block is available.


## Full Control

In addition to the possibility of storing your data distributed on remote machines, BitDust also offers another unique possibility to have total control over your distributed space and secure your data from __theft__ or __elimination__.

It is possible to organize fully secured storage of user data so that it is absolutely unavailable to anyone apart from the owner. The user and only the user will have the key to the only existing copy of this data, allocated on multiple machines within the BitDust network. These nodes store only encrypted fragments of the general data array and cannot read them or use them in any way.  

It is totally fine to just delete the original file from your hard disk drive after the data upload to the BitDust network has finished.

__NOTE__: In the beginning we will be growing the BitDust network with supporters. We need a large number of supporters to guarantee the availability of your data.

Further, the private key of the user can be stored on a USB-stick and the local copy of the private key can be deleted from your local disk. The BitDust program will then start on the given computer only if this USB-stick is plugged in: the software reads the secret key at startup and stores it in the computer's memory, without creating a copy on the local disk drive. After launching the program, the user removes the USB-stick and hides it in a safe place. So the USB-stick acts as a normal "physical" key - just like a key for your car.

In addition to a portable USB-stick, we recommend that you print the private key on a regular sheet of A4 paper and keep it in a safe place. This provides additional security, for example if the USB-stick breaks down. We also advise you to set a password and encrypt the private key stored on your USB-stick, so that even if you lose it you are still safe.

This approach enables practically the same mode of operations with data as, for example, with data stored on a cloud server, but gives a significantly higher level of control over it. When data is stored on cloud servers it is under the control of the respective service suppliers, even if it was encrypted on the client side. Indeed, data can be encrypted by the user before it is put on the cloud server. Still, the data is stored in data centers in an integral form and can be read (in most cases it is still not encrypted), deleted or taken away without notice. Another aspect is that access/traffic to your customer account can be monitored by the service provider in real time - they will know exactly when, by whom and which file was uploaded/downloaded.

Our approach is unique: after uploading personal user data to the BitDust network and deleting the original data from your local hard disk drive, there is no physical integral copy available anywhere any more. The data stored on remote computers is just fragments of the source data, which are useless if taken separately – they undergo a process of "data disintegration". 
Restoring the source user data is possible only with the help of the private key, which is kept on the user’s USB-stick or possibly on additional media. 

We firmly believe that your most private data should only be read and used by you and we want to provide you with the full control to make this possible.
