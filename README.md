# Melograph
A self configuring, multiple collection, peer-to-peer personal music streaming solution over bittorrent

## How it works

### Procol Overview
![Flow graph](https://i.imgur.com/FSsGDUK.png)

### Clients
A Melograph client (node) is in charge in indexing and performing tag extraction on all music files in configuration-specified local directories.
It responds to queries from other clients with the same shared key, notifies other clients of changes to the local database, and is capable of
requesting a song from another connected node. Other nodes are found using bittorrent trackers and the DHT by looking for any peers in the table
using a unique shared `infohash` (channel name) listed in the Melograph client configuration.

### Global Music Index
For nodes that want to search and play music from the distributed collection, a global music index is created from each connected client's local
database. When a `search` or `list all` request is given to a Melodyne client, it asks all connected nodes for a copy of their database and builds
the global collection databse from them. For subsequent calls, it only requests the databases that have been modified.

### Encryption
In order to keep messages and audio streams between nodes isolated and secure, all communication is encrypted between nodes and in order for clients
to connect to each other, they must all share the same (private) symmetric key. This prevents other Melograph nodes from eavesdropping on
communication between clients or sending spoofed messages to them.

### Streaming
If a Melograph client requests a song that isn't available on the local system, it asks the client that _does_ have it to initiate a new music stream.
This stream can either be transcoded on the fly by the remote node, or it can be sent unmodified if the player supports the specific audio codec.

### Interfacing
Melograph clients can be interfaced with by using the default CLI, the built-in RESTful API, or through various web interfaces and apps. For most
users, the latter is the easiest and most user-friendly option.
