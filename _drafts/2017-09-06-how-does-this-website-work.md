---
layout:     post
summary:    The fact that you're reading this now means that I've set up this website correctly. Hooray!
title:      How This Website Works
---

## Success!

The fact that you're reading this now means that I've set up this website correctly. Hooray! However, in the process of getting it set up I found myself blindly following guides without really knowing that I was doing. I figured that the best course of action was to write about how the website works as a way of educating myself as well as anyone else who is curious about this sort of thing.

## What You're Reading

All the content that you see on this website is hosted on [GitHub Pages](https://pages.github.com). The posts and pages are written in [Markdown](https://daringfireball.net/projects/markdown/), which is then transformed into a static website through [Jekyll](https://jekyllrb.com). 

Jekyll is basically a content management system without a database. Think of it as a super lightweight version of WordPress where all your posts and pages are Markdown & HTML files. You don't go through the `wp-admin` UI to create new posts, instead you just open up whatever text editor you have and start writing it there. 

GitHub Pages offers hosting for static websites entirely for free (thank you GitHub!). So when you combine this with a static website generator like Jekyll, it means that you can have blog-like websites hosted on GitHub Pages without spending a cent!

Using the GitHub Pages + Jekyll combo allows you to have a website for free, but you're limited to `[WEBSITE NAME].github.io` domains. If you look at your browser's address bar, you'll notice that I'm using my own domain instead. I purchased it from the fine folks over at [Namecheap](https://www.namecheap.com).

## How You Got Here

Hooking up your own custom domain to your GitHub Pages website is [well documented](https://help.github.com/articles/using-a-custom-domain-with-github-pages/). The guide tells you _what_ you need to do, but it doesn't really tell you _why_ these things need to be done.

The first thing GitHub asks you to do is to [add a custom domain to your GitHub Pages website](https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/). This step helps you to generate a [CNAME file](https://github.com/adamhaafiz/adamhaafiz.com/blob/master/CNAME) and commit it into your repository. "What's a CNAME file?" you might be asking. Good question, I'll revisit it later.

The next step is to point your custom domain to your GitHub Pages website. This is the point where I realised that the addresses `adamhaafiz.com` and `www.adamhaafiz.com` are **not** the same thing!

### The Domain Decision

A domain such as `adamhaafiz.com` is called an apex domain (also called naked domain). An address like `www.adamhaafiz.com` is a subdomain, just like how `blog.adamhaafiz.com` uses the `blog` subdomain. Then the next question was: which one should I be using?

I decided that I wanted to use `adamhaafiz.com`,[^1] with all requests going to `www.adamhaafiz.com` subdomain redirecting to the apex domain instead. Try it out now: [www.adamhaafiz.com](http://www.adamhaafiz.com). You'll notice that your browser gets automatically redirected to the apex domain.

## Deciphering DNS

At this stage, there are actually two things that we want to achieve:

1. Point our custom `adamhaafiz.com` domain to our GitHub Pages website
2. Redirect all traffic from the `www` subdomain to the apex domain

### Pointing in the Right Direction

The way that we point domains to other domains/IP addresses is through DNS records. For this case, what we need specifically is a CNAME record. What does a CNAME record do?

CNAME stands for Canonical Name. This record can be used to provide an alias for a domain, e.g. making requests to `blog.adamhaafiz.com` redirect to `www.adamhaafiz.com`. There's one big caveat though: CNAME records **do not work** on apex domains!



### Redirecting Traffic


---

[^1]: Yes, I'm aware of some of the [critcism surrounding this choice](http://www.yes-www.org/why-use-www/)
