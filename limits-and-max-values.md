---

copyright:
  years: 2018
lastupdated: "2018-10-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limits and Maximum Values

## Is there a maximum value for Time To Live? A minimum?

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 0 seconds.

## Is there a limit on the number of Origin and TTL entries?

Yes, the combined limit is 75 entries per CDN.

## What is the largest file size that can be delivered via Akamai CDN?

Attempts to retrieve or deliver files larger than 1.8GB will receive a `403 Access Forbidden` response for the default performance configuration. If Large File Optimization is enabled, file downloads up to 320GB are possible.
