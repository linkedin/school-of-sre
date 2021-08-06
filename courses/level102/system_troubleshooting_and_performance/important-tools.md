### Important linux commands

Having knowledge of following commands will help find issues faster. Elaborating each command in detail is out of scope, please look for man pages or online for more information and examples around the same.

- For logs parsing -: grep, sed, awk, cut, tail, head
- For network checks -: nc, netstat, traceroute/6, mtr, ping/6, route, tcpdump, ss, ip
- For DNS -: dig, host, nslookup
- For tracing system call -: strace
- For parallel executions over ssh -: gnu parallel, xargs + ssh.
- For http/s checks -: curl, wget
- For list of open files -: lsof
- For modifying attributes of the system kernel -: [sysctl](https://man7.org/linux/man-pages/man8/sysctl.8.html)

In case of distributed systems, some good third party tools can help to execute commands/instructions on many hosts at once, like:

- **SSH based tools**
    - [ClusterSSH](https://github.com/duncs/clusterssh): Cluster ssh can help you run a command in parallel on many hosts at once.
    - [Ansible](https://github.com/ansible/ansible): It allows you to write ansible playbooks which you can run on hundreds/thousands of hosts at the same time.
- **Agent Based tools**
    - [Saltstack](https://github.com/saltstack/salt): Is a configuration, state and remote execution framework, provides a wide variety of flexibility to users to execute modules on large numbers of hosts at once.
    - [Puppet](https://github.com/puppetlabs/puppet): Is an automated administrative engine for your Linux, Unix, and Windows systems, performs administrative tasks.

### Log analysis tools

These can help in writing SQL type queries for parsing, analysing logs and provide an easy UI interface to create dashboards which can render various types of charts based on defined queries.

- [ELK](https://www.elastic.co/what-is/elk-stack): Elasticsearch, Logstash and Kibana, provide package of tools and services to allow, parse logs, index logs and analyse logs easily and quickly. Once logs/data is parsed/filtered through logstash and indexed in elasticsearch, one can create dynamic dashboards in Kibana in a matter of minutes. Such provides easy analysis and correlation on application errors/exceptions/warnings.
- [Azure kusto](https://docs.microsoft.com/en-us/azure/data-explorer): Azure kusto is a cloud based service similar to Elasticsearch and Kibana, it allows easy indexing of heavy logs, provides SQL type interface for writing queries, and an interface to create dynamic dashboards.

