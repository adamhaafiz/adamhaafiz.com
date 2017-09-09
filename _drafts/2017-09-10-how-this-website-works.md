---
layout:     post
summary:    The fact that you're reading this now means that I've set up this website correctly. Hooray!
title:      How This Website Works
---

## Success!

The fact that you're reading this now means that I've set up this website correctly. Hooray! However, in the process of getting it set up I found myself blindly following guides without really knowing that I was doing. I figured that writing down how the website works would be a good way to educate myself as well as anyone else who was curious about this sort of thing.

## What You're Reading

All the content that you see on this website is hosted on [GitHub Pages](https://pages.github.com). The posts and pages are written in [Markdown](https://daringfireball.net/projects/markdown/), which is then transformed into a static website through [Jekyll](https://jekyllrb.com). 

Jekyll is basically a content management system without a database. Think of it as a super lightweight version of WordPress where all your posts and pages are Markdown & HTML files. You don't go through the `wp-admin` UI to create new posts, instead you just open up whatever text editor you have and start writing it there. 

GitHub Pages offers hosting for static websites entirely for free (thank you GitHub!). So when you combine this with a static website generator like Jekyll, it means that you can have blog-like websites hosted on GitHub Pages without spending a cent!

Using the GitHub Pages + Jekyll combo allows you to have a website for free, but you're limited to `[WEBSITE NAME].github.io` domains. If you look at your browser's address bar, you'll notice that I'm using my own domain instead. I purchased it from the fine folks over at [Namecheap](https://www.namecheap.com).

## How You Got Here

Hooking up your own custom domain to your GitHub Pages website is [well documented](https://help.github.com/articles/using-a-custom-domain-with-github-pages/). The guide tells you _what_ you need to do, but it doesn't really tell you _why_ these things need to be done.

The first thing GitHub asks you to do is to [add a custom domain to your GitHub Pages website](https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/). Doing this step ensures that any visitors to your GitHub Pages URL gets redirected to your custom domain instead. Try it out: [adamhaafiz.github.io](http://adamhaafiz.github.io)

The next step is to point your custom domain to your GitHub Pages website. This is the point where I realised that the URL addresses `adamhaafiz.com` and `www.adamhaafiz.com` are **not** the same thing!

### The Domain Decision

A domain such as `adamhaafiz.com` is called an apex domain (also called a naked or root domain).[^1] An address like `www.adamhaafiz.com` uses the `www` subdomain, just like how `blog.adamhaafiz.com` uses the `blog` subdomain. Writing this down made me feel dumb about not knowing that `www` was considered a subdomain too.. but hey, I'm glad I learnt about it!

With that new information, I decided that I wanted to use `adamhaafiz.com`,[^2] with all requests going to `www.adamhaafiz.com` subdomain redirecting to the apex domain instead.

## Dabbling with DNS

At this stage, there were two things that I wanted to achieve:

1. Point my custom `adamhaafiz.com` domain to my GitHub Pages website
2. Redirect all traffic from the `www` subdomain to the apex domain

In order for these two things to happen, we're gonna have to play around with DNS. **D**omain **N**ame **S**ervers are basically the address books of the internet. When you visit a URL such as `www.google.com`, your browser asks the DNS server (yes, [RAS syndrome](https://en.wikipedia.org/wiki/RAS_syndrome)) to resolve the URL into an IP address.

### Pointing in the Right Direction

Most domain registrars allow you to modify the DNS records for the domains that your purchased through them. This way, we can add the necessary DNS records for what we want to achieve. For this case, what we need specifically is a `CNAME` record. 

`CNAME` stands for Canonical Name. This record can be used to provide an alias for a domain, e.g. making requests to `blog.adamhaafiz.com` redirect to `www.adamhaafiz.com`. There's one big caveat though: `CNAME` records **do not work** on apex domains!

This is where `ALIAS` (or `ANAME`) records come in. This is a kind of pseudo-record that allows pointing of apex domains to other domains. It's a pseudo-record because most DNS providers manually map these records to `A` records underneath. `A` records (**A**ddress records) are the most basic form of DNS records, as they simple map any domain to an IP address.

Unfortunately, my domain registrar Namecheap does not support `ALIAS` records. So in the end I had to set up `A` records to point my domain to the GitHub servers. The only disadvantage is that in the rare case that GitHub decides to change the IP address, I will have to manually update my records to point to the new address. 

### Redirecting Traffic

Setting up the redirect from the `www` subdomain to the apex domain is simply a matter of addding a new CNAME record.  Try it out now: [www.adamhaafiz.com](http://www.adamhaafiz.com). You'll notice that your browser gets automatically redirected to the apex domain.

#### "Hey, didn't you say earlier that CNAME records don't work with apex domains?"

You can add CNAME records to point a subdomain **to** any other URL (including apex domains), but you can't use them to point **from** an apex domain.

Before we wrap up, remember that earlier step about adding a custom domain to your GitHub pages wesite? This step actually generates a [CNAME file](https://github.com/adamhaafiz/adamhaafiz.github.io/blob/master/CNAME) and commits it into your repository. I haven't been able to find out what exactly this CNAME file does, but I *think* GitHub uses it to know which domain to point to when someone visits the GitHUb Pages URL directly.

## Next Step: HTTPS

The security conscious amongst you may have noticed that this website does not support HTTPs yet. Fret not! By the time you visit this website again (in other words by my next blog post), I hope to have HTTPS set up. Stay tuned!

I hope this post was helpful. If you've got any questions or would like to get in touch with me, [feel free to do so](/contact/). 

---

[^1]: I'd love to know the _proper_ technical term for this. Please [get in touch with me](/contact/) if you know! 
[^2]: Yes, I'm aware of some of the [critcism surrounding this choice](http://www.yes-www.org/why-use-www/).
