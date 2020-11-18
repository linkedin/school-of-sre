# Conclusion

With this we have traversed through the TCP/IP stack completely. We hope there will be a different perspective when one opens any website in the browser post the course.

During the course we have also dissected what are common tasks in this pipeline which falls under the ambit of SRE.

# Post Training Exercises
1. Setup own DNS resolver in the dev environment which acts as an authoritative DNS server for example.com and forwarder for other domains. Update resolv.conf to use the new DNS resolver running in localhost
2. Set up a site dummy.example.com in localhost and run a webserver with a self signed certificate. Update the trusted CAs or pass self signed CA’s public key as a parameter so that curl https://dummy.example.com -v works properly without self signed cert warning
3. Update the routing table to use another host(container/VM) in the same network as a gateway for 8.8.8.8/32 and run ping 8.8.8.8. Do the packet capture on the new gateway to see L3 hop is working as expected(might need to disable icmp_redirect)

