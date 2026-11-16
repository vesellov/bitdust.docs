# User Identification

In this section you can read about user identification and how it is done within the BitDust network.


* [Identity files](#identity-files)
* [ID-servers](#id-servers)
* [IDURL address](#idurl-address)
* [Hosts rotation](#hosts-rotation)
* [Global unique entity ID](#global-unique-entity-id)
* [Digital signature](#digital-signature)
* [How authentication works](#how-authentication-works)
* [Private key](#private-key)
* [Your privacy](#your-privacy)


## Identity files

For other nodes to connect to a user's machine, they have to know the address of the user's computer on the Internet. These contacts are stored in an XML file called __Identity__. When connecting to other nodes, their identity files are downloaded and the received information is used for the transfer of service packets - by default via the proxy-router of the remote node, or directly if routing is disabled there (see [User Connections behind NAT](principle.md#user-connections-behind-nat)).

In the user identity file the following information is stored:

* contact data of network protocols for connecting to the user's machine
* the public part of the user's personal key
* a digital signature protecting the file content from modifications
* a list of IDURL addresses on the Internet at which the file is available
* the date of file creation
* the version of the BitDust software running on the user's computer

To join the BitDust network, each user needs to create a personal identity file – this happens automatically when you create an account in the BitDust program the first time it starts.



## ID-servers

In theory a user's identity file can be located anywhere; however, it is important that it is easily accessible to all other nodes in the network. For example, a user can put the file on their personal website. In that case they need to make sure that this copy can easily be updated from the original, which is stored on the user's machine. To guarantee a more secure and fault-tolerant interaction between nodes in the network you can store multiple copies of your identity in several places. 

This kind of storage is done using multiple machines, called ID-servers or Identity servers, on which copies of identity files are stored. Despite the word "server" in the name, ID-servers do not form a central storage of identities: there are many ID-servers in the network owned by different people, every user keeps copies of their own identity file on several ID-servers at once and can move it to other ID-servers at any time. These servers are maintained by members of the BitDust community and any user can start an ID-server on their own machine. A list of "well known" ID-servers is published in the [default_network.json](https://github.com/bitdust-io/public/blob/master/default_network.json) file.

An ID-server is a part of the BitDust program, but by default it is turned off in the program settings. If required, each user can turn on this option and start such a server on their own computer – afterwards the ID-server will start automatically together with the BitDust program. An ID-server consumes almost no computer resources, but allows you to support other nodes and increase the overall fault-tolerance of the entire BitDust network.

If you feel enthusiastic now, you can read more about how to run an identity server in the BitDust network. See the [Start new Identity Server](identity_server.md) page or how to run a [Full Seed node](seed_node.md) from scratch.

If you plan to maintain your BitDust node for a while and support the network, it makes sense to include your node in the list of "well known" nodes, which are hard-coded in the [default_network.json](https://github.com/bitdust-io/public/blob/master/default_network.json) file.

You can also fork the [Public Git Repository](https://github.com/bitdust-io/public), modify the `default_network.json` file in your forked repository and open a [Pull Request](https://github.com/bitdust-io/public/pulls) with your changes - this way we can collaborate all together and maintain a list of the most reliable BitDust Identity servers.

Contact the BitDust contributors to notify them about starting a new Identity Server and one of the developers will approve your Pull Request.



## IDURL address

Each BitDust user is identified by an IDURL key – this is the address of the identity file on the global Internet. This identifier is related to the device running the BitDust software, not to the person who manages this device. Only by using the Private Key stored on that device can you change your identity in the network.


An example IDURL might look like this:

        http://first-machine.com/alice.xml

 
And the identity file itself looks like this:

        <identity>
        <contacts>
          <contact>tcp://123.45.67.89:7771</contact>
          <contact>udp://alice@first-machine.com</contact>
        </contacts>
        <sources>
          <source>http://first-machine.com/alice.xml</source>
          <source>http://server-two.net/alice.xml</source>
        </sources>
        <certificates/>
        <scrubbers/>
        <postage>1</postage>
        <date>Sat Dec 13 14:08:36 2014</date>
        <version>
          sources Linux-3.11.0-15-generic-i686-with-Ubuntu-12.04-precise
        </version>
        <publickey>
          ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgGcYwhNipj6yJyEaD77qLNrJ ...
        </publickey>
        <signature>
          1557824826897671730886854628410961132377100189021332866347553795 ...
        </signature>
        </identity>


This is an example of an older identity file of a node with routing disabled: the contacts contain the node's own IP address. By default, with routing via a proxy-router enabled, the contacts contain the network address of the router instead. The UDP transport shown in the example is temporarily disabled in current versions.

Each identity file can have several copies stored on different servers – to ensure the fault tolerance of user identification within the BitDust network. Thus each node can have several IDURL addresses by which it can be identified.

A user is able to create an authentic identifier within the BitDust network – for example, by starting an ID-server on the host of their website and placing their personal identity file there. For instance:

        http://veselin-penev.com/id.xml


It is preferable that machines running ID-servers have properly configured domain names. Then the IDURL identifiers of the users whose identity files are stored there will look more attractive.
    
But of course it is not mandatory to buy a domain name to run your own ID-server. In that case ID files on your host may look like this:
   
        http://123.45.67.89/id.xml

    
For some reasons this can be preferable, for example if you do not wish to depend on global DNS providers.

The system is designed in such a way that even if one or more servers that store your identity file fail, other users can download it from other ID-servers and are able to connect with you. The program continuously monitors your ID-servers and, in case of failure, triggers automatic distribution of your identity file to other available ID-servers, and notifies your active nodes immediately.



## Hosts rotation

Periodically the software triggers a method to check and propagate your identity file to other nodes. Before it starts, it can switch your current "primary" ID-server to another one - another ID-server will be found and your identity file will be sent to that host. Your main global IDURL address will change, because the new server will be placed in the first position under the `<sources>` tag, for example:

        <identity>
        <contacts>
          <contact>tcp://123.45.67.89:7771</contact>
          <contact>udp://alice@host3.org</contact>
        </contacts>
        <sources>
          <source>http://host3.org/alice.xml</source>
          <source>http://first-machine.com/alice.xml</source>
          <source>http://server-two.net/alice.xml</source>
        </sources>
        <certificates/>
        <scrubbers/>
        <postage>1</postage>
        <date>Sat Dec 13 14:08:36 2014</date>
        <version>
          sources Linux-3.11.0-15-generic-i686-with-Ubuntu-12.04-precise
        </version>
        <publickey>
          ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgGcYwhNipj6yJyEaD77qLNrJ ...
        </publickey>
        <signature>
          1557824826897671730886854628410961132377100189021332866347553795 ...
        </signature>
        </identity>


The final IDURL will be updated like so:

      http://host3.org/alice.xml


Both IDURLs now point to the same device, but we only switch the main location of your identity file - to be used by all other nodes that are talking to you. To do that, the software will "propagate" your new identity file to all of your currently active contacts. Also it will update copies on ID-servers you are using already: "first-machine.com" and "server-two.net".

The software will try to keep your "nickname" unchanged: for example, if the "host3.org" server already stores another "alice.xml" file signed with another public key, it cannot be used by you, because this name is already taken. A nickname is the name you pick in the beginning and can never be changed.

We expect a regular BitDust user to be able to start their own ID-server easily - a lot of "hosters" are required for the network to operate in a reliable, private and safe way.

BitDust software that is running on your machine will automatically "migrate" your identity between ID-servers in the network, which makes it much harder to "block" or "attack" your presence in the network.



## Global unique entity ID

BitDust supports a global unique ID for any existing object (file, group, user, etc.).
This is an example of a full remote path to file "cat.png" within the BitDust network:

        group_abc$alice@first-machine.com:animals/cat.png#F20160313043757PM


A full identifier consists of several parts, for instance:

* identity server host (DNS name or IP:PORT address): first-machine.com
* user identity filename (without ".xml"): alice
* key ID: group_abc
* backup ID: animals/cat.png
* version name: F20160313043757PM


If an identity server was set using the IP:PORT format, the global ID might look like this:

        key_xyz$bob@123.45.67.89_8000:cars/citroen.png#F20160313052124PM


This approach has something in common with "remotes" and "branches" in Git. For example, a full remote file path in Git can be written like so:

        https://github.com/vesellov/bitdust.devel/blob/master/automats/automat.py


Looking more deeply, we can split this URL into several parts:

* server host: github.com
* user account ID: vesellov
* repository ID: bitdust.devel
* branch ID: master
* file path: automats/automat.py



## Digital signature

When a user's identity is created or changed, a digital signature is generated – it is bound to the file content and protects it from any unintended modifications by third parties. The digital signature is calculated from the file content via a hash function, and to create it you need the Private Key, which is stored only on your machine.

The general principle of protecting a user identity from changes is based on the fact that all other nodes always check that the digital signature matches the file content each time they get a new copy of the identity – this is a standard method in cryptography called `VerifySignature`.

This framework helps to safely store and distribute user identity files through any sources, including ID-servers. The owner of a machine running an ID-server has no possibility to substitute the content of the files stored there. But they can easily delete them or turn off the server – to prevent the loss of the identity file, each user's file is stored on several servers all over the world.



## How authentication works

There is no "login" procedure and no authentication server in the BitDust network. Authentication is fully based on digital signatures and is performed independently by every node:

* the user's private key (the [Master Key](glossary.md#master-key)) is generated and stored only on the user's device - it never leaves it
* the user signs their own identity file and every packet sent to the network with that private key
* the identity file contains the user's public key and is published on several ID-servers
* any node that receives a packet downloads the sender's identity file (or takes it from the local cache), checks the signature of the identity file itself and then verifies the signature of the packet with the public key from that identity file
* if the signature is valid, the node knows that the packet was created by the owner of that identity and was not modified on the way

ID-servers only store and publish copies of identity files - they do not authenticate anyone and are not able to forge an identity file, because they do not have the user's private key.

Thus two nodes are able to authenticate each other directly, without any third party being involved in the decision.



## Private Key

When the BitDust program is first installed, the private key is generated; this key protects the identity file and your personal data. Only this key can grant you access to the user's personal data and/or allow changes to the identity file.

In BitDust you will not find any of the common methods of user authorization:
 
* no password
* no email address
* no account recovery by answering a secret question
* no calling a support team
* no centralized storage of member accounts


#### ATTENTION!!!
    If you lose your private key, you completely lose access to all your uploaded files.
    It is highly recommended to make two or three copies of your private key on reliable and
    compact media right after the program setup and hide them in a secure place.

The user is responsible for the security of their private key. Apart from these backup copies, the Master Key should NEVER leave the computer on which it was generated – this could have a major impact on the security of your data.

We recommend using modern anti-virus software or other tools to arrange extra defence against network attacks, as well as other methods of increasing the security level of your personal device. In addition, we advise you to look through useful information resources on this topic in order to raise your awareness.



## Your privacy

Your identity file is distributed publicly on the network and at the same time is a unique source that allows other users to connect to your device.
The file does not store your personal data (name, surname, address, etc.) but contains technical and service information (nickname, IP address, RSA public key) required for BitDust to operate.

The BitDust network protocol is a [pseudonymous peer-to-peer](https://en.wikipedia.org/wiki/Anonymous_P2P) communication system: BitDust does not collect any personal data of its users, every user is known in the network only by a nickname and a public key.

While registering a new identity, you will choose a "nickname" that will never change during the lifetime of that identity.
Your nickname may represent your real name or be a random string. It can be the name of an animal or a president or anything else you wish other users to see when communicating with you via BitDust.

Your identity file also stores networking information, such as an IP address and TCP port number. By default routing via proxy-routers is enabled, so the file contains the network address of your proxy-router instead of your own - the router itself still knows your real IP address. If you disable routing, the file contains the IP address of your own device, which may reveal your "physical" location; in that case you must also configure port forwarding and open ports so that your node can receive incoming connections, see [User Connections behind NAT](principle.md#user-connections-behind-nat). Hiding the physical location of the user is not a goal of the BitDust project.

If you wish to further reduce the disclosure of your networking info, there are a few optional possibilities in BitDust:

* Run your own BitDust ID-server node on an external device (whose physical location may be disclosed - it acts as a "shield") and re-configure your "master" device using [known-servers](https://bitdust.io/wiki/settings.html#services/identity-propagate/known-servers-(string)) and [preferred-servers](https://bitdust.io/wiki/settings.html#services/identity-propagate/preferred-servers-(string)) settings before registering a new identity in the network
* Run your own BitDust proxy-router node on an external device (whose physical location may be disclosed - it acts as a "shield") and re-configure your "master" device using [preferred-routers](https://bitdust.io/wiki/settings.html#services/proxy-transport/preferred-routers-(text)) setting - this way the IP address of your "master" device is not visible to any other nodes in the network at all
* Use VPN services to enable lower-level networking protection on your device, and BitDust will work on top of that via the same TCP protocol - other users will see the IP addresses of the VPN providers, but not your own IP address in that situation

Note also that, while the blockchain functions are enabled, your nickname and public key are recorded in the public BitDust blockchain when your identity is registered there - see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts).

In other words, BitDust provides pseudonymity, not anonymity: the project guarantees that it does not collect your personal data, but it does not guarantee concealment of your physical location in the network.

