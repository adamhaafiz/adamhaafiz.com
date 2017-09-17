---
layout:     post
summary:    The fact that you're reading this now means that I've set up this website correctly. Hooray!
title:      Welcome HTTPS!
---

You are now accessing this website securely thanks to HTTPS. 

![welcome-https](/images/welcome-https.png){: .centre-image}
*Look at those pretty lock icons!*

Out of the box GitHub Pages already supports HTTPS for sites using `[WEBSITE NAME].github.io` domains. However, as you probably can tell this website uses a custom domain. Hence we do not get the option of having HTTPS support from GitHub Pages.

## Cloudflare to the Rescue

One of the services that Cloudflare offers is called [Universal SSL](https://blog.cloudflare.com/introducing-universal-ssl/). Originally announced in September 2014, Universal SSL was a plan to get everyone on a Cloudflare plan to enjoy the benefits of HTTPS, regardless of whether they were on a paid plan or a **free** plan.

I've since taken full advantage of Cloudflare's free plan by adding my domain to my Cloudflare account. Some other behinds the scenes changes had to be made as well, which is what I'll be documenting in this post.

But for you, the reader, nothing should be any different. As a matter of fact, apart from an upgrade to HTTPS this website should actually feel like it loads faster than before.

## Behind the Scenes

[In my previous post]({% link _posts/2017-09-10-how-this-website-works.md %}) I had already laid out the underworkings of this webiste and how everything was put together. It mostly covered setting up the DNS to point computers in the right direction when they access the website.

Cloudflare acts as a reverse proxy for the website. What that means is that instead of your browser accessing GitHub Pages directly, your browser speaks to Cloudflare instead. Cloudflare *then* goes and fetches the data from GitHub Pages before returning it to you.

- GitHub Pages already offers HTTPS, but only for .github.io domains
- Cloudflare offers HTTPS for custom domains through their Universal SSL
    - What the hell is a universal SSL cert?

1. Add domain to Cloudflare
2. Change nameservers on domain registrar to point to Cloudflare nameservers
3. Add a CNAME record for both the apex domain and the www subdomain to point to the GitHub site (say bye bye to those nasty A records! CName Flatting is basically same as the ALIAS records mentioned before.. seems like everyone has their own name for it)
4. Go to the crypto tab and turn on full SSL
5. On same page turn on Always Use HTTPS
6. Add page rule to redirect any www to https://apex
9. Cache everything
10. Done!

- What is a nameserver?
