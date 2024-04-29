# IP Routing and Data Link Layer
We will dig how packets that leave the client reach the server and vice versa. When the packet reaches the IP layer, the transport layer populates source port, destination port. IP/Network layer populates destination IP (discovered from DNS) and then looks up the route to the destination IP on the routing table. 

```bash
# Linux `route -n` command gives the default routing table
route -n
```

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.17.0.1      0.0.0.0         UG    0      0        0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth0
```

Here, the destination IP is bitwise AND’d with the Genmask and if the answer is the destination part of the table, then that gateway and interface is picked for routing. Here, [linkedin.com](https://www.linkedin.com)’s IP `108.174.10.10` is AND’d with `255.255.255.0` and the answer we get is `108.174.10.0` which doesn’t match with any destination in the routing table. Then, Linux does an AND of destination IP with `0.0.0.0` and we get `0.0.0.0`. This answer matches the default row.

Routing table is processed in the order of more octets of 1 set in Genmask and Genmask `0.0.0.0` is the default route if nothing matches. 
At the end of this operation, Linux figured out that the packet has to be sent to next hop `172.17.0.1` via `eth0`. The source IP of the packet will be set as the IP of interface `eth0`. 
Now, to send the packet to `172.17.0.1`, Linux has to figure out the MAC address of `172.17.0.1`. MAC address is figured by looking at the internal ARP cache which stores translation between IP address and MAC address. If there is a cache miss, Linux broadcasts ARP request within the internal network asking who has `172.17.0.1`. The owner of the IP sends an ARP response which is cached by the kernel and the kernel sends the packet to the gateway by setting Source MAC address as MAC address of `eth0` and destination MAC address of `172.17.0.1` which we got just now. Similar routing lookup process is followed in each hop till the packet reaches the actual server. Transport layer and layers above it come to play only at end servers. During intermediate hops, only till the IP/Network layer is involved.

![Screengrab for above explanation](images/arp.gif)

One weird gateway we saw in the routing table is `0.0.0.0`. This gateway means no Layer3 (Network layer) hop is needed to send the packet. Both source and destination are in the same network. Kernel has to figure out the MAC of the destination and populate source and destination MAC appropriately and send the packet out so that it reaches the destination without any Layer3 hop in the middle.

As we followed in other modules, let's complete this session with SRE use cases.

## Applications in SRE role
1. Generally the routing table is populated by DHCP and playing around is not a good practice. There can be reasons where one has to play around the routing table but take that path only when it's absolutely necessary.
2. Understanding error messages better like, “No route to host” error can mean MAC address of the destination host is not found and it can mean the destination host is down. 
3. On rare cases, looking at the ARP table can help us understand if there is a IP conflict where same IP is assigned to two hosts by mistake and this is causing unexpected behavior.

