# Your own BitDust Network from scratch

In this section, you will learn how to create and maintain your own fully isolated and private BitDust network under your complete control.

It is also very easy to set up a completely public network, which is fully open for everyone to join and communicate together.

Here we assume that you have basic Linux and system administration skills. You don’t have to be a Python expert to manage BitDust, but you need to know how to use SSH, how a Debian system works and what a "web server" actually is.



## Infrastructure requirements

You will need several computers connected to the Internet; these can be:

* dedicated or virtual servers
* cloud servers
* home or office computers - make sure port forwarding is configured in that case

It is also recommended that you already have domain names assigned to your hosts, so that you can use the DNS names for your hosts instead of the public IP addresses.

In order to show the whole process of creating a BitDust network from scratch, we will use an example of a few computers that will be the core of our new network. Any number of other nodes can join later, as soon as the BitDust core network is configured.

Suppose you already have 4 Debian Linux servers under your control with SSH access and these domain names:

* host-a.com
* host-b.com
* host-c.com
* host-d.com



## Part 1: install BitDust software

Connect to each machine via SSH one by one and perform the following steps to install the BitDust software.

First, install all dependencies with the APT package manager:

    sudo apt-get update
    sudo apt-get install git gcc python3-dev python3-virtualenv python3-pip


Then, using Git, clone the public BitDust repository directly from GitHub:

    git clone https://github.com/bitdust-io/public.git bitdust


Now you will create a virtual environment with all the required Python dependencies inside it, making sure the BitDust software will run fully isolated.

A single command should do it for you automatically; all required files will be generated in the `~/.bitdust/venv/` folder:

    cd bitdust
    python3 bitdust.py install


The last step to make the BitDust software ready to use is to create a short alias in your OS:

    sudo ln -s -f /home/<user>/.bitdust/bitdust /usr/local/bin/bitdust



## Part 2: start DHT network

### Brief info about the Distributed Hash-Table

[Distributed hash-table](https://en.wikipedia.org/wiki/Distributed_hash_table),
or `DHT`, provides an interface similar to an associative array or dictionary of key/value pairs, where the storage of all (key, value) pairs is distributed among multiple nodes.

The DHT network in BitDust is based on the [Kademlia](https://en.wikipedia.org/wiki/Kademlia) algorithm. Every node has a unique ID, and the "distance" between nodes and keys is calculated with the XOR metric. Each node keeps a routing table of known contacts, and every key/value pair is stored on several nodes whose IDs are closest to the key. A node must know at least one node that is already running (a "seed" node) in order to connect to the network for the first time - after that it discovers other nodes by itself. Moreover, if some of the nodes fail, the network remains stable: values are stored on several nodes at once and are periodically republished, so they stay available.

The BitDust software implements a DHT network which is tightly integrated with other components. The implementation was adopted from the [Entangled project](http://entangled.sourceforge.net/), which uses the UDP protocol as a transport layer.

To get more information about how BitDust uses the Distributed Hash-Table, go to the [BitDust DHT](dht.md) page.


### Genesis seed node

Therefore, the first step to launch the BitDust Distributed Hash-Table network is to launch the very first node, or the "genesis" node, as we call it - all other nodes will use it as an entry point. We need to choose one of the 4 nodes of our core network and reserve it for this purpose.

Let's use the `host-a.com` machine to run the "genesis" DHT node:

    ssh host-a.com


BitDust software consists of many different components that are managed by "network services" organized within the software. By default, all services are configured to best serve the end user of BitDust - the consumer. But since we do not need to run this "role" on our "core" servers (they are intended for servicing and providing resources to end users), we must disable some of the network services for the BitDust software to work correctly:

    bitdust set services/customer/enabled false
    bitdust set services/supplier/enabled false
    bitdust set services/proxy-transport/enabled false
    bitdust set services/proxy-server/enabled false
    bitdust set services/private-messages/enabled false
    bitdust set services/nodes-lookup/enabled false


If you run BitDust software on your home or office computer behind a NAT router, and you do not have a public IP address and a direct Internet connection, you will have to configure port forwarding yourself. This is necessary for all "core" servers because they must be able to receive incoming connections from other nodes in the network. So please make sure UDP port `14441` is open.

Now we will prepare the configuration for the DHT node itself. First, set the UDP port you are going to use to receive incoming packets within the DHT network:

    bitdust set services/entangled-dht/udp-port "14441"


Tell BitDust to use that node as "genesis" DHT seed:

    bitdust set services/entangled-dht/known-nodes "genesis"


Now just start BitDust as a "daemon" process - it will run in the background and operate as a DHT "genesis" node on the `host-a.com` machine:

    bitdust daemon


You can verify the current status of your DHT node by calling the BitDust REST API from the command line with the `curl` tool:

    curl localhost:8180/network/status/v1?dht=1


The output from `curl` should give you `"contacts": 0` which means there are no other nodes connected to `host-a.com` yet.


### Connect seed nodes to each other

You now have a DHT network of 1 single node - not much at all, actually.

Let's connect the other 3 nodes to the first one. The first steps are similar to what we have already done with the "genesis" node:

    ssh host-b.com
    bitdust set services/customer/enabled false
    bitdust set services/supplier/enabled false
    bitdust set services/proxy-transport/enabled false
    bitdust set services/proxy-server/enabled false
    bitdust set services/private-messages/enabled false
    bitdust set services/nodes-lookup/enabled false
    bitdust set services/entangled-dht/udp-port "14441"


Now point the second node to the "genesis" node so that it knows where it should connect for the first time when entering the DHT network:

    bitdust set services/entangled-dht/known-nodes "host-a.com:14441"


Start BitDust on the second node:

    bitdust daemon


Verify the connection status again with `curl localhost:8180/network/status/v1?dht=1` call - you should see `"contacts": 1` in the response on both machines: `host-a.com` and `host-b.com`.

Repeat the above actions on `host-c.com` and `host-d.com` to connect all 4 seed nodes together.



## Part 3: start Identity servers

### Brief info about BitDust identities

Another key component of the BitDust network is the Identity server.

Despite the word "server" in the name, Identity servers do not form a central point of the network: there are many Identity servers owned by different people, every identity file is stored on several of them at once and any user can start their own Identity server. Read more on the [Start new Identity Server](identity_server.md) page.

Each active BitDust user, whether a consumer or a provider of resources, must have an "identity" within the network to be able to communicate with other nodes and be a part of the BitDust community.

Your "identity" is a small digitally-signed XML file that is distributed across several Identity servers on the BitDust network. This XML file contains important information about the device that describes how to connect to the user securely: IP address and port number (by default - of the user's proxy-router, see [User Connections behind NAT](principle.md#user-connections-behind-nat)), public key, nickname, etc.

When you contact another user in the network for the first time, you both share copies of your identities and learn how to connect to each other via the Internet. The BitDust software does everything automatically for you and establishes a secure private peer-to-peer connection between devices.

Therefore, other computers must have the latest copy of your identity in order to be able to communicate with you. For example, when your IP address changes, a new XML file will be automatically generated and "propagated" to the network.

This is the main purpose of the Identity server on the BitDust network - it stores the latest copy of your identity file. A remote user can always access one of the Identity servers, download the XML file, and be able to communicate with you.

The Identity server uses the HTTP protocol to serve the XML files it stores locally. To receive incoming identity files from other nodes it uses the TCP protocol. Basically this is a very simple web server, which works like this:

+ receives incoming TCP data from any remote host
+ validates input data - it must be a valid identity file
+ stores the file locally
+ periodically checks all of the stored files and removes identities that have not been "touched" for a long time

Identity servers do not authenticate users - every node verifies digital signatures by itself, see [How authentication works](identities.md#how-authentication-works).

To get more information about BitDust identities, go to the [BitDust User Identification](identities.md) page.


### Install HTTP Web server

We advise you to use Apache2, Nginx or another web server to handle incoming HTTP traffic and pass it on to the Identity server process running on the same host.

The BitDust software is supposed to always run without root permissions. Because of that, you cannot use port 80 directly in BitDust; an easy workaround is to redirect traffic via a "classical" web server like Apache2 or Nginx.

Let's start with the `host-a.com` machine and install the first Identity server there. Make sure you have set a hostname on your machine, and install Apache2 as an example:

    ssh host-a.com
    sudo hostname -b host-a.com
    sudo apt-get install apache2


Edit the Apache2 main config file:
    
    sudo nano /etc/apache2/apache2.conf 


You need to add a line to set your domain name:
    
    ServerName host-a.com


Create a new `.conf` file for Apache2:

    sudo nano /etc/apache2/conf-available/host_a_com.conf


Use this sample to create the Apache2 config; basically you can just copy & paste it and only change the domain name:

    <VirtualHost *:80>
        ServerName host-a.com
        ServerAlias www.host-a.com
        ProxyPreserveHost on
        ProxyRequests Off
        RewriteEngine on
        ProxyPass / http://localhost:8084/
        ProxyPassReverse / http://localhost:8084/
        Redirect / http://localhost:8084/
        RewriteRule ^/(.*) http://localhost:8084/$1 [P,L]
    </VirtualHost>


Enable this configuration in Apache2 by running this command:

    sudo a2enconf host_a_com


Enable the proxy_http and rewrite modules in Apache2 by running these commands:

    sudo a2enmod proxy_http 
    sudo a2enmod rewrite


Now restart the Apache2 server:

    sudo service apache2 restart


### Configure BitDust Identity server

Now configure BitDust on the `host-a.com` node and set the domain name for this new identity server by running these commands:

    bitdust set services/identity-server/enabled true
    bitdust set services/identity-server/web-port 8084
    bitdust set services/identity-server/tcp-port 6661
    bitdust set services/identity-server/host host-a.com


If you run BitDust software on your home or office computer behind a NAT router, and you do not have a public IP address and a direct Internet connection, you will have to configure port forwarding yourself. This is necessary for all "core" servers because they must be able to receive incoming connections from other nodes in the network. So please make sure TCP ports `6661` and `80` are open.

You should also let the BitDust software know the whole list of Identity servers inside the network:

    bitdust set services/identity-propagate/known-servers host-a.com:80:6661,host-b.com:8080:6661,host-c.com:8080:6661,host-d.com:8080:6661


Now restart BitDust to apply your configuration:

    bitdust restart


Now open your favorite web browser and navigate to `http://host-a.com`. You should see an empty page with the title "Identities on host-a.com".

To make life easier you can also configure your system to start BitDust software automatically when it reboots. For example, on a Debian system you can use the `crontab` tool for that:

    crontab -e
    @reboot /usr/local/bin/bitdust daemon


Repeat the same configuration on the other machines in the "core" network. In this example we will have 4 Identity servers in total:

* www.host-a.com
* www.host-b.com
* www.host-c.com
* www.host-d.com



## Part 4: create first identities

Now it is time to finally create the first "users" inside our small network, which is actually almost ready to be used.

Let's create the first identity on the `host-a.com` machine:

    ssh host-a.com
    curl -X POST -d '{"username": "alice"}' localhost:8180/identity/create/v1
    bitdust restart


Congratulations! You just created your first BitDust user. Let's verify that `alice` is online: if you run that `curl` request again, it should give you much more information than before:

    curl localhost:8180/network/status/v1?dht=1


Also we strongly advise you to always create a backup copy of your identity! Only from that backup file will you be able to restore it in case of loss. Your Private Key will be copied to a text file, and you need to save that file in a very safe place:

    curl -X POST -d '{"destination_path": "/home/<user>/bitdust_identity_backup_key.txt"}' localhost:8180/identity/backup/v1


Now continue and create other identities `bob`, `carl` and `dave` on the other machines. And do not forget to create backup copies of the identities!



## Part 5: start traffic routers

### The purpose of the Proxy servers

As you should have understood from the previous sections, there are different types of nodes within the BitDust network — in other words, "roles". By design, the central role in the [BitDust ecosystem](ecosytem.md) is the end user, the "customer". Other "roles" are designed to support the customer in one way or another and provide the customer with the necessary resources and services.

End users within the BitDust network can be connected with direct peer-to-peer connections. However, in many cases, depending on the type of Internet connection and network configuration, a direct connection between two end users is not physically possible - most of them are behind NAT and have no public IP address. That is why routing via proxy-routers is enabled by default for end users, and direct connections are used only when routing is disabled and the node is able to accept incoming connections.

A possible solution to this problem is to use one or more intermediate nodes, which have a more open network configuration, to redirect traffic between two users located behind NAT.

For this purpose, the role of the "router" was created, implemented as a network service called "proxy-server". Any user, of their own free will, can enable and configure "proxy-server" service inside the BitDust software and help other users connect to each other.

Thus, the router will use the bandwidth of its Internet connection not only for its own needs, but also to help those end users who are currently using it. In other words, the router's owner donates part of their Internet traffic to BitDust users.

At the same time, the Proxy server cannot read the private data that the end user receives or passes through it, or modify the service information - see [Data Security](security.md#data-transfer).


### Configure BitDust Proxy servers

The "proxy-server" service uses the TCP protocol to transfer data from/to the end user. If you run BitDust software on your home or office computer behind a NAT router, and you do not have a public IP address and a direct Internet connection, you will have to configure port forwarding yourself. Please make sure TCP port `7771` is open.

Let's start the "router" service on each node inside the "core" network.

To enable the Proxy server, you first need to disable the "proxy-transport" service - this is actually the other side of BitDust proxy routing. Proxy Transport is a service that the end user uses to connect to a random Proxy server within the BitDust network. Proxy Transport and Proxy server must never run together on the same node, and by default everything in the BitDust software is configured to best serve the end user. That is why by default the "proxy-transport" service is enabled, but the "proxy-server" service is disabled. Let's change that:

    ssh host-a.com
    bitdust set services/proxy-transport/enabled false
    bitdust set services/proxy-server/enabled true


You can verify that the configuration was successfully applied by checking the output of the `bitdust states` command:

    bitdust states | grep proxy


The output should look like this:

    6: service_proxy_server(ON) 
    11: service_proxy_transport(OFF) 
    50: proxy_router(LISTEN) 


It should be noted that it is not necessary to start the router service on machines that form the core of the network. We assume that the "routers" in the network will be numerous and their number will grow as the entire network grows - depending on the needs of users.

The core of the network, by design, should be stable and consist of only a few machines. By design, user data should not pass through these "key machines" - they play their main role only when a new user is initially connected.

Here we provide detailed information on configuring services and roles in a simple network consisting of only four machines. In much larger networks, it will be more efficient to run separate machines for each role and optimize the overall performance.



## Part 6: start suppliers

The most valuable feature that BitDust provides is fully independent and private distributed online storage. A "customer" is someone who consumes space from the BitDust network, and a "supplier" is the one who provides this space.

To get more information about the BitDust storage algorithm, go to the [BitDust Distributed Storage](storage.md) page.

As we already stated in the previous part, for performance reasons it is not necessary to start multiple roles on the same node. Here we have a very simple example of a network, and we only intend to show how to use the BitDust software and run a fresh network from scratch.

If we are talking about a global public network, the "supplier" role, as well as the "proxy server" role, is supposed to be run by many individuals all around the world. The "well known" Identity servers and DHT seed nodes, which new nodes use as an entry point to the network, are supposed to be maintained by a smaller community of skilled developers - but still any user is able to start their own Identity server.

So, let's assume that `alice`, `bob`, `carl` and `dave` are not part of the "core" network we just built, but simply enthusiasts who have just installed the software on their virtual servers and want to donate part of their drives to other users.

On a regular node the "supplier" service is enabled by default. On our "core" nodes we disabled it in Part 2, so to become a supplier you just need to enable and configure the "supplier" service again:

    ssh host-a.com
    bitdust set services/supplier/enabled true
    bitdust set services/supplier/donated-space 50 GB


You can check the current consumption of your donated storage at any time with this `curl` request:

    curl localhost:8180/space/donated/v1


Now `alice` is ready to store data for other users. In the last part you will learn how to actually start using your own BitDust network.



## Part 7: start customers

### Prepare network.json file

...


