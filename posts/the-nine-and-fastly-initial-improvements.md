---
title: The Nine and Fastly - Initial Improvements
description: The Nine switched from Cloudflare to Fastly. The results surprised us (in a good way).
layout: layouts/post.njk
---

The Nine recently switched our main site ([nine.is](https://nine.is)) over to Fastly, our new CDN partner. After our initial work with Fastly, we noticed amazing improvements in speed.

## Why do we need a CDN?

Caboose, our CMS, has served us well for a while, but it's been showing its age. Over the last several years, we've seen our time to first byte (TTFB) steadily increasing as we onboarded more and more customers. All our sites are hosted on the same hosting infrastructure (we have multiple servers, but all can serve any site). As we added more sites to our network, the server response time increased significantly. At its peak, we'd see initial delays of up to 2 seconds.

The job of a CDN is to **reduce the number of requests hitting your server**. CDNs like Fastly cache pages as they serve them (meaning they store a copy of the page on their server). After the initial request, if another request comes in for the same page, Fastly will serve the cached copy. Fastly serves the copy until we tell them to stop. This is called **purging the cache**.

A 'cache hit' is a request that's served from the CDN cache without it reaching our servers. A 'cache hit ratio' is the total number of cache hits divided by the total number of requests. The higher the cache hit ratio, the better.

If a cache hit ratio is higher, that means a fewer number of requests are hitting application servers. If the cache hit ratio is 80%, that means 80% of requests aren't hitting our servers. Conversely, 20% of requests *are* hitting our servers. We needed a CDN because it would speed up our sites, but also because it would reduce the number of requests we had to deal with. This would save us significantly in server costs.

## Why Fastly?

We initially brought Fastly onboard for a separate internal project, but after exploring, we decided that it'd be a good idea to switch from our current CDN provider, CloudFlare. CloudFlare had a generous free tier with unlimited bandwidth, but its caching left much to be desired. Our cache hit ratio was about 25% on average across all our sites.

Fastly had a more impressive cache invalidation API and offered much more granular customization than CloudFlare gave us. Their sales team was much more responsive and hands-on, and the onboarding process was amazing. Everything, from the console to the team we worked with, was impressive and best-in-class.

## Moving over our nine.is site

After working with Fastly's onboarding team, we decided our best course of action would be to move over one of our sites to their platform to see how it would fare. After internal discussion, we decided to move over our own site. We spent a few weeks working with the config options, trying to tune the cache to our site and hosting platform. After seeing promising results, we decided to release our initial version and take the site live this week.

Moving our site over was a breeze. Configuring TLS, DNS, and the rest was amazingly simple thanks to Fastly's intuitive UI. We moved over the DNS in under an hour and had the new version of our site live quickly. 

## Results:

Our cache hit ratios are incredibly high. 80% of all served requests are cached, which is ridiculous:

![Two graphs from the Fastly dashboard: the cache hit ratio graph, and the cache coverage graph. The cache hit ratio graph shows an average of 80.21% cache hit ratio. The cache coverage graph shows on average 97.92% coverage.](https://publish-01.obsidian.md/access/fb80b5478f2116d805b6dbe13b776532/Images/Pasted%20image%2020210319151436.png)

Page load times have decreased by a good bit. This table shows the average time it takes for a request to be served from Fastly over an hour timeframe. `MISS` indicates that there was not a cache hit (meaning the request hit our servers), and `HIT` indicates that Fastly served the request.

| Average Duration (ms) | Cache Status | Number of requests |
|:--|:--|:--|
| 2.030109  | HIT | 2547 |
| 353.244798 | MISS | 1565 |
| 274.225670 | PASS | 188 |
| 1.089789 | SYNTH | 380 |

When a request was sent to our servers, it took on average 300-ish milliseconds for it to resolve. When a request was sent from Fastly, it took **2 milliseconds** to resolve. That's about a 99% decrease.

We saw drastically faster page load times, but not as much of an improvement in our PageSpeed and Lighthouse scores. Because Lighthouse and PageSpeed measure *user experience* (not only how fast it takes for something to load), not all the metrics they measured were impacted by the increase in speed Fastly gave us. Increasing the speed of our site will require being more frugal with how we include Javascript, making smart decisions about inlining critical CSS, and doing other more technical things to improve user experience for our websites.

Fastly was the perfect choice for us. We love it so far, and we can't wait to migrate more of our customers' sites to it.