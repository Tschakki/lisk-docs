# Lisk Peer-to-Peer Communication

Peer-to-Peer communication serves a vital function within the Lisk network. The peering mechanisms provide the required architecture to facilitate network consensus, block propagation and transaction propagation.

## Architecture

The peers in the Lisk network use JSON objects with compressed [**blocks**](blocks.md) and [**transactions**](transactions.md).
The Lisk logic running on every node in the Lisk network uses remote procedure calls (RPCs) and events to communicate the transaction and block JSON objects to the other peers.
The RPCs and events are also transmitted as JSON objects with additional fields telling the Lisk application which method to use in order to process the transmitted object.
In order to effectively transmit these JSON objects to the other peers, websockets are used via the [SocketCluster Framework](https://socketcluster.io).
An overview of the architecture of the peer-to-peer communication in Lisk is provided below.

![lisk_protocol-p2parchitecture](assets/lisk_protocol-p2parchitecture.png "lisk_protocol-p2parchitecture")

## System Headers
Every time a Lisk node communicates with a peer of the Lisk network, a system header is added to the message. The system headers are used to identify full nodes and provide basic information about the software running on the system.

The following JSON object is generated from system data for this purpose:

```json
{
  "os":"darwin16.3.0",
  "version":"0.6.0a",
  "port":7000,
  "height":1574654,
  "nethash":"da3ed6a45429278bac2666961289ca17ad86595d33b31037615d4b8e8f158bba",
  "broadhash":"c7e0902a7016205d456a427edda2b09f4b875f98ef40a224018a0274347146ac",
  "minVersion":">=0.5.0"
}
```

## Block Propagation

Block propagation serves a vital function on the Lisk network. Without block propagation, the system would grind to a halt and the blockchain would cease to function.
Blocks are made in a decentralized fashion and must be sent to all nodes on the network in order to establish consensus. When a block is generated, it is broadcast to 25 randomly selected peers.
These forward the validated block to 25 randomly selected peers and so on. In order to prevent overbroadcasting of data, every block is given a relay limit of 3 and blocks that have already been received are not broadcast again.


## Transaction Propagation

Transactions must move from one node to all other nodes in order to be included in blocks.
The broadcast queue for transactions works by drawing up to 25 transactions from the transactions pool and performing a validation process on those transactions.
These transactions are then broadcast to other nodes in a bundled JSON object. This can be represented as an array of objects, depending on the [**transaction type**](transactions.md).
The bundle is then broadcast to the network at regular intervals, currently specified as every 5 seconds.
The time delay allows the bundle to accumulate additional transactions from the network (up to 25). In addition to broadcasting the object, the bundle is given a relay limit to prevent spamming the network.
In the current implementation the relay limit is set as 3, which means that every bundle will be broadcast for **at most 3 hops** by the peers on the network.

## Transaction Pool

The transaction pool provides the Lisk network a robust solution for preserving unconfirmed transactions that have overflowed into the next block.
As described in [**blocks**](blocks.md), each block can only include 25 transactions and the transaction pool allows up to 1.000 multisignature transactions and other 1.000 for the remaining transaction types to remain queued for the next block(s).
The transaction pool could be thought of as a memory pool, keeping transactions ready until they are signed into a block.
The second usage of the transaction pool is to provide a mechanism for propagating transactions.
When a node prepares a transaction bundle, it draws up to 25 transactions from the pool and broadcast them to the network.
In order to keep the transaction pool tidy, all transactions are given a time to live.
This time to live is defined as 10800 seconds, or 1080 blocks. The final use for the transaction pool is to house transactions with pending signatures.
Like unconfirmed transactions, these transactions will expire out of the pool based on the lifetime specified when the transaction is first received.
