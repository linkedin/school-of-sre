
# Prerequisites

It is recommended to have basic knowledge of network security, TCP and
datacenter setup and the common terminologies used in them. Also, the
readers are expected to go through the School of Sre contents - 

- [Linux Networking](http://linkedin.github.io/school-of-sre/level101/linux_networking/intro/)

- [system design](http://linkedin.github.io/school-of-sre/level101/systems_design/intro/)

- [security](http://linkedin.github.io/school-of-sre/level101/security/intro/)

# What to expect from this course

This part will cover how a datacenter infrastructure is segregated for different
application needs as well as the consideration of deciding where to
place an application. These will be broadly based on, Security, Scale,
RTT (latency), Infrastructure features.

Each of these topics will be covered in detail,

Security - Will cover threat vectors faced by services facing
external/internal clients. Potential mitigation options to consider
while deploying them. This will touch upon perimeter security, [DDoS](https://en.wikipedia.org/wiki/Denial-of-service_attack)
protection, Network demarcation and ring-fencing the server clusters.

Scale - Deploying large scale applications, require a better
understanding of infrastructure capabilities, in terms of resource
availability, failure domains, scaling options like using anycast, layer
4/7 load balancer, DNS based load balancing.

RTT (latency) - Latency plays a key role in determining the overall
performance of the distributed service/application, where calls are made
between hosts to serve the users.

Infrastructure features - Some of the aspects to consider are, whether
the underlying data centre infrastructure supports ToR resiliency, i.e.,
features like link bundling (bonds), BGP (Border Gateway Protocol), support for anycast service,
load balancer, firewall, Quality of Service.

# What is not covered under this course

Though these parameters play a role in designing an application, we will
not go into the details of the design. Each of these topics are vast, hence the objective is to introduce the terms and relevance of the parameters in them, and not to provide extensive details about each one of them.

#  Course Contents
1. [Security](http://linkedin.github.io/school-of-sre/level102/networking/security/)
2. [Scale](https://linkedin.github.io/school-of-sre/level102/networking/scale/)
3. [RTT](http://linkedin.github.io/school-of-sre/level102/networking/rtt/)
4. [Infrastructure features](http://linkedin.github.io/school-of-sre/level102/networking/Infrastructure-features/)
5. [Conclusion](http://linkedin.github.io/school-of-sre/level102/networking/Conclusion/)


# Terminology
 
Before discussing each of the topics, it is important to get familiar with few commonly used terms
 
Cloud
 
This refers to hosted solutions from different providers like Azure, AWS, GCP. Wherein enterprises can host their applications for either public or private usage.
 
On-prem
 
This term refers to physical Data Center(DC) infrastructure, built and managed by enterprises themselves. This can be used for private access as well as public (like users connecting over the Internet).
 
Leaf switch (ToR)
 
This refers to the switch, where the servers connect to, in a DC. They are called by many names, like access switch, Top of the Rack switch, Leaf switch.
 
The term leaf switch comes from the [Spine-leaf architecture](https://searchdatacenter.techtarget.com/definition/Leaf-spine), where the access switches are called leaf switches. Spine-leaf architecture is commonly used in large/hyper-scale data centres, which brings very high scalability options for the DC switching layer and is also more efficient in building and implementing these switches. Sometimes these are referred to as Clos architecture.
 
Spine switch
 
Spine switches are the aggregation point of several leaf switches, they provide the inter-leaf communication and also connect to the upper layer of DC infrastructure.
 
DC fabric
 
As the data centre grows, multiple Clos networks need to be interconnected, to support the scale, and fabric switches help to interconnect them.
 
Cabinet
 
This refers to the rack, where the servers and ToR are installed. One cabinet refers to the entire rack.
 
BGP
 
It is the Border Gateway Protocol, used to exchange routing information between routers and switches. This is one of the common protocols used in the Internet and as well Data Centers as well. Other protocols are also used in place of BGP, like OSPF.
 
VPN
 
A Virtual Private Network is a tunnel solution, where two private networks (like offices, datacentres, etc) can be interconnected over a public network (internet). These VPN tunnels encrypt the traffic before sending over the Internet, as a security measure.
 
NIC
 
Network Interface Card refers to the module in Servers, which consists of the Ethernet port and the interconnection to the system bus. It is used to connect to the switches (commonly ToR switches).
 
Flow
 
Flows refer to a traffic exchange between two nodes (could be servers, switches, routers, etc), which has common parameters like source/destination IP address, source/destination port number, IP Protocol number. This helps in traffic a particular traffic exchange session, between two nodes (like a file copy session, or an HTTP connection, etc).
 
ECMP
 
Equal Cost Multi-Path means, a switch/router can distribute the traffic to a destination, among multiple exit interfaces. The flow information is used to build a hash value and based on that, exit interfaces are selected. Once a flow is mapped to a particular exit interface, all the packets of that flow exit via the same interface only. This helps in preventing out of order delivery of packets.
 
RTT
 
This is a measure of the time it takes for a packet from the source to reach the destination and return to the source. This is most commonly used in measuring network performance and also troubleshooting.
 
TCP throughput
 
This is the measure of the data transfer rate achieved between two nodes. This is impacted by many parameters like RTT, packet size, window size, etc.

Unicast

This refers to the traffic flow between a single source to a single destination (i.e.) like ssh sessions, where there is one to one communication.

Anycast

This refers to one-to-one traffic flow as above, but endpoints could be multiple (i.e.) a single source can send traffic to any one of the destination hosts in that group. This is achieved by having the same IP address configured in multiple servers and every new traffic flow is mapped to one of the servers.

Multicast

This refers to one-to-many traffic flow (i.e.) a single source can send traffic to multiple destinations. To make it feasible, the network routers replicate the traffic to different hosts (which register as members of that particular multicast group).
