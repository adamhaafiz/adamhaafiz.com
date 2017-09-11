---
layout:     post
summary:    The fact that you're reading this now means that I've set up this website correctly. Hooray!
title:      Welcome HTTPS!
---

- GitHub Pages already offers HTTPS, but only for .github.io domains
- Cloudflare offers HTTPS for custom domains through their Universal SSL
    - What the hell is a universal SSL cert?

1. Add domain to Cloudflare
2. Change nameservers on domain registrar to point to Cloudflare nameservers
3. Add a CNAME record for both the apex domain and the www subdomain to point to the GitHub site
4. Go to the crypto tab and turn on full SSL
5. Add page rule for apex domain to Always Use HTTPS
6. Add another page rule to redirect https://www to apex securely
7. What about http://www ? That's covered by the CNAME record I think
8. Turn on HSTS
9. Cache everything
10. Done!

- What is a nameserver?
