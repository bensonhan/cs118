###### Benson Han 604665553 
## HW 3 Disc 1C
### 1.
a) Padding because there is no longer a minimum packet size. Instead of stuffing bits after a shorter message, the protocol will just wait out a timer in order to detect a collision
 
Jamming (for small packets) because Nethernet's wait timer functions to serve the same effect. Instead of requiring the receiver to send a few extra bits to extend the length of the collided signal, the sender will know if there's a collision because it will receive a signal within it's 51.2 usec timer.

b) No because runts packets are allowed in Nethernet unlike in Ethernet. In Ethernet, packets smaller than 64 bytes will not last long enough to generate a collision signal that makes it back to the sender to notify it of the collision. However, in Nethernet, the collision can still be detected for small packets because the sender listens for ANY transmission for 51.2 usec after the packet is sent. If any signal is detected that means that there was a collision.

c) See figure 1

In this configuration, there's a station C in between A and B that is in just the right spot to hear the collision between the signals sent by A and B. If Nethernet does not employ the normal means of detecting collisions (sensing a voltage level higher than the DC balance), then station C will mistake the collision between A and B as a signal instead of a collision. It will do so because C has not yet transmitted anything so there is no timer that tells it to be on the lookout for signals that would mean a collision occurred in the 51.2 usec window.

d) See figure 1

Now suppose there is another station D in between stations A and C such that all the signals that pass through D are the normal DC-balanced voltage. In this case, even when there is a collision between A and B, it happens far away enough that D doesn't detect the collision. The signal that D gets from B - which D thinks is clean - is actually corrupted but since D doesn't detect the collision, it is none the wiser.

e) See figure 1

In this scenario, A is sending a packet to B and B is sending a packet to D. There is a collision between these two sent packets. However, because of the results of our reasoning in part d) station D does not detect the collision that B does. B will then retransmit the packet to D. D will then accept this duplicate packet thinking it's the next packet. 
### 2.
a) See figure 2

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
In each step of the distance vector routing algorithm, it should also compute and store the:
``` 
MinMaxPS(D,R) = min(MinMaxPS(D,N), MinMaxPS(R,N))
```
This will store the minimum of the maximum packet sizes of all the links on the best route to each destination D from the routers R.