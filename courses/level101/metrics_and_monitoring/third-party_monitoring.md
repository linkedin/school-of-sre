##

# Third-party monitoring

Today most cloud providers offer a variety of monitoring solutions. In
addition, a number of companies such as
[Datadog](https://www.datadoghq.com/) offer
monitoring-as-a-service. In this section, we are not covering
monitoring-as-a-service in depth.

In recent years, more and more people have access to the internet. Many
services are offered online to cater to the increasing user base. As a
result, web pages are becoming larger, with increased client-side
scripts. Users want these services to be fast and error-free. From the
service point of view, when the response body is composed, an HTTP 200
OK response is sent, and everything looks okay. But there might be
errors during transmission or on the client side. As previously
mentioned, monitoring services from within the service infrastructure
give good visibility into service health, but this is not enough. You
need to monitor user experience, specifically the availability of
services for clients. A number of third-party services such asf
[Catchpoint](https://www.catchpoint.com/),
[Pingdom](https://www.pingdom.com/), and so on are available for
achieving this goal.

Third-party monitoring services can generate synthetic traffic
simulating user requests from various parts of the world, to ensure the
service is globally accessible. Other third-party monitoring solutions
for real user monitoring (RUM) provide performance statistics such as
service uptime and response time, from different geographical locations.
This allows you to monitor the user experience from these locations,
which might have different internet backbones, different operating
systems, and different browsers and browser versions. [Catchpoint
Global Monitoring
Network](https://pages.catchpoint.com/overview-video) is a
comprehensive 3-minute video that explains the importance of monitoring
the client experience.