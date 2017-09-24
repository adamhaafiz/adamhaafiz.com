---
layout:     post
summary:    You are now accessing this website securely thanks to HTTPS.
title:      Hello HTTPS
---

You are now accessing this website securely thanks to HTTPS. 

![welcome-https](/images/welcome-https.png){: .centre-image}
*Look at those pretty lock icons!*

Out of the box GitHub Pages already supports HTTPS for sites using `[WEBSITE NAME].github.io` domains. However, for sites on custom domains this is not possible. Thankfully, this is where Cloudflare comes in with their awesome [Universal SSL](https://blog.cloudflare.com/introducing-universal-ssl/) service that they provide.

## Hello Cloudflare

Cloudflare is a company that offers a multitude of services ranging from content delivery networks, load balancing, DDoS protection, visitor analytics and of course SSL.

![cloudflare-full-ssl](https://www.cloudflare.com/img/products/ssl/full-ssl-strict.svg){: .centre-image}
*Image taken from [Cloudflare](https://www.cloudflare.com/ssl/)*

So how does Cloudflare fit into the picture? Cloudflare acts as a reverse proxy for the website. What that means is that instead of your browser accessing GitHub Pages directly (Origin Server in the above photo), your browser speaks to Cloudflare instead. Cloudflare *then* goes and fetches the data from GitHub Pages before returning it to you. 

#### "Won't that make the website load slower?"

As a matter of fact, it should have the exact _opposite_ effect. By taking advtange of their CDN, Cloudfare can return a cached version of this entire website (hooray for static websites!) to your browser. Not only that, but Cloudflare has [a whole bunch of tricks up its sleeve](https://www.cloudflare.com/performance/) to speed up the entire process.[^1]

#### "How does Cloudflare help with SSL?"

The moment I add my domain to my Cloudflare account, it automatically [provisions an SSL certificate for that domain](https://security.stackexchange.com/a/101523). This certificate is the one that your browser will be using when it connects to this domain. From there Cloudflare can serve this website to your browser over HTTPS.

Since Cloudflare sits in between your browser and the origin server, your browser doesn't even need to know about the existence of GitHub Pages. That's entirely Cloudflare's problem, not your browser's.

## Setting Everything Up

Since Cloudflare works as a reverse proxy, the first order of business is to ensure that visitors go through Cloudflare instead of accessing the origin server directly. This can be done by changing the nameservers on my domain to point to Cloudflare's nameservers instead of the default ones provided by the domain registrar.

A nameserver is the server that responds to DNS record requests from web browsers. This way, whenever a browser requests the DNS records for `adamhaafiz.com`, Cloudflare will be the one answering these requests rather than Namecheap.

From there Cloudflare can do whatever magic it needs to do. It also means that all of the DNS records I set up before will now be configured through the Cloudflare dashboard.

### DNS Changes

![dns-records](/images/dns-records.png){: .centre-image}

[In my previous post]({% link _posts/2017-09-10-how-this-website-works.md %}), I commented about how Namecheap didn't support `ALIAS` records. Luckily, Cloudflare does. However, Cloudflare calls it [CNAME Flattening](https://support.cloudflare.com/hc/en-us/articles/200169056-CNAME-Flattening-RFC-compliant-support-for-CNAME-at-the-root). Since this kind of DNS record is not _officially_ supported, it serves as a reminder that different service providers will have different names for it.

This allowed me to drop the ugly `A` records I had and replace them with 1 flattened `CNAME` record that points the apex domain to `adamhaafiz.github.io` (and of course another `CNAME` record for the `www` subdomain). 

### Turning on SSL

![crypto](/images/crypto.png){: .centre-image}

The next step was to go into the Crypto tab on the Cloudflare dashboard and turn on Full SSL. This setting tells Cloudflare how it should connect to the origin server. Setting it to Full tells Cloudflare to connect to GitHub Pages using HTTPS and not HTTP.

In the same page I also turned on the setting for "Always use HTTPS". This makes is so that any requests using `http` will be responded with a 301 redirect to the equivalent `https` page. 

![page-rules](/images/page-rules.png){: .centre-image}

Afterwards I added a page rule so that all requests going to the `www` subdomain be redirected to the `https` root domain. This was done so that all 4 addresses to the website would all redirect to the same one. For instance, all of the following domains: 

- [http://adamhaafiz.com](http://adamhaafiz.com) (handled by previous "Always use HTTPS" setting)
- [http://www.adamhaafiz.com](http://www.adamhaafiz.com)
- [https://www.adamhaafiz.com](https://www.adamhaafiz.com)

..should redirect to the *one true domain*:

- [https://adamhaafiz.com](https://adamhaafiz.com)

You can test them out yourself by clicking on any one of the above links. All of them will redirect you to the root domain served over HTTPS. If you're curious, you can even poke around and check out what kind of certificate I got from Cloudflare:

![ssl-cert](/images/ssl-cert.png){: .centre-image}

It's a COMODO issued Domain Validated SSL certificate. If you check under "Details" you will also be able to see all the other domains that are using the same certificate. 

And that's it! If you've got any questions or would like to get in touch with me, [feel free to do so](/contact/). 

---

[^1]: I'll admit that I don't understand half of the things on that page. But I trust them 100% that they know what they're doing.