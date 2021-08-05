Performance tools are an important part of development/operations lifecycle, Its highly important for understanding application behavior. SRE generally uses such tools to evaluate how well service will perform and make/suggest improvements accordingly.

### Performance analysis commands

Most of these commands are a must to know for doing performance analysis of a system or service.

- top -: shows real-time view of running system, processes, threads etc.
- htop -: Similar to top command, but a bit more interactive then it.
- iotop -: An interactive disk I/O monitoring tool.
- vmstat -: Virtual memory statistics explorer.
- iostat -: Monitoring tool for input/output statistics for devices and partitions.
- free -: Tell info about physical memory and swap memory. 
- sar -: System activity report, reports diff metrics such as cpu, disk, mem, network, etc.
- mpstat -: Display info about CPU utilization and performance.
- lsof -: Provides info about the list of open files, opened by which processes.
- perf -: Performance analysing tool.

### Profiling tools

Profiling is an important part of performance analysis of the service. There are various profiler tools available, which can help figure most frequent code-paths, debugging, memory profiling, etc. These can generate the heatmap to understand the code performance when under load.

- [FlameGraph](https://github.com/brendangregg/FlameGraph): Flame graphs are a visualization of profiled software, allowing the most frequent code-paths to be identified quickly and accurately.
- [Valgrind](https://valgrind.org/info/about.html): It is a programming tool for memory debugging, memory leak detection, and profiling.
- [Gprof](https://sourceware.org/binutils/docs/gprof): GNU profiler tool uses a hybrid of instrumentation and sampling. Instrumentation is used to collect function call information, and sampling is used to gather runtime profiling information.

To know how LinkedIn performs On-Demand Profiling on its services, Read LinkedIn blog [ODP: An Infrastructure for On-Demand Service Profiling](https://engineering.linkedin.com/blog/2017/01/odp--an-infrastructure-for-on-demand-service-profiling)

### Benchmarking

It is a process of measuring the best performance of the service. Like how much QPS service can handle, its latency when load is increasing, host resource utilization, loadavg etc etc. The regression testing (i.e load testing) is a must before deploying the service to production.

**Some of known tools -:**

- [Apache Benchmark Tool, ab](https://httpd.apache.org/docs/2.4/programs/ab.html):, It simulate a high load on webapp and gather data for analysis
- [Httperf](https://github.com/httperf/httperf): It sends requests to the web server at a specified rate and gathers stats. Increase till one finds the saturation point.
- [Apache JMeter](https://github.com/apache/jmeter): It is a popular open-source tool to measure web application performance. JMeter is a java based application and not only a web server, but you can use it against PHP, Java, REST, etc.
- [Wrk](https://github.com/wg/wrk): It is another modern performance measurement tool to put a load on your web server and give you latency, request per second, transfer per second, etc. details.
- [Locust](https://github.com/locustio/locust): Easy to use, scriptable and scalable performance testing tool.

**Limitation -:**

Above tools help in synthetic load or stress testing, but such does not measure actual end user experience, It can’t see how end user resources will affect application performance, it is due to lack of memory, CPU, or poor connectivity to the internet.

To know how LinkedIn performs load testing across its fleet. Read : [Eliminating toil with fully automated load testing](https://engineering.linkedin.com/blog/2019/eliminating-toil-with-fully-automated-load-testing)

And to know how LinkedIn makes use of Real Time Monitoring (RUM) data to overcome the limitations of load testing, and help improve overall experience for end users. Read : [Monitor and Improve Web Performance Using RUM Data Visualization](https://engineering.linkedin.com/performance/monitor-and-improve-web-performance-using-rum-data-visualization)

### Scaling

System designed optimally can perform up to a certain limit only, based on availability of resources. Continuous optimization is always needed to ensure optimum use of resources at its peak. With increasing QPS, Systems need to scale up. We can either scale vertically or horizontally. Vertical scalability has its limits as one can increase cpu, memory, disk, GPU and other specifications to certain limit only, whereas horizontal scalability can grow easily and infinitely given limitations imposed by application design and environment attributes.

Scaling a web application will require some or all of the following -:

- Ease the server load by adding more hosts.
- Distributing the traffic across servers by using Load Balancers.
- Scale up DB by sharding the data and increasing read replicas.

Here’s a good read how LinkedIn scaled its application stack [A Brief History of Scaling LinkedIn](https://engineering.linkedin.com/architecture/brief-history-scaling-linkedin)
