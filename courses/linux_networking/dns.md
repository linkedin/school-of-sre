# DNS
Domain Names are the simple human-readable names for websites. The Internet understands only IP addresses, but since memorizing incoherent numbers is not practical, domain names are used instead. These domain names are translated into IP addresses by the DNS infrastructure. When somebody tries to open [www.linkedin.com](https://www.linkedin.com) in the browser, the browser tries to convert [www.linkedin.com](https://www.linkedin.com) to an IP Address. This process is called DNS resolution. A simple pseudocode depicting this process looks this

```python
ip, err = getIPAddress(domainName)
if err:
  print(“unknown Host Exception while trying to resolve:%s”.format(domainName))
```

Now let’s try to understand what happens inside the getIPAddress function. The browser would have a DNS cache of its own where it checks if there is a mapping for the domainName to an IP Address already available, in which case the browser uses that IP address. If no such mapping exists, the browser calls gethostbyname syscall to ask the operating system to find the IP address for the given domainName

```python
def getIPAddress(domainName):
    resp, fail = lookupCache(domainName)
    If not fail:
       return resp
    else:
       resp, err = gethostbyname(domainName)
       if err:
         return null, err
       else:
          return resp
```

Now lets understand what operating system kernel does when the [gethostbyname](https://man7.org/linux/man-pages/man3/gethostbyname.3.html) function is called. The Linux operating system looks at the file [/etc/nsswitch.conf](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html) file which usually has a line

```bash
hosts:      files dns
```

This line means the OS has to look up first in file (/etc/hosts) and then use DNS protocol to do the resolution if there is no match in /etc/hosts. 

The file /etc/hosts is of format


IPAddress FQDN [FQDN].*

```bash
127.0.0.1 localhost.localdomain localhost
::1 localhost.localdomain localhost
```

If a match exists for a domain in this file then that IP address is returned by the OS. Lets add a line to this file

```bash
127.0.0.1 test.linkedin.com
```

And then do ping test.linkedin.com

```bash
ping test.linkedin.com -n
```

```bash
PING test.linkedin.com (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.047 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.036 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.037 ms

```

As mentioned earlier, if no match exists in /etc/hosts, the OS tries to do a DNS resolution using the DNS protocol. The linux system makes a DNS request to the first IP in /etc/resolv.conf. If there is no response, requests are sent to subsequent servers in resolv.conf. These servers in resolv.conf are called DNS resolvers. The DNS resolvers are populated by [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) or statically configured by an administrator. 
[Dig](https://linux.die.net/man/1/dig) is a userspace DNS system which creates and sends request to DNS resolvers and prints the response it receives to the console.

```bash
#run this command in one shell to capture all DNS requests
sudo tcpdump -s 0 -A -i any port 53
#make a dig request from another shell
dig linkedin.com
```

```bash 
13:19:54.432507 IP 172.19.209.122.56497 > 172.23.195.101.53: 527+ [1au] A? linkedin.com. (41)
....E..E....@.n....z...e...5.1.:... .........linkedin.com.......)........
13:19:54.485131 IP 172.23.195.101.53 > 172.19.209.122.56497: 527 1/0/1 A 108.174.10.10 (57)
....E..U..@.|.	....e...z.5...A...............linkedin.com..............3..l.

..)........
```


The packet capture shows a request is made to 172.23.195.101:53 (this is the resolver in /etc/resolv.conf) for linkedin.com and a response is received from 172.23.195.101 with the IP address of linkedin.com 108.174.10.10

Now let's try to understand how DNS resolver tries to find the IP address of linkedin.com. DNS resolver first looks at its cache. Since many devices in the network can query for the domain name linkedin.com, the name resolution result may already exist in the cache. If there is a cache miss, it starts the DNS resolution process. The DNS server breaks “linkedin.com” to “.”, “com.” and “linkedin.com.” and starts DNS resolution from “.”. The “.” is called root domain and those IPs are known to the DNS resolver software. DNS resolver queries the root domain Nameservers to find the right nameservers which could respond regarding details for "com.". The address of the authoritative nameserver of “com.” is returned. Now the DNS resolution service contacts the authoritative nameserver for “com.” to fetch the authoritative nameserver for “linkedin.com”. Once an authoritative nameserver of “linkedin.com” is known, the resolver contacts Linkedin’s nameserver to provide the IP address of “linkedin.com”. This whole process can be visualized by running 

```bash
dig +trace linkedin.com
```


```bash
linkedin.com.		3600	IN	A	108.174.10.10
```
This DNS response has 5 fields where the first field is the request and the last field is the response. The second field is the Time to Live which says how long the DNS response is valid in seconds. In this case this mapping of linkedin.com is valid for 1 hour. This is how the resolvers and application(browser) maintain their cache. Any request for linkedin.com beyond 1 hour will be treated as a cache miss as the mapping has expired its TTL and the whole process has to be redone.
The 4th field says the type of DNS response/request. Some of the various DNS query types are
A, AAAA, NS, TXT, PTR, MX and CNAME. 
- A record returns IPV4 address of the domain name 
- AAAA record returns the IPV6 address of the domain Name
- NS record returns the authoritative nameserver for the domain name
- CNAME records are aliases to the domain names. Some domains point to other domain names and resolving the latter domain name gives an IP which is used as an IP for the former domain name as well. Example www.linkedin.com’s IP address is the same as 2-01-2c3e-005a.cdx.cedexis.net. 
- For the brevity we are not discussing other DNS record types, the RFC of each of these records are available [here](https://en.wikipedia.org/wiki/List_of_DNS_record_types).

```bash
dig A linkedin.com +short
108.174.10.10


dig AAAA linkedin.com +short
2620:109:c002::6cae:a0a


dig NS linkedin.com +short
dns3.p09.nsone.net.
dns4.p09.nsone.net.
dns2.p09.nsone.net.
ns4.p43.dynect.net.
ns1.p43.dynect.net.
ns2.p43.dynect.net.
ns3.p43.dynect.net.
dns1.p09.nsone.net.

dig www.linkedin.com CNAME +short
2-01-2c3e-005a.cdx.cedexis.net.
```
Armed with these fundamentals of DNS lets see usecases where DNS is used by SREs.

## Applications in SRE role

This section covers some of the common solutions SRE can derive from DNS
1. Every company has to have its internal DNS infrastructure for intranet sites and internal services like databases and other internal applications like wiki. So there has to be a DNS infrastructure maintained for those domain names by the infrastructure team. This DNS infrastructure has to be optimized and scaled so that it doesn’t become a single point of failure. Failure of the internal DNS infrastructure can cause API calls of microservices to fail and other cascading effects.
2. DNS can also be used for discovering services. For example the hostname serviceb.internal.example.com could list instances which run service b internally in example.com company. Cloud providers provide options to enable DNS discovery([example](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/service-discovery.html#dns-based-service-discovery))
3. DNS is used by cloud provides and CDN providers to scale their services. In Azure/AWS, Load Balancers are given a CNAME instead of IPAddress. They update the IPAddress of the Loadbalancers as they scale by changing the IP Address of alias domain names. This is one of the reasons why A records of such alias domains are short lived like 1 minute.
4. DNS can also be used to make clients get IP addresses closer to their location so that their HTTP calls can be responded faster if the company has a presence geographically distributed. 
5. SRE also has to understand since there is no verification in DNS infrastructure, these responses can be spoofed. This is safeguarded by other protocols like HTTPS(dealt later). DNSSEC protects from forged or manipulated DNS responses.
6. Stale DNS cache can be a problem. Some [apps](https://stackoverflow.com/questions/1256556/how-to-make-java-honor-the-dns-caching-timeout) might still be using expired DNS records for their api calls. This is something SRE has to be wary of when doing maintenance.
7. DNS Loadbalancing and service discovery also has to understand TTL and the servers can be removed from the pool only after waiting till TTL post the changes are made to DNS records. If this is not done, a certain portion of the traffic will fail as the server is removed before the TTL.





