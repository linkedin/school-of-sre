# System troubleshooting and performance improvements

## Prerequisites

* [Linux Basics](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/)
* [System design](https://linkedin.github.io/school-of-sre/level101/systems_design/intro/)
* [Basic Networking](https://linkedin.github.io/school-of-sre/level101/linux_networking/intro/)
* [Metrics and Monitoring](https://linkedin.github.io/school-of-sre/level101/metrics_and_monitoring/introduction/)

## What to expect from this course

This brief course tries to provide a general introduction on how to troubleshoot system issues, like analysing api failures,
resource utilization, network issues, hardware and OS issues. Course also briefs on profiling and benchmarking to measure overall system performance.

## What is not covered under this course

This course does not cover following -:

* System Design and Architecture.
* Programming practices.
* Metrics and Monitoring.
* OS basics.

## Course Contents
- [Introduction](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/introduction)
- [Troubleshooting](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting)
    - [Troubleshooting Flowchart](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#troubleshooting-flowchart)
    - [General Practices](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#general-practices)
    - [General Host issues](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#general-host-issues)
- [Important tools to know](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools)
    - [Important linux commands](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools/#important-linux-commands)
    - [Log analysis tools](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools/#log-analysis-tools)
- [Performance improvements](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements)
    - [Performance analysis commands](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#performance-analysis-commands)
    - [Profiling tools](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#profiling-tools)
    - [Benchmarking](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#benchmarking)
    - [Scaling](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#scaling)
- [Troubleshooting Example](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting-example)
- [Conclusion](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/conclusion)
    - [Further readings](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/conclusion/#further-readings)

## Introduction
Troubleshooting is an important part of operations & development. It can’t be learned by just reading one article or completing a course online, 
Its a continuous learning process, one learns it during :-

* Daily operations and development.
* Finding & Fixing application bugs.
* Finding & Fixing system & network issues.
* Performance analysis and improvements.
* And more.

From an SRE’s perspective, It is expected that they are aware of certain topics upfront to be able to troubleshoot problems around single or distributed systems.

* Know your resources well, understand host specifications, liks CPU, Memory, Network, Disk etc.
* Understand system design and architecture.
* Ensure important metrics are being collected/rendered properly.

There was a famous quote by HP founders - **“What gets measured gets fixed”**

If system components and performance metrics are captured thoroughly then there is a high chance of success in troubleshooting an issue, at its earliest.

### Scope
There is no common approach to troubleshoot different types of applications or services, the failure can occur at any layer of it. We will keep the scope of this work to a web api service type only.

**Note -:** Linux ecosystem is wide, there are hundreds of tools and utilities which can help with system troubleshooting, each comes with its own set of benefits and functionalities. We will cover some of the known tools, either already available with Linux or are available in the open source world. Detailed explanation of mentioned tools in this doc is out of scope, please explore the internet or man pages for more examples and documentation around the same.

