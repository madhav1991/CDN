---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Using Cache Control to control an HTTP client's cache duration

## Introduction
When using a CDN, two levels of caching are available:
  * **Caching at the edge** occurs when a CDN edge server caches a piece of content from the origin.
  * **Caching downstream** from the edge network of servers occurs when an end-user or HTTP client, such as a requesting browser, caches a piece of content from an edge server.

The method you choose to control how long content is cached at the requester, such as a browser, depends on the following factors:
  * Whether the [Respect Header setting](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) is ON or OFF. By default it is set to ON.
  * Whether the origin server provides a `max-age` value in the Cache-Control header for a particular piece of content. 

Regardless of how these factors change, your origin must provide a Cache-Control header for the intended content to the edge, if you want edge servers to send HTTP responses with the Cache-Control header for that content.

Essentially, Cache-Control headers sent from an edge server downstream ask the requester to cache the associated content according to the caching directives or values specified by the edge server.

## Respect Header: Off
If your origin does provide a Cache-Control header with a `max-age` directive and value for a specific piece of content, then the cache duration for the specific piece of content cached on the edge is still derived from your CDN's TTL settings. Additionally, the edge server responds to the requester downstream with a Cache-Control `max-age` value equal to the lesser of:
  * The origin's Cache-Control `max-age` value.
  * The remaining time left until the content goes stale on the edge.

However, if your origin does not provide a Cache-Control header to the edge server, then edge servers will not provide a Cache-Control header to the requester. The edge cache duration for your content is still derived from your CDN's TTL Settings.

## Respect Header: On
If your origin does provide a Cache-Control header with `max-age` for a specific piece of content, the origin's Cache-Control `max-age` value becomes the cache duration for that specific piece of content cached on the edge, overriding any applicable TTL settings for that piece of content. Additionally, the edge responds to the requester with a Cache-Control `max-age` value equal to the remaining time left until the content goes stale on the edge server.

However, if your origin does not provide a Cache-Control header to the edge server, then the edge server will not provide a  Cache-Control header to the requester. The edge cache duration for your content is still derived from your CDN's TTL Settings.

## Summary

|Respect Header|Origin Provides Cache-Control|Specific Content's Cache Duration on Edge Server|Edge Server Provides Cache-Control|
|---|---|---|---|
|On|Yes, origin specifies a `max-age`|Edge cache duration overriden with Origin's `max-age` value|Yes, edge also provides a `max-age` with a value that is the (overridden) time before the edge needs to refresh the content from the origin|
|On|Yes, but origin does not specify a `max-age`|Edge cache duration based on the CDN's TTL configuration|Yes, edge also provides a `max-age` with a value that is the time before the edge needs to refresh the content from the origin|
|On|No|Edge cache duration based on the CDN's TTL configuration|No|
|Off|Yes, origin specifies a `max-age`|Edge cache duration base on the CDN's TTL configuration|Yes, edge also provides a `max-age` value that is the lesser of origin's `max-age` value and the time before the edge needs to refresh the content from the origin|
|Off|Yes, but origin does not specify a `max-age`|Edge cache duration based on the CDN's TTL configuration|Yes, edge also provides a `max-age` with a that is the time before the edge needs to refresh the content from the origin|
|Off|No|Edge cache duration based on the CDN's TTL configuration|No|

## More Information
* How to [manage your CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Cache-Control as defined in section 14.9 of [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
