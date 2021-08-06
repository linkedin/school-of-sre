## Caching static assets

Extending the existing caching solution a bit, we arrive at Content Delivery Networks(CDNs). CDNs are the caching layer that is closest to the user. A significant chunk of resources served in a webpage, may not be changing on an hourly or even a daily basis. In those cases, we would want to cache these at the CDN level, reducing our load. CDNs not only help reduce the load on our servers by removing the burden of serving static / bandwidth intensive resources, they also let us be present closer to our users, by way of points of presence(POPs). CDNs also let us do geo-load balancing, in case we have multiple data centres around
the world, and would want to serve from the closest data center (DC) possible.

**Taking it a step further**

With the addition of caching and distributing our application into simpler services, we have solved the problem of scaling to 50000 users. However, our users may be geographically distributed locations and may not be at the same distance from our data centre or our cloud region. Consistency in user experience is important, else we are excluding users who are far away from our location, potentially eliminating a significant chunk of potential users. However, it is not impractical to have data centers all over the world, or even in more than a couple of locations in the world. This is where CDNs and POPs come into picture.

## Points of Presence

CDN POPs are geographically distributed data centers aimed at being close to users. POPs reduce the round trip time by delivering content from a location that is nearest to the user. POPs typically may not have all the content, but have caching servers that cache the static assets, and fetch the rest of the content from the [origin server](https://www.cloudflare.com/en-in/learning/cdn/glossary/origin-server/) where the application actually resides. Their main function is to reduce round trip time by bringing the content closer to the website’s visitor. POPs can also route traffic to one of the multiple origin DCs possible. This way, POPs can be leveraged to add resiliency as well as load-balancing.


Now, with our image sharing application becoming more popular by the day, let us assume that we have hit 100,000 concurrent users. And we have built another data center, predicting this increase in traffic. Now we need to be able to route the service to both of these data centers in a reliable manner, while also retaining the ability to fall back to a single data center in case there is an issue with one of the two DCs. This is where sticky routing comes into play.

## Sticky Routing

When an user sends a request, there are cases in which we might want to serve a specific user’s requests from a DC if we have multiple DCs, or a specific server inside a DC. We may also wish to serve all requests from a specific POP by a single data center. Sticky routing helps us do exactly that. It might be simply pinning all users to a specific DC or pinning specific users to specific servers. This is typically done from the POP, so that as soon as the user enters reaches our servers, we can route them to the nearest DC possible.

## Geo DNS

When a user opens the application, the user can be directed to one of the multiple
globally distributed POPs. This can be done using [GeoDNS](https://jameshfisher.com/2017/02/08/how-does-geodns-work/), which simply put, gives out a different IP address(which are distributed geographically), depending on the location of the user making the DNS request. GeoDNS is the first step in distributing users to different locations - it is not 100% accurate, and typically makes use of IP address allotment information for guessing the location of the user. However, it works well enough for \>90% of the users. After this, we can have a sticky routing service that assigns each user to a specific DC, which we can use to assign a DC to this user, and set a cookie. When the user next visits, the cookie can be read at the POP to decide which data center the user’s traffic must be directed to.

Having multiple DCs and leveraging sticky routing has not only scaling benefits, but also adds to the resiliency of the service, albeit at the cost of additional complexity.

Let us consider another use case in which an user uploads a new profile picture for themselves. If we have multiple data centres or POPs which are not synced in real time - not all of them might have the newer picture. In such a case, it would make sense to tie that user to a specific DC/region until the update has propagated to all regions. Sticky routing would enable us to do this.


## References
1. [CDNs](https://www.cloudflare.com/en-in/learning/cdn/what-is-a-cdn/)
2. LinkedIn's TrafficShift [blog](https://engineering.linkedin.com/blog/2017/05/trafficshift--load-testing-at-scale) talks about sticky routing