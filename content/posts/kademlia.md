+++
date = "2015-01-30T08:36:54-07:00"
draft = true
title = "Understanding routing in Bittorrents: algorithms behind Kademlia DHT"

+++

Routing at Bittorrent: understanding Kademlia DHTs (Distributed Hash Tables)
======================


### (unfinished  draft)

That article is currently work in progress and will probably take a while to finish.

DHTs (Distributed Hash Tables) protocols are beautiful and practical and I am convinced that we as a species are only beginning to untap their true potential. Here is a good introduction to the core concepts behind the DHT Networks: [www.freedomlayer.org/articles/dht_intro.html](http://www.freedomlayer.org/articles/dht_intro.html)

That intro introduces Chord DHT protocol as a somewhat  simpler case. In practice, many applications for untrusted networks use Kademlia, including the most succesful one to date: trackerless Bitttorrent file sharing.

Here I am going to use NetVis, an open source network visualizer framework, to visualize in detail how Kademlia protocol operates.

We will review and annotate a Kademlia-style DHT implementation from the IPFS project source code as an example.



IPFS - the Interplanetary File System is an amazing project that combines best and proved approaches behind git and gittorrent and you should [totally learn about it](https://github.com/jbenet/ipfs).

While IPFS is an extensive project and combines many concepts, all we need to understand  for our purposes is that DHT there serves as  a routing purpose in the way similar to Bittorrent file swarm.

That is, DHT network is used to fetch content stored at other nodes.  Content is identified by a unique hash. The DHT stores these content block hashes as keys and addresses of the nodes that have these content blocks as values.

All that is implemented within the [routing/dht](https://godoc.org/github.com/jbenet/go-ipfs/routing) IPFS subpackage.

DHT routing implement the general routing interface defined at [`routing/routing.go`](https://github.com/jbenet/go-ipfs/blob/9dd12922b341d891a2365beb10d0142fd10fb235/routing/routing.go):
```go
	type IpfsRouting interface {
	    FindProvidersAsync(context.Context, u.Key, int) <-chan peer.PeerInfo

	    // PutValue adds value corresponding to given Key.
	    PutValue(context.Context, u.Key, []byte) error

	    // GetValue searches for the value corresponding to given Key.
	    GetValue(context.Context, u.Key) ([]byte, error)

	    // Announce that this node can provide value for given key
	    Provide(context.Context, u.Key) error

	    // Find specific Peer
	    // FindPeer searches for a peer with given ID, returns a peer.PeerInfo
	    // with relevant addresses.
	    FindPeer(context.Context, peer.ID) (peer.PeerInfo, error)

	    // Ping a peer, log the time it took
	    Ping(context.Context, peer.ID) (time.Duration, error)

	    // Bootstrap allows callers to hint to the routing system to get into a
	    // Boostrapped state
	    Bootstrap(context.Context) error
	}
```
That corresponds to the folowing key operations:

- **PutValue** - we put value into the hash table with `u.Key` as the key, and arbirtary `[]byte` as a value 
- **GetValue** - we resolve the key from the hash table
- **Provide**  - A node anounces to DHT that it can serve a specific key
- **FindPeer** - - we search for a peer node within the DHT network by its ID ( IPFS is a complicated project with a lot of concepts and they try to stick to a  consistent nomenclature. As they have plenty of graphs wit nodes, they call the DHT network nodes peers everywhere accross the project).








