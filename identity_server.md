# How to start a new Identity Server in the BitDust network


## Intro

You need to create your own new identity to be able to communicate with other nodes and access the BitDust community.
Identity files are distributed across identity servers in the network: other nodes hold and expose copies of your identity.

A third node can access that ID server and download your identity to be able to talk to you.
So any node in the network can start its own ID server and help to maintain the network and make it more reliable.

Your global IDURL is formed from your nickname and the DNS name (or IP address) of the first ID server.

When one of your Identity servers is down, the BitDust software will automatically find a fresh Identity server for you, "propagate" your identity there and remove the dead Identity server from the list of your sources. That process is called "identity rotate" and it is fully automated and decentralized.

Because of that, the distribution of identity files in the BitDust network is actually pretty reliable and fully independent of any single party.

Note that ID servers do not authenticate users - they only store and publish copies of identity files, which are digitally signed by their owners and cannot be forged. Every node verifies digital signatures by itself, see [How authentication works](identities.md#how-authentication-works).


## Install and configure a web-server

To protect your BitDust node from outside we can use a web-server and re-route traffic internally to the `bitdust` process.

Here are two example configurations for well-known servers: `Apache2` and `Nginx`.
But first, you need to set up a hostname on your machine if you have not done it before:

        sudo hostname -b my-own-identity-server.com


#### Nginx setup

Install Nginx with Apt:

        sudo apt-get update
        sudo apt-get install nginx


Disable the default virtual host, which is pre-configured when Nginx is installed via the Ubuntu package manager apt:

        sudo unlink /etc/nginx/sites-enabled/default


Enter the /etc/nginx/sites-available directory and create a configuration file:

        cd /etc/nginx/sites-available
        sudo nano my-own-identity-server_com.conf


You can use the sample config below:

        server {
            server_name my-own-identity-server.com www.my-own-identity-server.com;
            listen 80;
            listen [::]:80;
            access_log /var/log/nginx/idserver-log.log;
            error_log /var/log/nginx/idserver-error.log;
            location / {
                proxy_pass http://127.0.0.1:8084/;
            }
        }


Link the configuration from /etc/nginx/sites-available to /etc/nginx/sites-enabled. It is recommended to use a symbolic link.

        ln -s /etc/nginx/sites-available/my-own-identity-server_com.conf /etc/nginx/sites-enabled/my-own-identity-server_com.conf


Enable Nginx system service and start the web-server:

        sudo systemctl enable nginx
        sudo systemctl start nginx
        sudo service nginx reload


#### Apache2 setup

Install Apache2 server if you do not have it yet on your machine:

        sudo apt-get update
        sudo apt-get install apache2


Edit the Apache2 main config file:
    
        sudo nano /etc/apache2/apache2.conf 


You need to add a line to set your domain name:
    
        ServerName my-own-identity-server.com


Create a new `.conf` file for Apache2:

        sudo nano /etc/apache2/conf-available/my-own-identity-server_com.conf


Use this template to create the Apache2 config; basically you can just copy & paste it and only change the domain name:

        <VirtualHost *:80>
          ServerName my-own-identity-server.com
          ServerAlias www.my-own-identity-server.com
          ProxyPreserveHost on
          ProxyRequests Off
          RewriteEngine on
          ProxyPass / http://localhost:8084/
          ProxyPassReverse / http://localhost:8084/
          Redirect / http://localhost:8084/
          RewriteRule ^/(.*) http://localhost:8084/$1 [P,L]
        </VirtualHost>


Enable this configuration in Apache2 by running this command:

        sudo a2enconf my-own-identity-server_com


Enable the proxy_http and rewrite modules in Apache2 by running these commands:

        sudo a2enmod proxy_http 
        sudo a2enmod rewrite


Now restart the Apache2 server:

        sudo service apache2 restart



## Configure BitDust software

First you need to [install BitDust](install.md) on your machine. 

Now configure BitDust on your node to set the domain name for this new identity server by running these commands:

        bitdust set services/identity-server/host my-own-identity-server.com
        bitdust set services/identity-server/enabled true


You can manually set a port number for incoming connections using this command:

        bitdust set services/identity-server/web-port 8084



## Run standalone Identity Server

Run this command to start the Identity server process in the current terminal:

        bitdust identity server start


Now open your favorite web browser and navigate to [my-own-identity-server.com](http://my-own-identity-server.com). You should see an empty page with the title "Identities on my-own-identity-server.com".

Congratulations! You are now running your own identity server and are able to register your own identity there or help other people by hosting their identity files on your machine.


## Run Identity Server as daemon process

Make sure the identity_server network service is enabled in the settings (check the configuration above) and start the BitDust software in "daemon" mode:

        bitdust daemon


Make sure the ID server is up and running:

        bitdust states | grep id_server
        36: id_server(LISTEN)


Open your browser and go to [my-own-identity-server.com](http://my-own-identity-server.com) to check the server status.

To make life easier you can also configure BitDust to start automatically when your machine reboots. For example, on a Debian system you can use the `crontab` tool for that:

        crontab -e
        @reboot /usr/local/bin/bitdust daemon


## Create dedicated identity

Normally, when you create a new identity, the software does everything for you automatically and just selects a few random ID servers, ping them and "propagate" your new identity to those nodes.

But you can decide which ID servers you prefer, if you would like to control exactly where your identity will be distributed, and modify your "known" ID servers:

    bitdust set services/identity-propagate/known-servers first-server.com:80:6661,second-host.net:8080:6661


Now if you create a new identity with such commands:

    bitdust id erase
    bitdust id create my_dedicate_identity


it will be automatically "propagated" to `first-server.com` and `second-host.net` and your global IDURL should look like this: `"http://first-server.com/my_dedicate_identity.xml"`



## Support BitDust network

If you plan to maintain your ID server for a while and support the network, it makes sense to include it in the list of "well known" identity servers, which is stored in the [default_network.json](https://github.com/bitdust-io/public/blob/master/default_network.json) file in the root of the BitDust public repository.

You can fork the [Public Git Repository](https://github.com/bitdust-io/public), modify the `default_network.json` file in your forked repository and open a [Pull Request](https://github.com/bitdust-io/public/pulls) with your changes. Contact the BitDust contributors to let them know that you have started a new ID server, and one of the developers will approve your Pull Request.

