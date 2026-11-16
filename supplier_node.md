# Running BitDust supplier node


A supplier is a node whose owner donates part of the storage of their own PC and shares resources with other users in the network.
The supplier role is enabled by default on every node - the steps below show how to run a node that only donates storage and does not consume it.

At the moment suppliers are not rewarded for the contribution they make to the network. Storage contracts and payments with BitDust coins already exist in the software, but at the moment this does not affect data storage yet, and the function can be disabled in the program settings, see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts).

Read more about [BitDust installation](install.md) or just follow the few steps below to install the software on your machine.


First, clone the BitDust sources locally:

        git clone https://github.com/bitdust-io/public.git bitdust


Create the BitDust virtual Python environment:
        
        cd bitdust
        python3 bitdust.py install


The last step to make the BitDust software ready to use is to create a short alias in your OS; then you can just type `bitdust <some command>` in the command line to get fast access to the program:
        
        sudo ln -s -f /home/<user>/.bitdust/bitdust /usr/local/bin/bitdust  # location depends on your system


Now you need to configure a few software settings to actually donate storage space to the network.

Disable the services you do not need if you run the supplier role. Switch off the "customer" service so your machine will not consume any storage, but only donate it:

        bitdust set services/customer/enabled false


Declare how much space you are willing to donate to the BitDust network:

        bitdust set services/supplier/donated-space 20GB


If you have not yet created an identity for your device in the BitDust network, you can do it with this command:

        bitdust id create <some nick name>


When you do it for the first time, we recommend creating another copy of your Private Key in a safe place to be able to recover your identity in BitDust in the future. You can make a backup copy with this command (the BitDust software must be running):

        bitdust key backup master ~/<nickname>.bitdust.key


Finally, start the BitDust software in daemon mode so it keeps running in the background and you can keep using your PC:

        bitdust daemon


You can always check the current state of your "donated storage" with this command:

        bitdust storage

