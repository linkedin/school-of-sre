A resilient system is one that can keep functioning in the face of
adversity. With our application, there can be numerous failures that act
as adversities. There can be network level failures that take out entire
data centres, there might be issues at the rack level or at the server
level, or there might be something wrong with the cloud provider. We may also run out of capacity, or there might be a wrong code push that
breaks the system. We will talk about a couple of such issues, and
understand how we might design a system to work around such things. In
some cases, a workaround might not be possible. However it is still
valuable to know potential vulnerabilities to the system stability.

Resilient architectures leverage system design patterns such as
graceful degradation, quotas, timeouts and circuit breakers. Let us look
at some of them in this section.

## Quotas

A system may have a component or an endpoint that is consumed by
multiple components and endpoints. It is important to have something in
place that will prevent one consumer or client from overwhelming such a
system. Quotas are one way to do this - we simply assign a specific
quota for each component - by way of specifying requests per unit time.
Anyone who breaches the quota is either warned or dropped, depending on
the implementation. This way, one of our own systems misbehaving cannot
result in denial of service to others. Quotas also help us prevent cascading failures.

## Graceful Degradation

When a system with multiple dependencies encounters failure in one of
the dependencies, gracefully degrading to minimum viable functionality
would be a lot better than grinding the entire system to a halt. For
example, let us assume there is an endpoint (an URL for a service or a specific function) in our application whose responsibility is to parse the location information in an user uploaded
image from the image's metadata and provide suggestions for location
tagging to the user. Rather than failing the entire upload, it is much
better to skip over this functionality and still give the user an option
to manually tag a location. Gracefully degrading is always better
compared to total failures.

## Timeouts

We sometimes call other services or resources like databases or API endpoints in our application. When calling such a resource from our application, it is important to always have a reasonable timeout. It doesn’t necessarily even have to be that the resource will fail for all requests. It just might be that a specific request falls in the high tail latency category. A reasonable time out is helpful to keep the user experience consistent - it is better to fail rather than to have frustratingly long delays, in some cases.

## Exponential back-offs

When a service endpoint fails, retries are one way to see if it was a momentary failure. However, if the retry is also going to fail, there is no point in endlessly retrying. At large enough scale, the retries can compete with the new requests (which might very well be served as expected) and saturate the system. To avoid this, we can look at exponential back-off for retries. This essentially decreases the rate at which the clients retry, upon encountering consecutive failures on retries.

## Circuit breakers

While exponential back off is one way to deal with retry storms, circuit breakers can be another. Circuit breakers can help failures from percolating the entire system. Else, an unmitigated failure that flows through the system may result in false alerts, worsening the mean time to detection(MTTD) and mean time to resolution(MTTR). For example, in case one of the in-memory cache nodes fails resulting in requests reaching the database post the initial timeouts for cache, it might end up overloading the database. If the initial connection between cache node failure and DB node failure is not made, then it might result in increased MTTD of the actual cause and consequently the MTTR.

## Self healing systems

A traditionally load-balanced application with multiple instances might fail when more than a threshold of instances stop responding to requests - either because they are down, or suddenly there is a huge influx of requests, resulting in degraded performance. A self-healing system adds more instances in this scenario to replace the failed instances.
Auto-scaling like this can also help when there is a sudden spike in query. If our application runs on a public cloud, it might simply be a matter of spinning up more [virtual machines](https://azure.microsoft.com/en-in/overview/what-is-a-virtual-machine/). If we are running on-premise out of our data center, then we will want to think about capacity planning much more carefully. Regardless of how we handle adding additional
capacity - simply addition may not be enough. We should also think about additional potential failure modes that might be encountered. For example, the load balancing layer itself might need scaling up, to handle the influx of new backends.

## Continuous Deployment and Integration

A well designed system also needs to take into account the need for a proper staging setup that can mimic the production environment as closely as possible. There should also be a way for us to replay production traffic in the staging environment to test changes to production thoroughly.