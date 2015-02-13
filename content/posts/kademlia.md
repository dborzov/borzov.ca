+++
date = "2015-01-30T08:36:54-07:00"
draft = true
title = "Understanding routing in Bittorrents: algorithms behind Kademlia DHT"

+++

Routing at Bittorrent: understanding Kademlia DHTs (Distributed Hash Tables)
======================


### (unfinished  draft)
DHTs (Distributed Hash Tables) protocols are practical and useful tehcnologies and I am sure tat we are only beginning to untap their potential. Here is a good introduction to the core concepts behind the DHT Networks: [www.freedomlayer.org/articles/dht_intro.html](http://www.freedomlayer.org/articles/dht_intro.html)

That article introduces Chord DHT protocol. In practice, many applications for untrusted networks use Kademlia, including the most succesful one to date: trackerless Bitttorrent file sharing.

Here I am going to use NetVis, an open source network visualizer framework, to visualize in detail how Kademlia protocol operates.

We will review a Kademlia-style DHT implementation from the IPFS source code as an example.
IPFS - Interplanetary File System is an amazing project that combines best and proved approaches behind git and gittorrent and you should [totally learn about it](https://github.com/jbenet/ipfs).

While IPFS is an extensive project and combines many concepts, all we need to understand  for our purposes is that DHT there serves as  a routing purpose in the way similar to Bittorrent file swarm.

That is, DHT network is used to fetch content stored at other nodes.  Content is identified by a unique hash. The DHT stores these content block hashes as keys and addresses of the nodes that have these content blocks as values.

All that is implemented within the [routing/dht](https://godoc.org/github.com/jbenet/go-ipfs/routing) IPFS subpackage.

DHT routing needs implement the following key commands:

- **PutValue**
- **GetValue**
- **Provide**
- **FindPeer**








