---
layout: page
title: Packets
wikiPageName: Packets
menu: wiki
---

This document is intended to provide a specification for the way in which nodes running Purple Core communicate with each other at the network layer. This specification is intended to be a reference for core protocol developers and for alternative implementations of the protocol.

### Table of Contents
1. [Packet Header](#packet-header)
   1. [Description](#packet-header-description)
   2. [Binary encoding](#packet-header-binary-encoding)
2. [Connect](#connect)
   1. [Description](#connect-description)
   2. [Connection Protocol](#connect-protocol)
   2. [Binary encoding](#connect-bin-encoding)
3. [Request Peers](#request-peers)
   1. [Description](#request-peers-description)
   2. [Binary encoding](#request-peers-encoding)
4. [Send Peers](#send-peers)
   1. [Description](#send-peers-description)
   2. [Binary encoding](#send-peers-bin-encoding)

### Packet header <a name="packet-header"></a>
A packet header that wraps all other packets.

#### Description <a name="packet-header-description"></a>
When reading from the TCP stream, a fixed size header of `7 bytes` is first read and decoded. This contains information about the packet that is to be received next.

#### Binary Encoding <a name="packet-header-binary-encoding"></a>
This section describes the binary encoding of a `Packet Header`. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                           |
| -------------------------|-----------------| ----------------------------------------------------------------------|
| Network layer version    | 8bits           | The version of the network layer                                      |
| Packet length            | 16bits          | The length of the packet field                                        |
| Checksum                 | 32bits          | The CRC32 checksum of the packet field + the name of the network      |
| Packet                   | Variable length | The packet                                                            |


### Connect <a name="connect"></a>
A `Connect` packet is used for connecting to a new peer.

#### Description <a name="connect-description"></a>
Nodes running Purple Core participate in the network by establishing a TCP connection to a set of peers, by default 8, through which all communication with the network is performed. A `Connect` packet is used for establishing a connection to a specific peer and for exchanging encryption keys for any subsequent traffic. A `Connect` packet is the only type of packet that is sent **unencrypted**. 

#### Connection Protocol <a name="connect-protocol"></a>
In order for `Peer1` and `Peer2` to achieve a connection they must follow the following steps:
1. `Peer1` must know the ip and port of `Peer2`. These can be obtained from a bootnode when initially connecting.
2. `Peer1` establishes a TCP connection to `Peer2`.
3. `Peer1` generates `(PK1, SK1)` and attaches `PK1` to a `Connect` packet.
4. `Peer1` sends the `Connect` packet to `Peer2`.
5. `Peer2` receives the packet and generates `PK2, SK2`.
6. `Peer2` attaches `PK2` to another `Connect` packet which it sends back to `Peer1`.
7. `Peer1` and `Peer2` having performed the Diffie-Hellmann key exchange, now compute their encryption keys.
8. `Peer1` and `Peer2` are now connected and can send each other encrypted packets.

#### Binary Encoding <a name="connect-bin-encoding"></a>
This section describes the binary encoding of a `Connect` packet. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                           |
| -------------------------|-----------------| ----------------------------------------------------------------------|
| Packet type(1)           | 8bits           | The type of the packet                                                |
| Timestamp length         | 8bits           | The length of the timestamp field                                     |
| Network layer version    | 8bits           | The version of the network layer                                      |
| Network name hash        | 32bytes         | The hash of the network name                                          |
| Key exchange Public Key  | 32bytes         | The session generated public key used for Diffie-Hellmann key exchange|
| Node Id                  | 32bytes         | The id of the connecting node                                         |
| Signature                | 64bytes         | The node's signature .                                                |
| Timestamp                | Variable length | RFC 3339 encoded timestamp                                            |

### Request Peers <a name="request-peers"></a>
A `RequestPeers` packet is used by a node to query another node for a number of peers.

#### Description <a name="request-peers-description"></a>
In order to populate the peer list of a freshly bootstrapped node, it must request peers from its existing connections. The node sends a `RequestPeers` packet to any of its existing peers which states the number of required nodes. The peers respond with a `SendPeers` packet which contains a list of peer addresses. 

#### Binary Encoding <a name="request-peers-bin-encoding"></a>
This section describes the binary encoding of a `RequestPeers` packet. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                           |
| -------------------------|-----------------| ----------------------------------------------------------------------|
| Packet type(2)           | 8bits           | The type of the packet                                                |
| Timestamp length         | 8bits           | The length of the timestamp field                                     |
| Requested Peers          | 8bits           | The number of requested peers                                         |
| Node Id                  | 32bytes         | The id of the node                                                    |
| Signature                | 64bytes         | The signature of node                                                 |
| Timestamp                | Variable length | RFC 3339 encoded timestamp                                            |

### Send Peers <a name="send-peers"></a>
A `SendPeers` packet is used for sending peer lists to other peers.

#### Description <a name="send-peers-description"></a>
After receiving a `RequestPeers` packet, a node will respond with a `SendPeers` packet which contains the ip addresses of randomly selected peers from the node's connection list. The sender will choose addresses of nodes based on their reliability i.e. uptime, reputation and time since last connection.

#### Binary Encoding <a name="send-peers-bin-encoding"></a>
This section describes the binary encoding of a `SendPeers` packet. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                           |
| -------------------------|-----------------| ----------------------------------------------------------------------|
| Packet type(3)           | 8bits           | The type of the packet                                                |
| Timestamp length         | 8bits           | The length of the timestamp field                                     |
| Peers length .           | 16bits          | The length of the peers field                                         |
| Node Id                  | 32bytes         | The id of the node                                                    |
| Signature                | 64bytes         | The signature of node                                                 |
| Timestamp                | Variable length | RFC 3339 encoded timestamp                                            |
| Peers                    | Variable length | RLP encoded list of peer addresses. Can be ipv4 or ipv6               |
