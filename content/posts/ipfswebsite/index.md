---
title: "Introduction"
date: 2020-06-08T08:06:25+06:00
description: Creating an IPFS site
menu:
  sidebar:
    name: Creating This website on IPFS
    identifier: ipfs
    weight: 10
tags: ["Basic", "Multi-lingual"]
categories: ["Basic"]
---


## Creating this site and putting it on IPFS

TLDR: If your like  me and learn from reading code over guides. Here is the repo [https://github.com/derage/derage.github.io](https://github.com/derage/derage.github.io). You can checkout how to [deploy it here](https://github.com/derage/derage.github.io/blob/source/.github/workflows/deploy-gh-site.yaml)

I present to you a partially distributed website running on IPFS and using cloudflare to cache the website and DNS. If you are using a brave browser or chrome with the IPFS extension installed, you can go to [ipfs://QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ](ipfs://QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ). If you are on a traditional browser, you can check out the website pinned to pinata at [https://gateway.pinata.cloud/ipfs/QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ/](https://gateway.pinata.cloud/ipfs/QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ/). 

When looking at this link, you will see it actually doesn't load all the assets properly. Thats because pinata is a free service with strict rate limiting. To get around this, I use cloudflare to cache. 

### How to setup a website on IPFS

In order to follow this guide, you will need the following:

1. Account setup on [pinata](https://www.pinata.cloud/) or a similar pinning service. You can also pin on multiple services for more availability. When creating the account on pinata, make sure you grab the api tokens
2. create an account on [cloudflare](https://www.cloudflare.com/) and make sure its also serving your dns you want to use. My setup i bought a domain through [google](https://domains.google.com/) then followed [this guide](https://developers.cloudflare.com/dns/zone-setups/full-setup/setup)
3. Create a static website, I personally used [hugo](https://gohugo.io/) which is an awesome tool to generate static sites using templates. Feel free to install hugo, and then [follow this guide](https://toha-guides.netlify.app/posts/getting-started/prepare-site/). The most important part is to make sure the template you choose or website you build does not need the domain name to route to sections. If you do, it will have issues.
4. Make sure nodejs is installed locally so you can use [ipfsd](https://github.com/ipfs-shipyard/ipfs-deploy), or leverage my github tasks to deploy it for you

Ok I know the above is a long list of pre-reqs, but why try and re-invent the wheel if someone already created a better guide then you? so if you followed everything exactly, you should now have a functioning hugo website and if you followed the above guide, it should already be deployed to github pages (just to prove it works properly without a base domain setting).