###### Benson Han 604665553 
### 1.
a) Padding
 
Jamming (for small packets) because Nethernet's wait timer functions to serve the same effect. Instead of requiring the receiver to send a few extra bits to extend the length of the collided signal, the sender will know if there's a collision because it will receive a signal within it's 51.2 usec timer.

b) No because runts packets are allowed in Nethernet unlike in Ethernet. In Ethernet, packets smaller than 64 bytes will not last long enough to generate a collision signal that makes it back to the sender to notify it of the collision. However, in Nethernet, the collision can still be detected for small packets because the sender listens for ANY transmission for 51.2 usec after the packet is sent. If any signal is detected that means that there was a collision.

c)  
### 2.
a) See figure 3

In this topology both X and Y are on the same Ethernet while the INTRO server is on the other. X will send a message across the bridge to the INTRO server. Then, the INTRO server will send a message back across the bridge to Y. Since the message will contain the headers...
* DEST: Y
* SRC: X
* DATA: H

...the bridge - which learns based on the SRC - will update it's table and mark X on the side of INTRO which is incorrect. Now anytime a message is sent to the bridge from the side of INTRO with the destination X it will be dropped because the bridge thinks X is on that side.

b) The introduction protocol can be changed so that the message sent from the INTRO server contains the headers...
* DEST: Y
* SRC: INTRO
* DATA: H

In this way the bridge won't mistakenly mark X on the same side as the INTRO server as that is not always the case. Instead, the bridge will mark the INTRO server on the correct side.
### 3.
Connected by a bridge
* B sends out an ARP request to all the other end-nodes to find the Data Link Address of A
* Nodes C, D, and E drop the request because they are not the destined target
* Node A receives the ARP request and sends B it's MAC address - this is where things get messy
* Since A incorrectly thinks it's MAC address is the broadcast address and tell B this, anytime B wants to send A a message it will use the broadcast address and end up sending the message to everyone in the network - including those connected by the bridge (nodes A, C, D, and E)

Connected by an IP router

The problem gets slightly better with an IP router. When there is an IP router instead of a bridge, each LAN now has it's own mask for their respective IP prefixes. Now, when B sends a message to A, it will only be broadcast to the nodes on B's side of the IP router. In this case, it will only send the message to nodes A and C. This is still a problem, but definitely a smaller one.
### 4.
In each step of the distance vector routing algorithm, it should also compute and store:

