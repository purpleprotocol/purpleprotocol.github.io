---
layout: page
title: Blocks
wikiPageName: Blocks
menu: wiki
---

Once an event has been advanced from the causal graph, it is appended by each sequencer to a locally kept buffer. Once the buffer reaches the maximum allowed size, the node creates a block that contains the events and broadcasts it to the network.

### Choosing criteria
If two blocks with the same parent hash are received by the same node, the block that will be directly followed by most subsequent blocks will be chosen as being the correct one.

### Binary encoding structure
The following table is a reference for the binary encoding of a `Block`. All fields are in Big Endian byte order.

| Field name               | Size            | Description                                                    |
| -------------------------|-----------------| ---------------------------------------------------------------|
| Events length            | 32bits          | The length of the events field                                 |
| Parent hash              | 32bytes         | The hash of the parent block                                   |
| Root hash                | 32bytes         | The root hash of the block's events                            |
| Hash                     | 32bytes         | The hash of the transaction                                    |
| Events                   | Variable length | The events contained in the block                              |

