+++
date = "2015-01-30T08:36:54-07:00"
draft = true
title = "Understanding routing in Bittorrents: algorithms behind Kademlia DHT"

+++

Understanding routing in Bittorrents: algorithms behind Kademlia DHT
======================

DHTs (Distributed Hash Tables) are important and useful. Here is a good introduction to the core concepts behind the DHT protocols: [www.freedomlayer.org/articles/dht_intro.html](http://www.freedomlayer.org/articles/dht_intro.html)

That article introduces Chord DHT as it is somewhat simpler to understand. In practice, many applications for untrusted networks use Kademlia, including the most succesful one to date: trackerless Bitttorrent file sharing.

Here I am going to use NetVis, an open source network visualizer framework, to review in detail how Kademlia protocol operates.

Kademlia needs to implement the following key commands:

- **NEW NODE ENTERS NETWORK**
- **FIND VALUE**






