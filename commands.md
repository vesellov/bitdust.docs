# BitDust command line

The `bitdust` command is a thin client: most commands connect to the already running BitDust process (the "engine") and call its [API](api.md) methods.
Some commands work only while the engine is running, and some - only while it is stopped; this is noted below.

To create the `bitdust` command in your OS, see [Installation](install.md).

General syntax:

    bitdust [options] [command] [arguments]


* [Start and stop](#start-and-stop)
* [Identity](#identity)
* [Keys](#keys)
* [Settings](#settings)
* [Files](#files)
* [Suppliers and customers](#suppliers-and-customers)
* [Contacts and chat](#contacts-and-chat)
* [Network](#network)
* [Services and state machines](#services-and-state-machines)
* [Remote devices](#remote-devices)
* [Other commands](#other-commands)
* [Options](#options)
* [Examples](#examples)



## Start and stop

#### install
create the Python virtual environment in `~/.bitdust/venv/` and install all required packages

#### install crontab
add a record to your crontab to start BitDust automatically after the machine reboots

#### alias
print a short shell script which runs BitDust, so you can save it as a system-wide command:

    python3 bitdust.py alias > /usr/local/bin/bitdust
    chmod +x /usr/local/bin/bitdust

#### start
start the main BitDust process in the current terminal; the same as running `bitdust` without a command

#### daemon
#### detach
start the main BitDust process in the background

#### restart
restart the main BitDust process, or start it if it is not running yet

#### stop
stop the main BitDust process

#### kill
stop the main BitDust process and kill all its child processes

#### show
start the main BitDust process in the background and open the user interface



## Identity

The commands that create, restore or erase your identity work only while BitDust is stopped.

#### id
print your global ID in the BitDust network, for example `alice@first-machine.com`

#### idurl
print your IDURL - the address of your identity file on the Internet

#### identity
#### identity show
print the content of your identity file

#### identity create &lt;nickname&gt; [private key size]
generate a new private key (the [Master Key](glossary.md#master-key)) and a new identity file - BitDust must be stopped;
the nickname must contain from 3 to 20 characters: lowercase letters, numbers, `-` and `_`, and cannot be changed later;
the key size can be `1024`, `2048` (default) or `4096`

#### identity backup [file path]
save your IDURL and your Master Key into a file, by default into your home folder;
use an absolute path for the file - a relative path is resolved against the BitDust software folder

    ATTENTION! always keep a backup copy of your Master Key in a safe place,
    this is the only possible way to recover access to your data

#### identity restore &lt;file path&gt; [IDURL]
recover your identity from a backup copy of your private key - BitDust must be stopped;
the IDURL is taken from the first line of the file created by `identity backup`, otherwise it must be given as the second argument

#### identity erase
delete the local identity from this machine - BitDust must be stopped

#### identity server
#### identity server start
#### identity server stop
start or stop the identity server; if BitDust is not running, a stand-alone identity server is started in the current terminal,
see [Start new Identity Server](identity_server.md)



## Keys

All commands in this section work only while BitDust is running.
The ID of your Master Key is `master`, the IDs of the [Additional Keys](glossary.md#additional-key) look like `share_0780f0d6...$alice@first-machine.com`, see [File Sharing](sharing.md).

#### key
#### key list
list all known keys: your own keys and keys received from other users

#### key create &lt;key ID&gt; [key size]
generate a new additional private key, the default key size is `4096`

#### key get [key ID]
print the private key to the console, by default the Master Key

    WARNING!!! never publish your Master Key

#### key copy [key ID]
copy the private key to the clipboard, by default the Master Key, use Ctrl+V to paste it

#### key backup &lt;key ID&gt; &lt;file path&gt;
save the private key into a file, use `master` as key ID for your Master Key;
use an absolute path for the file - a relative path is resolved against the BitDust software folder

#### key share &lt;key ID&gt; &lt;IDURL&gt;
send a copy of the given additional key to a trusted user, so they can download and read the data encrypted with that key;
this action cannot be undone, see [Grant access](sharing.md#grant-access)

#### key delete &lt;key ID&gt;
erase the given private key

    WARNING!!! all data encrypted with that key will be lost



## Settings

#### get &lt;option&gt;
print the current value of a program setting

#### set &lt;option&gt; [value]
assign a new value to a program setting, or print the current value if no value is given

#### set list
print all settings and their current values

#### set
#### set help
print a short help about settings

These commands work in both cases: while BitDust is running and while it is stopped.
All available settings are described on the [Settings](settings.md) page. Some of them have short names:

| short name | setting |
|:---|:---|
| `donated` | `services/supplier/donated-space` |
| `needed` | `services/customer/needed-space` |
| `suppliers` | `services/customer/suppliers-number` |
| `debug` | `logs/debug-level` |
| `block-size` | `services/backups/block-size` |
| `max-block-size` | `services/backups/max-block-size` |
| `max-copies` | `services/backups/max-copies` |
| `keep-local-data` | `services/backups/keep-local-copies-enabled` |
| `proxy` | `services/proxy-transport/enabled` |
| `tcp` | `services/tcp-transport/enabled` |
| `tcp-port` | `services/tcp-connections/tcp-port` |
| `udp` | `services/udp-transport/enabled` |
| `udp-port` | `services/udp-datagrams/udp-port` |
| `http` | `services/http-transport/enabled` |
| `http-port` | `services/http-connections/http-port` |
| `dht-port` | `services/entangled-dht/udp-port` |
| `limit-send` | `services/network/send-limit` |
| `limit-receive` | `services/network/receive-limit` |



## Files

All commands in this section work only while BitDust is running.

Files are uploaded into a "virtual" catalog of remote paths. A full remote path has this format:

    {key ID}${nickname}@{ID-server host}:{path}

for example `master$alice@first-machine.com:documents/report.pdf`.
The short form `documents/report.pdf` can also be used - then your own identity and your Master Key are used.
To make the data shareable, use the ID of an [Additional Key](glossary.md#additional-key) in the remote path from the start: the key of already encrypted data cannot be changed later, see [File Sharing](sharing.md).

#### file list [remote path]
show the catalog of files and folders

#### file create &lt;remote path&gt;
create a new file item in the catalog - this must be done before uploading

#### dir create &lt;remote path&gt;
create a new folder in the catalog

#### file upload &lt;local path&gt; &lt;remote path&gt;
start uploading a local file or folder to your suppliers into the given catalog item

#### file download &lt;remote path&gt; [local path]
download a file or folder from the suppliers; if the local path is not given, the data is saved into the current folder

#### file progress [upload | download]
show currently running uploads (by default) or downloads

#### file cancel [upload | download] &lt;remote path&gt;
abort a currently running upload (by default) or download

#### file delete &lt;remote path&gt;
#### dir delete &lt;remote path&gt;
remove the item (a folder - with all sub-folders) from the catalog and from your suppliers

#### file sync
request the actual list of files from your suppliers and restart unfinished uploads



## Suppliers and customers

All commands in this section work only while BitDust is running.

#### supplier list
show the list of your suppliers - nodes that store your data on their machines

#### supplier ping
send your identity to all your suppliers and wait for their response to check their statuses

#### supplier replace &lt;position&gt;
fire the supplier at the given position and hire another random node instead

#### supplier change &lt;position&gt; &lt;new IDURL&gt;
replace the supplier at the given position with the given node

#### customer list
show the list of your customers - nodes that store their data on your machine

#### customer ping
send your identity to all your customers and wait for their response to check their statuses

#### customer remove &lt;IDURL&gt;
stop supporting the given customer and remove all local files stored for them

#### storage
show statistics of donated, consumed and local storage space



## Contacts and chat

All commands in this section work only while BitDust is running.

#### friend
show the list of your contacts

#### friend check &lt;nickname&gt;
look up a user by nickname in the network

#### friend add &lt;nickname or IDURL&gt;
add a user to your contacts

#### chat
start an interactive chat session in the terminal: send and receive text messages

#### chat send &lt;IDURL&gt; "&lt;text message&gt;"
send a single text message to a user



## Network

#### reconnect
restart all network connections - BitDust must be running

#### ping &lt;IDURL&gt;
send your identity to the given node and wait for its response to check its status - BitDust must be running

#### network create &lt;URL&gt;
load a custom network configuration (like [default_network.json](https://github.com/bitdust-io/public/blob/master/default_network.json)) from the given URL, to join a private BitDust network, see [Your own BitDust Network from scratch](build_network.md)

#### network select &lt;name&gt;
switch to another saved network configuration

#### dhtseed [daemon]
start a stand-alone Distributed Hash Table seed node, in the current terminal or in the background - BitDust must be stopped, see [Distributed Hash-Table](dht.md#support-bitdust-network)



## Services and state machines

All commands in this section work only while BitDust is running.

#### service list
list all [network services](services.md) and their states

#### service tree
show the dependency tree of the network services

#### service &lt;service name&gt;
print detailed info about the given service, for example `service service_proxy_transport`

#### service start &lt;service name&gt;
#### service stop &lt;service name&gt;
start or stop the given service without restarting the program

#### states
#### automats list
list all running [state machines](automats.md) and their current states



## Remote devices

These commands configure secure access to your BitDust node from a remote device, for example from the mobile application. They work only while BitDust is running.

#### device list
list all remote device configurations

#### device info &lt;name&gt;
print the details of the given device configuration

#### device add direct &lt;name&gt; [host] [port] [key size]
add a configuration for a direct connection from the remote device, by default to `localhost` port `8281`

#### device add routed &lt;name&gt; [key size]
add a configuration for a routed connection: the encrypted traffic between your node and the remote device goes via intermediate nodes, this is useful when your network is restricted

#### device authorize &lt;name&gt; [client code] [client public key]
start the authorization of a remote device

#### device access-key &lt;name&gt; [file path]
generate an access key for the remote device and print it or save it into a file

#### device enable &lt;name&gt;
#### device disable &lt;name&gt;
allow or block connections from the given device

#### device reset &lt;name&gt;
revoke the authorization of the given device

#### device remove &lt;name&gt;
remove the device configuration



## Other commands

#### api
list all [API](api.md) methods with their parameters

#### api &lt;method&gt; [name=value ...]
call an API method and print the JSON response - BitDust must be running

#### version
display the current software version

#### help
print the full help on command line usage

#### help schedule
print the format of the schedule settings

#### help settings
print all settings and their values



## Options

-h, --help
show help and exit

-d DEBUG, --debug=DEBUG
set debug level

-q, --quite
quiet mode, do not print any messages to stdout

-v, --verbose
verbose mode, print more messages

-n, --no-logs
do not use logs

-o OUTPUT, --output=OUTPUT
print log messages to a file

-a APPDIR, --appdir=APPDIR
set an alternative location for application data files, default is `~/.bitdust/`

--twisted
show twisted log messages too

--cpu-profile
use cProfile to profile performance, output is in the file `/tmp/bitdust.profile`

--coverage, --coverage_config, --coverage_report
record code coverage (for developers)



## Examples

#### Create an identity and back up the Master Key

    bitdust identity create alice
    bitdust identity backup /home/alice/alice.bitdust.key


#### Some basic configuration

    bitdust set needed 100MB
    bitdust set donated 2GB
    bitdust set suppliers 7


#### Start/Stop software

    bitdust daemon
    bitdust stop
    bitdust restart


#### Upload a file

    bitdust file create documents/report.pdf
    bitdust file upload /home/alice/report.pdf documents/report.pdf
    bitdust file progress


#### List uploaded items

    bitdust file list


#### Download from your suppliers

    bitdust file download documents/report.pdf /tmp/report.pdf
    bitdust file progress download


#### Remove remote data

    bitdust file delete documents/report.pdf


#### Ping all suppliers to check their current status

    bitdust supplier ping


#### Ping a given node by IDURL

    bitdust ping http://first-machine.com/bob.xml


#### Get info about donated and consumed space

    bitdust storage


#### Modify a single setting

    bitdust set logs/debug-level 8
