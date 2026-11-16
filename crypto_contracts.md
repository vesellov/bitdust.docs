# BitDust Crypto Contracts

(in progress)

* [Business requirements](#business-requirements)



## Business requirements

BitDust already has its own blockchain with BitDust "coins" and storage contracts, but at the moment they do not affect data storage yet and can be disabled in the settings, see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts). The idea is to count hardware resources, not euros, dollars or bitcoins. However, to be able to buy/sell PC resources, it is of course necessary to implement some "coins". There are several important points that need to be considered here:

+ Because of the distributed design of the whole BitDust ecosystem, all billing and payments must be fully decentralized/distributed as well. That means there should not be a single point of control - nobody should be able to control and manage all billing operations in the network and collect/centralize customer information.

+ Imagine a situation where a customer located in country A uses storage provided by a supplier living in country B. The customer is able to pay in the local currency of country A, but the supplier would like to receive money in another currency, because they live in country B, where currency A is not available. So the method must be independent and generic.

+ Transfer/storage costs for each and every small transaction must be very low, and the number of regular transactions must be reduced as much as possible. For example, if a customer has 64 suppliers, paying all of them one by one is not efficient at all - they will prefer to pay only once for all finished contracts, or even a fixed price.

+ It must be possible to use BitDust network without spending any real cash - if you donate more resources than you consume all services must be free for you.

+ It must be possible to reward developers.



Based on these requirements we started designing and developing a distributed online payment system for the BitDust project. The goal is to allow users to earn some "coins" and benefit from providing the resources of their own PCs to other people. The first version of this payment system is already included in the software, but it does not affect data storage yet.

