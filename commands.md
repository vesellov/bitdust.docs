# BitDust command line 

## Commands

### start
start main BitDust process


### detach
start BitDust as a daemon process


### restart
restart BitDust 


### stop                
stop BitDust


### show                
start BitDust and show the main window


### integrate           
creates a `bitdust` alias in OS in `/usr/local/bin/bitdust` or in `~/bin/bitdust` if you do not have access to `/usr/local/`


### identity create [nickname] [private key size]
generates a new private key and a new identity file for you, key size can be `1024`, `2048` or `4096`


### identity restore [private key source file] [IDURL]
recover an existing identity file using a backup copy of your private key


### identity erase
delete the local identity from the machine


### key copy            
copy the private key to the clipboard


### key backup [key ID] [a filename for copy of private key]
copy the private key into a file, use `master` as key ID for your Master Key


### key print
print the private key to the console
WARNING!!! never publish your key


### get [option]
print the current value of a given program setting


### set [option] [value]
use this command to set a program setting


### set list            
print all available settings and their values


### file list
show a full catalog of registered
files and folders


### file idlist
show a list of items already uploaded
on remote peers 


### file bind [local path]
add a given path as a top-level item in the catalog


### file add [local path]
replicate a given path into the catalog,
this will add all parent folders to the catalog too


### file addtree [local folder path]
replicate a given folder with
all sub-folders to the catalog,
be aware that if you add too many items to the catalog
the software may operate inefficiently    


### file start [local path or ID]
start uploading a catalog item to remote peers,
bind a new local file/folder if the path does not yet
exist in the catalog


### file delete [local path, ID or full backup ID]
remove a file or folder (with all sub-folders)
from the catalog or only erase a given remote copy


### file delete local [full backup ID]
remove only the local copy of a given backup,
keep the remote copy on the suppliers' HDDs


### file queue        
show a list of paths to be uploaded


### file progress
show a list of currently running uploads


### file sync       
request all suppliers to check/restart uploads


### restore list
show a list of items already uploaded on remote peers


### restore start [local path or ID] [destination path]
download personal data back to the local machine
from remote peers, you can specify
the destination path on your local drive,
WARNING! the source path is the default location,
so it will overwrite existing files by default


### restore abort [backup ID]
abort the currently running restore process of a given item


### restore progress 
show currently running downloads


### supplier list       
show a list of your suppliers -
nodes that store your data on their machines


### supplier replace [IDURL or position]
execute a fire/hire process for a given supplier,
another random node will replace it


### supplier change [IDURL or position] [new IDURL]
replace a supplier with a given node


### supplier ping       
send an Identity packet to all suppliers
and wait for Ack packets to check their statuses


### customer list       
show a list of your customers -
nodes that store their own data on your machine


### customer remove [IDURL]
stop supporting a given customer and
remove all local files stored for them


### customer ping       
send an Identity packet to all customers
and wait for Ack packets to check their statuses


### storage
show donated/needed storage statistics


### automat list
list all running state machines and current states


### service list
list all registered services


### service [service name]
print detailed info for a given service


### ping [IDURL]        
send an Identity packet to this node
and wait for an Ack packet to check its status


### chat
start a chat session, send/listen text
messages from other users


### chat send [IDURL] ["text message"]
send a single text message to a remote user


### api [method] [params]
execute API method and return JSON response


### version             
display the current software version


### help                
print detailed info on command line usage


### usage               
print a brief list of available commands


## Options:

-h, --help        
show this help message and exit

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
set an alternative location
for application data files,
default is `~/.bitdust/`

--twisted       
show twisted log messages too




## Examples

#### Create an identity

    bitdust id create alice


#### Backup the Private Key

    bitdust key backup master ~/bitdust-alice-private.key
    

#### Some basic configuration

    bitdust set needed 100Mb
    bitdust set donated 250Mb
    bitdust set suppliers 7


#### Start/Stop software

    bitdust
    bitdust detach
    bitdust stop
    bitdust restart


#### Upload files/folders

    bitdust backup start ~/examples.desktop


#### List of uploaded items

    bitdust backups
    bitdust backup list
    bitdust backup idlist


#### Download from remote peers

    rm ~/examples.desktop
    bitdust restore 0/0/0
    ls ~/examples.desktop

    bitdust restore /home/veselin/examples.desktop /tmp
    ls /tmp/examples.desktop


#### Remove remote data
    
    bitdust backup delete /home/veselin/examples.desktop


#### Request the actual info from remote nodes 
 
    bitdust backups updated


#### Ping all suppliers to check their current status

    bitdust suppliers ping
    
#### Ping a given node by IDURL

    bitdust ping http://p2p-id.ru/veselin.xml

#### Get some info about donated and consumed space usage

    bitdust storage

#### Print a list of program settings

    bitdust set list
    
#### Modify a single setting

    bitdust set logs/debug-level 8

