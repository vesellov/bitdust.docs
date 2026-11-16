# Install BitDust software


* [About](#about)
* [Install software dependencies](#install-software-dependencies)
* [Get BitDust sources to your local machine](#get-bitdust-sources-to-your-local-machine)
* [Building virtual environment](#building-virtual-environment)
* [Run BitDust](#run-bitdust)
* [Binary Dependencies](#binary-dependencies)



## About

#### Decentralized encrypted storage platform

The lack of secure storage is a problem for Internet users. The BitDust decentralized encrypted storage platform solves this problem by giving users the ability to store their data fully encrypted on a decentralized network via an easy-to-use application, and to share data only with trusted contacts.

All your data is encrypted before it leaves your computer with a private key that your computer generates. Only you can read data encrypted with your [Master Key](glossary.md#master-key), which never leaves your device. If you want to share some data, encrypt it from the start with a separate [Additional Key](glossary.md#additional-key) and give a copy of that key to a trusted contact - the key of already encrypted data cannot be changed later, see [Sharing](sharing.md). Nobody else can read your data, not even the suppliers or the BitDust team!

BitDust is written in Python using the pure Twisted framework and published under GNU AGPLv3.

There is an early alpha version available for Windows, Mac and Linux. Any feedback regarding this alpha version is much appreciated. Please send your feedback to info@bitdust.io.


## Install software dependencies

Under Ubuntu/Debian, and probably most other distros as well, you can install all dependencies with a single command:

        sudo apt-get install git gcc python3-dev python3-virtualenv


Optionally, you can also install the [miniupnpc](http://miniupnp.tuxfamily.org/) tool if you want BitDust to automatically set up the UPnP configuration of your network router, so it can also accept incoming connections from other nodes. By default incoming traffic reaches your node via a proxy-router, so this is needed only if you disable routing - see [User Connections behind NAT](principle.md#user-connections-behind-nat):

        sudo apt-get install miniupnpc


On macOS you can install the requirements with this command:

        brew install git python3


Then you need to use the pip package manager to get all the required packages:

        pip install --upgrade --user
        pip install --upgrade pip --user
        pip install virtualenv --user


On a Raspberry Pi you will need to install these packages:

        sudo apt-get install git gcc python3-dev python3-virtualenv python3-pip libffi-dev libssl-dev



## Get BitDust sources to your local machine

The second step is to get the BitDust sources. To have full control over the BitDust process running on your local machine, it is better to make a fork of the public BitDust repository on GitHub at https://github.com/bitdust-io/public and clone it to your local machine:

        git clone https://github.com/<your GitHub username>/<name of BitDust fork>.git bitdust


If the sources are located in the `~/.bitdust/src/` folder (this is where the [BitDust Desktop](software_releases.md) application clones them), the software automatically updates itself: some time after start it runs `git fetch` and `git reset --hard origin/master` in that folder. Sources cloned to any other location are not updated automatically - you need to run `git pull` yourself. This way we make sure that everyone is running the latest version of the program. Once you have made a fork, you will have to update your fork manually and pull commits from the public BitDust repository if you trust them.

However, if you just trust the BitDust contributors, you can simply clone the public repository directly and the software will be up to date with the "official" public code base:

        git clone https://github.com/bitdust-io/public.git bitdust



## Building virtual environment

Afterwards you will need to build a virtual environment with all the required Python dependencies, making sure the BitDust software will run fully isolated.

A single command should do it for you; all required files will be generated in the `~/.bitdust/venv/` folder:

        cd bitdust
        python3 bitdust.py install


The last step to make the BitDust software ready to use is to create a short alias in your OS; then you can just type `bitdust <some command>` in the command line to get fast access to the program:
        
        sudo ln -s -f /home/<user>/.bitdust/bitdust /usr/local/bin/bitdust  # location depends on your system



## Run BitDust

Start using the software by creating an identity for your device in the BitDust network:
       
        bitdust id create <some nick name>
       

We recommend creating another copy of your Private Key in a safe place to be able to recover your identity and access to your data in the future. You can do it with this command (the BitDust software must be running):

        bitdust key backup master ~/<nickname>.bitdust.key


In the future, in case you need to restore access to your data, you will need to install BitDust again and recover your identity:

        bitdust identity restore ~/<nickname>.bitdust.key


Your settings and local files are located in this folder: `~/.bitdust/`

Type this command to read more info about BitDust commands:

        bitdust help


To run the software type:

        bitdust
        

Start as a background process:

        bitdust detach


To get more insights or just to learn how to start playing with the software,
visit the [BitDust Commands](commands.md) page.

To get more info about all API methods served by the BitDust engine process, go to the [BitDust API](api.md) page.



## Binary Dependencies

If you are installing BitDust on Windows, you may need some binary packages already compiled and packaged for Microsoft Windows; you can check the following locations and download the needed binaries and libraries:

* cygwin: [cygwin.com](https://cygwin.com/install.html)
* git: [git-scm.com](https://git-scm.com/download/win)
* python3: [python.org](http://python.org/download/releases)
* twisted: [twistedmatrix.com](http://twistedmatrix.com)
* pyasn1: [pyasn1.sourceforge.net](http://pyasn1.sourceforge.net)
* miniupnpc: [miniupnp.tuxfamily.org](http://miniupnp.tuxfamily.org/)

