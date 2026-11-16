# BitDust Roles and Coins

#### Draft 2

__NOTE__: This is an early draft proposal. The current implementation is different: the BitDust blockchain, wallet and storage contracts already exist in the software (but do not affect data storage yet), see [Blockchain and storage contracts](storage.md#blockchain-and-storage-contracts); the Rebuilder role is not implemented yet.
Only the roles involved in payments are listed here, see [Ecosystem](ecosytem.md) for the full list of roles.
An alternative approach is described in the [Merchants](merchants.md) draft proposal.



## Roles


### Customer

+ Consume resources from other nodes: storage, network traffic, CPU time

+ Pay the Miner with BTC or ETH to "buy a service"

+ Receive an "empty" BitDust coin from the Miner

+ Send the BitDust coin to the Supplier to "pay for service"



### Supplier

+ Donate part of their own HDD/SSD drive to the Customer so that they can store something

+ Receive a "signed" BitDust coin from the Customer

+ Send the "confirmed" BitDust coin to the Miner to "sell storage service"

+ Receive BTC or ETH from the Miner



### Router (Proxy Server)

+ Help other nodes to connect to each other and send/receive packets: re-route network traffic

+ Help nodes behind NAT to receive incoming connections

+ Receive a "signed" BitDust coin from the Customer

+ Send the "confirmed" BitDust coin to the Miner to "sell network bandwidth service"

+ Receive BTC or ETH from the Miner



### Rebuilder

+ Help a new Supplier to reconstruct "missing" data fragments and store Customer data

+ Receive a "signed" BitDust coin from the Supplier

+ Send the "confirmed" BitDust coin to the Miner to "sell CPU service"

+ Receive BTC or ETH from the Miner



### Miner

+ Help other nodes to get read access to the blockchain

+ Publish transactions on the blockchain for other nodes

+ Receive BTC or ETH from the Customer

+ Generate an "empty" BitDust coin and give it to the Customer

+ Receive the "confirmed" BitDust coin from the Supplier, Rebuilder or Router

+ Send BTC or ETH to the Supplier, Rebuilder or Router

+ Finally, "burn" the BitDust coin as a "proof of storage"



## Rules

1. Every Customer can make only one transaction per Miner. If they require more coins, they will have to find another Miner in the network

2. Every Supplier, Rebuilder or Router can make only one transaction per Miner. If they want to sell more "confirmed" coins to "burn" them, they will have to find another Miner in the network

3. A BitDust coin cannot be transferred after it has been "burned"

4. A fixed maximum life-cycle duration for each BitDust coin will be decided later (3-12 months) - the whole BitDust blockchain will be periodically cleaned up and mining will start from scratch all over again in the next cycle
