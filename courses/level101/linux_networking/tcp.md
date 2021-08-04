# TCP

TCP is a transport layer protocol like UDP but it guarantees reliability, flow control and congestion control.
TCP guarantees reliable delivery by using sequence numbers. A TCP connection is established by a three way handshake. In our case, the client sends a SYN packet along with the starting sequence number it plans to use, the server acknowledges the SYN packet and sends a SYN with its sequence number. Once the client acknowledges the syn packet, the connection is established. Each data transferred from here on is considered delivered reliably once acknowledgement for that sequence is received by the concerned party

![3-way handshake](images/established.png)

```bash
#To understand handshake run packet capture on one bash session
tcpdump -S -i any port 80
#Run curl on one bash session
curl www.linkedin.com
```

![tcpdump-3way](images/pcap.png)


Here client sends a syn flag shown by [S] flag with a sequence number 1522264672. The server acknowledges receipt of SYN with an ack [.] flag and a Syn flag for its sequence number[S]. The server uses the sequence number 1063230400 and acknowledges the client it’s expecting sequence number 1522264673 (client sequence+1). Client sends a zero length acknowledgement packet to the server(server sequence+1) and connection stands established. This is called three way handshake. The client sends a 76 bytes length packet after this and increments its sequence number by 76. Server sends a 170 byte response and closes the connection. This was the difference we were talking about between HTTP/1.1 and HTTP/1.0. In HTTP/1.1 this same connection can be reused which reduces overhead of 3 way handshake for each HTTP request. If a packet is missed between client and server, server won’t send an ack to the client and client would retry sending the packet till the ACK is received. This guarantees reliability.
The flow control is established by the win size field in each segment. The win size says available TCP buffer length in the kernel which can be used to buffer received segments. A size 0 means the receiver has a lot of lag to catch from its socket buffer and the sender has to pause sending packets so that receiver can cope up. This flow control protects from slow receiver and fast sender problem

TCP also does congestion control which determines how many segments can be in transit without an ack. Linux provides us the ability to configure algorithms for congestion control which we are not covering here.

While closing a connection, client/server calls a close syscall. Let's assume client do that. Client’s kernel will send a FIN packet to the server. Server’s kernel can’t close the connection till the close syscall is called by the server application. Once server app calls close, server also sends a FIN packet and client enters into time wait state for 2*MSS(120s) so that this socket can’t be reused for that time period to prevent any TCP state corruptions due to stray stale packets. 

![Connection tearing](images/closed.png)

Armed with our TCP and HTTP knowledge lets see how this is used by SREs in their role

## Applications in SRE role
1. Scaling HTTP performance using load balancers need consistent knowledge about both TCP and HTTP. There are [different kinds of load balancing](https://blog.envoyproxy.io/introduction-to-modern-network-load-balancing-and-proxying-a57f6ff80236?gi=428394dbdcc3) like L4, L7 load balancing, Direct Server Return etc. HTTPs offloading can be done on Load balancer or directly on servers based on the performance and compliance needs.
2. Tweaking sysctl variables for rmem and wmem like we did for UDP can improve throughput of sender and receiver.
3. Sysctl variable tcp_max_syn_backlog and socket variable somax_conn determines how many connections for which the kernel can complete 3 way handshake before app calling accept syscall. This is much useful in single threaded applications. Once the backlog is full, new connections stay in SYN_RCVD state (when you run netstat) till the application calls accept syscall
4. Apps can run out of file descriptors if there are too many short lived connections. Digging through [tcp_reuse and tcp_recycle](http://lxr.linux.no/linux+v3.2.8/Documentation/networking/ip-sysctl.txt#L464) can help reduce time spent in the time wait state(it has its own risk). Making apps reuse a pool of connections instead of creating ad hoc connection can also help
5. Understanding performance bottlenecks by seeing metrics and classifying whether its a problem in App or network side. Example too many sockets in Close_wait state is a problem on application whereas retransmissions can be a problem more on network or on OS stack than the application itself. Understanding the fundamentals can help us narrow down where the bottleneck is

