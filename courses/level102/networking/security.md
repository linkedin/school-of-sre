> *This section will cover threat vectors faced by services facing
external/internal clients. Potential mitigation options to consider
while deploying them. This will touch upon perimeter security, DDoS
protection, Network demarcation and operational practices.*

### Security Threat

Security is one of the major considerations in any infrastructure. There
are various security threats, which could amount to data theft, loss of
service, fraudulent activity, etc. An attacker can use techniques like
phishing, spamming, malware, Dos/DDoS, exploiting vulnerabilities,
man-in-the-middle attack, and many more. In this section, we will cover
some of these threats and possible mitigation. As there are numerous
means to attack and secure the infrastructure, we will only focus on
some of the most common ones.

**Phishing** is mostly done via email (and other mass communication
methods), where an attacker provides links to fake websites/URLs. Upon
accessing that, victim's sensitive information like login
credentials or personal data is collected and can be misused.

**Spamming** is also similar to phishing, but the attacker doesn't collect
data from users but tries to spam a particular website and probably
overwhelm them (to cause slowness) and well use that opportunity to,
compromise the security of the attacked website.

**Malware** is like a trojan horse, where an attacker manages to install a
piece of code on the secured systems in the infrastructure. Using this,
the hacker can collect sensitive data and as well infect the critical
services of the target company.

**Exploiting vulnerabilities** is another method an attacker can gain access
to the systems. These could be bugs or misconfiguration in web servers,
internet-facing routers/switches/firewalls, etc.

**DoS/DDoS** is one of the common attacks seen on internet-based
services/solutions, especially those businesses based on eyeball
traffic. Here the attacker tries to overwhelm the resources of the
victim by generating spurious traffic to the external-facing services.
By this, primarily the services turn slow or non-responsive, during this
time, the attacker could try to hack into the network, if some of the
security mechanism fails to filter through the attack traffic due to
overload.

### Securing the infrastructure

The first and foremost aspect for any infrastructure administration is
to identify the various security threats that could affect the business
running over this infrastructure. Once different threats are known, the
security defence mechanism has to be designed and implemented. Some of
the common means to securing the infrastructure are

#### Perimeter security

This is the first line of defence in any infrastructure, where
unwanted/unexpected traffic flows into the infrastructure are
filtered/blocked. These could be filters in the edge routers, that allow
expected services (like port 443 traffic for web service running on
HTTPS), or this filter can be set up to block unwanted traffic, like
blocking UDP ports, if the services are not dependent on UDP.

Similar to the application traffic entering the network, there could be
other traffic like BGP messages for Internet peers, VPN tunnels traffic,
as well other services like email/DNS, etc. There are means to protect
every one of these, like using authentication mechanisms (password or
key-based) for peers of BGP, VPN, and whitelisting these specific peers
to make inbound connections (in perimeter filters). Along with these,
the amount of messages/traffic can be rate-limited to known scale or
expected load, so the resources are not overwhelmed.

#### DDoS mitigation

Protecting against a DDoS attack is another important aspect. The attack
traffic will look similar to the genuine users/client request, but with
the intention to flood the externally exposed app, which could be a web
server, DNS, etc. Therefore it is essential to differentiate between the
attack traffic and genuine traffic, for this, there are different
methods to do at the application level, one such example using Captcha
on a web service, to catch traffic originating from bots.

For these methods to be useful, the nodes should be capable of handling
both the attack traffic and genuine traffic. It may be possible in
cloud-based infrastructure to dynamically add more virtual
machines/resources, to handle the sudden spike in volume of traffic, but
on-prem, the option to add additional resources might be challenging.

To handle a large volume of attack traffic, there are solutions
available, which can inspect the packets/traffic flows and identify
anomalies (i.e.) traffic patterns that don't resemble a genuine
connection, like client initiating TCP connection, but fail to complete
the handshake, or set of sources, which have abnormally huge traffic
flow. Once this unwanted traffic is identified, these are dropped at the
edge of the network itself, thereby protecting the resources of app
nodes. This topic alone can be discussed more in detail, but that will
be beyond the scope of this section.

#### Network Demarcation

Network demarcation is another common strategy deployed in different
networks when applications are grouped based on their security needs and
vulnerability to an attack. Some common demarcations are, the
external/internet facing nodes are grouped into a separate zone, whereas
those nodes having sensitive data are segregated into a separate zone.
And any communication between these zones is restricted with the help of
security tools to limit exposure to unwanted hosts/ports. These
inter-zone communication filters are sometimes called ring-fencing. The
number of zones to be created, varies for different deployments, for
example, there could be a host which should be able to communicate to
the external world as well as internal servers, like proxy, email, in
this case, these can be grouped under one zone, say De-Militarized Zones
(DMZ). The main advantage of creating zones is that, even if there is a
compromised host, that doesn't act as a back door entry for the rest of
the infrastructure.

####  Node protection

Be it server, router, switches, load balancers, firewall, etc, each of
these devices come with certain capabilities to secure themselves, like
support for filters (e.g. Access-list, Iptables) to control what traffic
to process and what to drop, anti-virus software can be used in servers
to check on the software installed in them.

#### Operational practices

There are numerous security threats for infrastructure, and there are
different solutions to defend them. The key part to the defence, is not
only identifying the right solution and the tools for it but also making
sure there are robust operational procedures in place, to respond
promptly, decisively and with clarity, for any security incident.

##### Standard Operating Procedures (SOP)

SOP need to be well defined and act as a reference for on-call to follow
during a security incident. This SoP should cover things like,

- When a security incident happens, how it will be alerted, to whom it
will be alerted.

- Identify the scale and severity of the security incident.

- Who are the points of escalation and the threshold/time to intimate
them, there could be other concerned teams or to the management or
even to the security operations in-charge.

- Which solutions to use (and the procedure to follow in them) to
mitigate the security incident.

- Also the data about the security incident has to be collated for
further analysis.

Many organisations have a dedicated team focused on security, and they
drive most of the activities, during an attack and even before, to come
up with best practices, guidelines and compliance audits. It is the
responsibility of respective technical teams, to ensure the
infrastructure meets these recommendations and gaps are fixed.

##### Periodic review

Along with defining SoP's, the entire security of the infrastructure has
to be reviewed periodically. This review should include,

- Identifying any new/improved security threat that could potentially
target the infrastructure.

- The SoP's have to be reviewed periodically, depending upon new
security threats or changes in the procedure (to implement the
solutions)

- Ensuring software upgrades/patches are done in a timely manner.

- Audit the infrastructure for any non-compliance of the security
standards.

- Review of recent security incidents and find means to improvise the
defence mechanisms.
