# Data Security

Security is one of the most important principles for BitDust. In this section you can read more about the different measures we take to make sure your data is safe and secure. Consisting out of the following items:

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

* Data encryption algorithm: `DES3`
* Session key size: `24` bytes
* Key session encryption algorithm: `RSA`
* Possible RSA key sizes: `1024`, `2048` and `4096` bit
* Hash-function for digital signature: `SHA1`


## Private Key

Data security in the BitDust system is based upon a crypto-system with an open key. For data encryption and digital signature creation this asymmetric algorithm uses a key consisting out of two parts: A public key and a private key. It is a secret key and only this key protects authenticity and the user's personal data.

The private key is created the first time you start the BitDust program. The original key is stored on the hard disk drive in the folder `.bitdust/metadata/mykeyfile`.

On each start of BitDust program the key will be loaded into the operational memory of the computer and will be used for user data protection. After  the program stops, this temporary copy of the private key will be automatically deleted during the memory cleaning.


#### ATTENTION!
    It is highly recommended to make several backup copies of your private key on record using different methods.
    Hide your private key in safe places just after the secret key generation.
    Only this secret key gives an access to user data!
    Losing the secret key means that you lose all of your data and you will never be able to access it!!!
    Never reveal your secret key in open or untrusted channels! 
    The reliability of your personal information security is completely dependent
    on the safety of your private key.


## Public Key

The scheme of the two-way user authentication within the BitDust network is described in the section User Identification.

The Public part of a user key or, in other words, "open" key is stored in [identity](glossary.md#identity) file which is open for everyone and serves as a kind of personal passport. These files can be freely allocated in the Internet and have unique address – IDURL.

A Public key is used for encryption of incoming data which needs to be transmitted via Internet from a remote machine to machine by the user. Only with the help of a secret key which is stored only within the user’s machine you wil be able to decrypt the data.

Thus a Public key must be forwarded to all the nodes with which you want to maintain a safe connection. For this purpose the BitDust program has a function for distributing identity files of the user through the network.


## Digital signature

All user identity files are secured with a digital signature. 
By creating or changing a user identity the generation of a digital signature takes place – it is connected to the file content and serves for security when changes may be applied.

The Digital signature is calculated due to the file content via Hash-function, for this needs to have a private user key which is stored only on the user's machine. Thus only the user has a possibility to make changes to the publicly available "passport" and only when having the secret key. 

The general principle of protecting the user identity from changes is based on the fact that all other nodes check the correspondence between the digital signature and the file content each time when they receive a new copy of its identity – this standard method in cryptography is called VerifySignature.


## Service Traffic

Digital signature is used not only for protection of a user identity file from modifications but also all service data which are forwarded from one node to another in the BitDust network. 

All users exchange so-called "service packages", which include a digital signature and are safe from modifications in the process of forwarding via the Internet. This enables data dibbling security as all received packages will be checked for the correspondence of content and the digital signature.

Service packages are forwarded in the not encrypted form and still can be marked as signed packages. The structure of a signed package is the following:

* `Command` – BitDust protocol command name
* `OwnerID` – package owner IDURL 
* `CreatorID` – IDURL of the creator, whose key signed the package 
* `PacketID` – package identifier 
* `Date` – creation date and time  
* `Payload` – useful data in source (not encrypted) form 
* `RemoteID` – IDURL of addressee 
* `Signature` – digital signature


## Data Transfer

To hide the data another class of packages is used, it is called "encrypted block".
These data blocks are forwarded inside the service packages and the field `Command` will have a value "Data" – which means that the given package contains the encrypted data block.

You can view this mechanism as two layers where a internal encrypted layer is somehow "put" inside the external one – or the service one: 

1. external layer allows forwarding of the data which is secured from modifications with the digital signature
2. internal layer allows hiding them with the help of encryption

The structure of a encrypted block is the following:

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

At the moment the encryption of data recorded in the field `EncryptedData` is done by the DES3 algorithm.

This symmetric algorithm uses a so-called session key for data perturbation – anyone who knows the key can decrypt them. The Session key is generated for each new data block and is used for encryption of only this block. Data security is based on hiding the key itself – it is encrypted with the help of a receiver’s public key. As a result the structure of the encrypted package doesn't get the Session key itself but its encrypted copy, which is recorded in the field `EncryptedSessionKey`. Encrypted data is recorded in the field `EncryptedData`.

Thus the RSA algorithm encrypts only the Session key, but the data itself is secured with the DES3 algorithm. Such an approach allows increasing the performance of the BitDust program, as the symmetric method of encryption DES3 works much faster than the algorithm RSA. In other words the simple combination of two algorithms is used:

+ symmetric DES3 encryption
+ asymmetric RSA enctryption

We are looking forward to adding a support of AES3 and BlowFish algorithms and provide users a chance to choose their desired method of symmetric encryption in the program settings.


## Storage

The Session key is not available to anyone except for the use – the user and only the can decrypt the field `EncryptedSessionKey` with the help of the private key and extract the Session key. Thus a user data which gets to the supplier’s machine are kept in an encrypted form and they are not available to be read by any supplier.

A supplier receives data portions inside service packages, in the field `Payload`, and records the data completely to different files on their hard disk drive. As a result the encrypted block portion are allocated on the supplier’s node – the field `EncryptedData` is kept the same as when it was created on the machine of the user.

When a user requests their data back from a supplier machine they are back in the source encrypted form, but again wrapped in the service pack structure for changed security. Afterwards the user decrypts the field `EncryptedSessionKey` (with their private key) and extracts from the session key, which then decrypts the source data block from the field `EncryptedData`.


## Data Integrity

You can upload some data into the BitDust network and immediately remove the source file from your computer - now only you can restore it again using your Private Key. Since that moment, that same file (or its copies) will no longer exist in the Universe!

The information uploaded to the BitDust network loses its integrity, as each supplier allocates on their machines only encrypted blocks. Nodes storing your data can be identified, but they are located in different parts of the World and reading your data is only possible by using private key. 

One might speak of uploaded in the network data going through the process of "data disintegration" – it becomes absolutely useless for everyone except for the data owner. Only the user has the private key and can reassemble them by providing the baseline minimum quantity of fragments in each block.


# Full Control

In addition to the  possibility of storing your data distributed on remote machines, BitDust also offers another absolutely unique possibility to have total control over your distributed space and secure your data from __theft__ or __elimination__.

It is possible to organize the fully secured storage of a user data so that it will be absolutely unavailable for anyone apart from the owner. The user and only the user will have the key to the only existing copy of this data, allocated on multiple machines within the BitDust network. These nodes store only encrypted fragments of the general data array and cannot read them or use them in any way.  

It is totally fine to just delete the original file from your hard disk drive after the data upload to the BitDust network has been finished.

__NOTE__: In the beginning we will be growing the BitDust network with supporters. We would require a high number of supporters to guarantee availability of your data.

Further, the private key of the user can be stored on a USB-stick and the local copy of the private key can be deleted from your local disk. The BitDust program will be started on the given computer only if this USB-stick is plugged in and the software will read the secret key by the start and store it in the computer operation memory, without creating a copy on the local disk drive. Now, after launching the program the user extracts USB-stick and hides it in a safe place. So USB-stick acts as a normal "physical" key - just like a key for your car.

In addition to portative USB-stick we recommend you to print the private key on a regular A4 paper and keep it in a safe place. In this case the user provides additional security if the USB-stick breaks down for example. We also advice you to put a password in place and encrypt your private key stored on your USB-stick, so that even if you lose it you are still safe.

This approach enables practically the same common mode of operations with data, which is usually stored on a cloud server for example, but gives a significantly higher level of control over it. When data is stored on cloud servers it is under the control of the respective service suppliers, even if it was encrypted on the client side. Indeed, data can be encrypted by the user before it is put on the cloud server. Still the data is stored in data centers in an integrated form and can be read (in most cases it is still not encrypted), deleted, taken away without notice. Another aspect here is that access/traffic to your customer account can be monitored by a service provider in real time - they will know exactly when, who, and which file was uploaded/downloaded.

Our given approach is unique, after uploading personal user data within the BitDust network and deleting th original data from your local hard disk drive there is no physical integral copy available any more any where. Data is stored distributed on remote computers are just fragments of source data and they are useless if taken separately – they undergo a process of "data disintegration". 
Restoring the source user data is possible only with the help of the private key which is kept on user’s USB-stick or possibly on additional media. 

We believe firmly that your most private data should only be read and used by you and we want to provide you with the full control to make this possible.
