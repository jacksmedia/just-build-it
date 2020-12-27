---
templateKey: blog-post
title: https on netlify also w AWS
date: 2020-12-26T02:33:01.032Z
description: it's the 2020s and you need a secure website
featuredpost: true
featuredimage: /img/new-lincoln-high-school.jpg
tags:
  - ssl
  - certificates
  - https
  - netlify
  - aws
---
Of course you bought your domain on AWS, and of course you want to launch betas on that domain using Netlify's awesome pipeline. Comrades, it's not difficult but it's not as easy as it oughta be!

For me, the biggest trick is to see thru the confusing labels on Netlify's panel, but ymmv--

### _basically ensure it sez the DNS is NOT set up w Netlify!!_

Since your domain is registered on AWS, you can use 2 core A.W. Services (AWS Certificate Manager & Route 53) for the records, not Netlify.

In Route 53:
- Create a NameServer record with 4 addresses of Netlify's servers 
- Create an A Record that links your site's IP with it's domain name
- Create a CNAME Record to also link the www subdomain to the A Record

In CertMgr:
- Switch your Region to us-east-1a before your create the SSL cert.
- Choose options to create a new SSL Certificate for your domain
- Route 53 can usually handshake and create the records to smooth over the last step here, which is authenticating that You actually own the domain... if it cannot create records for you automatically, see this post _{custom DNS records in AWS Route 53}_

When you set up these 2 AWS's, the changes will happen quickly. Expect the DNS to take under an hour, and the certificate to guarantee HTTPS (that SSL cert you asked for) will issue in under 24 hrs. Refresh Netlify's panel to see your site's custom domain and SSL progress, and keep building!