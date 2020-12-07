# Conclusion

## Scaling The App

The design and development is just a part of the journey. We will need to setup continuous integration and continuous delivery pipelines sooner or later. And we have to deploy this app somewhere.

Initially we can start with deploying this app on one virtual machine on any cloud provider. But this is a `Single point of failure` which is something we never allow as an SRE (or even as an engineer). So an improvement here can be having multiple instances of applications deployed behind a load balancer. This certainly prevents problems of one machine going down.

Scaling here would mean adding more instances behind the load balancer. But this is scalable upto only a certain point. After that, other bottlenecks in the system will start appearing. ie: DB will become the bottleneck, or perhaps the load balancer itself. How do you know what is the bottleneck? You need to have observability into each aspects of the application architecture.

Only after you have metrics, you will be able to know what is going wrong where. **What gets measured, gets fixed!**

Get deeper insights into scaling from School Of SRE's [Scalability module](../systems_design/scalability.md) and post going through it, apply your learnings and takeaways to this app. Think how will we make this app geographically distributed and highly available and scalable.

## Monitoring Strategy

Once we have our application deployed. It will be working ok. But not forever. Reliability is in the title of our job and we make systems reliable by making the design in a certain way. But things still will go down. Machines will fail. Disks will behave weirdly. Buggy code will get pushed to production. And all these possible scenarios will make the system less reliable. So what do we do? **We monitor!**

We keep an eye on the system's health and if anything is not going as expected, we want ourselves to get alerted.

Now let's think in terms of the given url shortening app. We need to monitor it. And we would want to get notified in case something goes wrong. But we first need to decide what is that _something_ that we want to keep an eye on.

1. Since it's a web app serving HTTP requests, we want to keep an eye on HTTP Status codes and latencies
2. Request volume again is a good candidate, if the app is receiving an unusual amount of traffic, something might be off.
3. We also want to keep an eye on the database so depending on the database solution chosen. Query times, volumes, disk usage etc.
4. Finally, there also needs to be some external monitoring which runs periodic tests from devices outside of your data centers. This emulates customers and ensures that from customer point of view, the system is working as expected.

## Applications in SRE role

In the world of SRE, python is a widely used language. For small scripts and tooling developed for various purposes. Since tooling developed by SRE works with critical pieces of infrastructure and has great power (to bring things down), it is important to know what you are doing while using a programming language and its features. Also it is equally important to know the language and its characteristics while debugging the issues. As an SRE having a deeper understanding of python language, it has helped me a lot to debug very sneaky bugs and be generally more aware and informed while making certain design decisions.

While developing tools may or may not be part of SRE job, supporting tools or services is more likely to be a daily duty. Building an application or tool is just a small part of productionization. While there is certainly that goes in the design of the application itself to make it more robust, as an SRE you are responsible for its reliability and stability once it is deployed and running. And to ensure that, you’d need to understand the application first and then come up with a strategy to monitor it properly and be prepared for various failure scenarios.

## Optional Exercises

1. Make a decorator that will cache function return values depending on input parameters.
2. Host the URL shortening app on any cloud provider.
3. Setup monitoring using many of the tools available like catchpoint, datadog etc.
4. Create a minimal flask-like framework on top of TCP sockets.

## Conclusion

This module, in the first part, aims to make you more aware of the things that will happen when you choose python as your programming language and what happens when you run a python program. With the knowledge of how python handles things internally as objects, lot of seemingly magic things in python will start to make more sense.

The second part will first explain how a framework like flask works using the existing knowledge of protocols like TCP and HTTP. It then touches the whole lifecycle of an application development lifecycle including the SRE parts of it. While the design and areas in architecture considered will not be exhaustive, it will give a good overview of things that are also important being an SRE and why they are important.
