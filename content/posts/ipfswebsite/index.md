---
title: "Creating an IPFS site"
date: 2022-02-23T08:06:25+06:00
description: Creating an IPFS site
menu:
  sidebar:
    name: Creating This website on IPFS
    identifier: ipfs
    weight: 10
tags: ["Basic", "Multi-lingual"]
categories: ["Basic"]
---


## IPFS

TLDR: If your like  me and learn from reading code over guides. Here is the repo [https://github.com/derage/derage.github.io](https://github.com/derage/derage.github.io). You can checkout how to [deploy it here](https://github.com/derage/derage.github.io/blob/source/.github/workflows/deploy-gh-site.yaml)

TLDR-TLDR: if you dont care about any of the tech and just want to hope right into IPFS, [you can take a look at this service](https://docs.ipfs.io/how-to/websites-on-ipfs/introducing-fleek/#host-a-site)

I present to you a partially distributed website running on IPFS and using cloudflare to cache the website and DNS. If you are using a brave browser or chrome with the IPFS extension installed, you can go to [ipfs://QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ](ipfs://QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ). If you are on a traditional browser, you can check out the website pinned to pinata at [https://gateway.pinata.cloud/ipfs/QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ/](https://gateway.pinata.cloud/ipfs/QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ/). 

When looking at this link, you will see it actually doesn't load all the assets properly. Thats because pinata is a free service with strict rate limiting. To get around this, I use cloudflare to cache. 

### What is IPFS, Why would you use it and how do you use it?

The easiest way to explain IPFS is its like an S3 bucket if that bucket ran on AWS, Google cloud, Azure, and a bunch of other servers down to someones server in their garage. The ARN of this bucket is called a CID and you use this CID to reference the snapshot of the contents uploaded at the time.

IPFS has some major issues that you have to plan for when using it, but the benefits can be amazing. Out of all the benefits, one is uptime. One can easily run a website as a multi-cloud site. That means if, lets say, AWS has an outage, your site will still function.

In order to use IPFS, you need to tell IPFS nodes to hold onto your files for you. This is called pinning. Onced pinned, your files are guaranteed to be distrubuted across all the nodes in the ecosystem for a certain amount of time. Some nodes are free with extreme rate limiting, others you need to pay a renter fee; but all will have a certain TTL that reset after every access. For instance, if you pin on infura its free, but after 6 months of not being accessed your files will be wiped. 

The best part of all this is you can actually run your own IPFS node on your own servers to contribute to this ecosystem. By doing this you can forever pin your own personal files and be apart of the distrubuted file system future. If you interested in looking more into [this check out this link.](https://docs.ipfs.io/how-to/command-line-quick-start/#take-your-node-online)

### How to setup a website on IPFS

In order to follow this guide, you will need the following:

1. Account setup on [pinata](https://www.pinata.cloud/) or a similar pinning service. You can also pin on multiple services for more availability. When creating the account on pinata, make sure you grab the api tokens
2. create an account on [cloudflare](https://www.cloudflare.com/) and make sure its also serving your dns you want to use. My setup i bought a domain through [google](https://domains.google.com/) then followed [this guide](https://developers.cloudflare.com/dns/zone-setups/full-setup/setup)
3. Create a static website, I personally used [hugo](https://gohugo.io/) which is an awesome tool to generate static sites using templates. Feel free to install hugo, and then [follow this guide](https://toha-guides.netlify.app/posts/getting-started/prepare-site/). The most important part is to make sure the template you choose or website you build does not need the domain name to route to sections. If you do, it will have issues.
4. Make sure nodejs is installed locally so you can use [ipfsd](https://github.com/ipfs-shipyard/ipfs-deploy), or leverage my github tasks to deploy it for you

Ok I know the above is a long list of pre-reqs, but why try and re-invent the wheel if someone already created a better guide then you? so if you followed everything exactly, you should now have a functioning hugo website and if you followed the above guide, it should already be deployed to github pages (just to prove it works properly without a base domain setting). Now lets try to upload to ipfs, and pin it.

First you can try to do this locally if you want to see all this in action. make sure to install [the ipfs cli](https://docs.ipfs.io/how-to/command-line-quick-start/#prerequisites). When I build my hugo site, all my static sites are generated to a folder locally called `public`. So with the ipfs cli installed, we can run `ipfs add -r public`. This should show all the files being upload to IPFS on your local private node running. The final output says the public folder. This is considered the parent CID that all your files can be referenced to:

```
...
...
...
added QmPbEWHYgATpN3uKtL2Qu4xzwnZQzCrqa12MCsv3caj5KH public/tags/multi-lingual/page
added QmaL2TUXK2M8iCznCdnr9fZPaoczxQUb9Z41A6CWcYLDxA public/tags/multi-lingual
added QmPNSdFGu8gcRxnWLm6zqEa4kCvZctLHv3BPj2XVUMX4Tb public/tags/page/1
added QmfWJrwG7QNqEj4G5Aths4Hwn77GFd1peMdko3SMWPcTZo public/tags/page
added QmVGQdeo1GnPWPZFar2nfuvVywGPn3J1Gvcmb8uNEStjQi public/tags
added QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ public
 5.51 MiB / 5.51 MiB [===========================================================================] 100.00%
```


And now your site is on IPFS! You can even run this command by finding one of the CID's from the output that points to a file and get the contents:

```
ipfs cat /ipfs/QmUSqRppqoPooQ6CfQeJsmdRpbALHCGx7kXT9vZJXaEtLw
<!doctype html><html><head><title>/tags/basic/</title><link rel=canonical href=../../../../tags/basic/><meta name=robots content="noindex"><meta charset=utf-8><meta http-equiv=refresh content="0; url=../../../../tags/basic/"></head></html>%   
```

Ok so now you have it added locally, but you still cant browse it in the web because we still need to pin it to a service. There are actually a couple out there you can use. [infura](https://infura.io/), [nftstorage](https://nft.storage/), and [pinata](https://www.pinata.cloud/). I will use the later one:

Make a free account at pinata, and generate a [api key at this location](https://app.pinata.cloud/keys). Make sure to grab the JWT and run the commands below to add pinata and upload

```
# Replace the JWT for the service
ipfs pin remote service add pinata https://api.pinata.cloud/psa $PINATA_JWT
# Grab the parent CID from the add above, and replace the var below
ipfs pin remote add $PARENT_CID --service=pinata --name=mysite
```

And now you site should be uploaded to IPFS! you can see your new site in a browser going to a url like this: `https://gateway.pinata.cloud/ipfs/$PARENT_CID`

Now if your site is not loading properly, Dont fear. open up the developer console on your browser and lets see the errors your getting. if you are seeing a bunch of `429 (Too Many Requests)` then thats perfect! we will fix this in the next step. If you are seeing other likes like `404` then your site is not using relative paths. All I had to do for my theme was to set `relativeURLs = true` and make sure to remove `baseURL:` setting, but if this does not work for you, your theme might not be properly created and is using full paths for some reason. [Heres an example article to help troubleshoot further](https://discourse.gohugo.io/t/how-can-i-force-hugo-to-generate-relative-paths-for-everything/24168/9)

### Use Cloudflare to cache the site and use DNS

Ok so the site is up, but your getting rate limited. we can get around this if we run our own public IPFS node, but why go through all that trouble? instead lets cache the pages on cloudflare and even use an SSL cert issued by cloudflare for extra security. 

* CNAME record for <your_domain> pointing to cloudflare-ipfs.com
* TXT record for _dnslink.<your_domain> with the value dnslink=/ipfs/<your_parent_cid>

For reference, here are my DNS records:
* CNAME jesse.cafarelli.org cloudflare-ipfs.com
* TXT _dnslink.jesse.cafarelli.org dnslink=/ipfs/QmaQymof8voW1HNPAK4FCJWxbopL94aBpD34qR696n5CEJ

This will take about 5-10 minutes to start working, but might be a little longer if it needs to cache a lot of files. And thats it! your site should fully be loading now.

### Automated CICD using github workflows

Now what type of devops person would I be if I didnt have some sort of CICD pipeline for future changes. This is where [ipfs-deploy tool](https://github.com/ipfs-shipyard/ipfs-deploy) comes into play. Whats awesome about this tool is it does everything we just did; Automatically!

To get this working locally, first install the tool, then use the pinata credentials you had before. you will also need to create a cloudflare token. To create the token:

1. [go to this url in cloud flare](https://dash.cloudflare.com/profile/api-tokens)
2. create a new token and use `Edit zone DNS` as a template
3. select the zone your DNS is in
4. Create the token


And now run this! keep in mind, `public/` is my static folder locally. Im running this from my websites repo. Also the `-C` means do not copy. This is critical for working in github workflow

```
export IPFS_DEPLOY_PINATA__API_KEY=MY_API_KEY
export IPFS_DEPLOY_PINATA__SECRET_API_KEY=MY_API_SECRET
export IPFS_DEPLOY_CLOUDFLARE__API_TOKEN=YOUR_CLOUDFLARE_TOKEN
export IPFS_DEPLOY_CLOUDFLARE__ZONE=YOUR_ZONE # mines cafarelli.org
export IPFS_DEPLOY_CLOUDFLARE__RECORD=YOUR_DNS_LINK_RECORD # mines _dnslink.jesse.cafarelli.org

npx ipfs-deploy -C public/ -p pinata -d cloudflare
```

If you dont see an errors then everything is working properly! Now lets have github do this all for us and have it upload on each push. If you followed the hugo theme tutorial, then your main branch is `source`. modify the workflow code if its does that match. First we need to add the secrets we just used locally to our repo. `https://github.com/GITHUB_USERNAME/GITHUB_REPO/settings/secrets/actions`

Once you add all the secrets, in your repo locally, create a file in `.github/workflows/deploy-gh-site.yaml` and fill with these contents:

```
name: Deploy to Github Pages

# run when a commit is pushed to "source" branch
on:
  push:
    branches:
    - source

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    # checkout to the commit that has been pushed
    - uses: actions/checkout@v2
      with:
        submodules: true  # Fetch Hugo themes (true OR recursive)
        fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
    
    # install Hugo
    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: '0.77.0'
        extended: true

    # build website
    - name: Build
      run: hugo --minify

    # push the generated content into the `main` (former `master`) branch.
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_branch: main # if your main branch is `master` use that here.
        publish_dir: ./public
    
    # Setup nodejs on the runner so we can run the deploy script
    - name: Use Node.js 16.x
      uses: actions/setup-node@v2
      with:
        node-version: 16.x
    
    # Use the awesome tool to deploy to pinata and cloudflare!
    - run: npx ipfs-deploy -C public/ -p pinata -d cloudflare
      env:
        # Make sure to set these up in your actions secrets so the tool can use them
        IPFS_DEPLOY_CLOUDFLARE__API_TOKEN: ${{ secrets.IPFS_DEPLOY_CLOUDFLARE__API_TOKEN }}
        IPFS_DEPLOY_CLOUDFLARE__RECORD: ${{ secrets.IPFS_DEPLOY_CLOUDFLARE__RECORD }}
        IPFS_DEPLOY_CLOUDFLARE__ZONE: ${{ secrets.IPFS_DEPLOY_CLOUDFLARE__ZONE }}
        IPFS_DEPLOY_PINATA__API_KEY: ${{ secrets.IPFS_DEPLOY_PINATA__API_KEY }}
        IPFS_DEPLOY_PINATA__SECRET_API_KEY: ${{ secrets.IPFS_DEPLOY_PINATA__SECRET_API_KEY }}
```

Commit this to your repo and BAM! you now are rocking IPFS with cloudflare caching; and a fully automated pipeline to upload!

### Whats next?

Well now we have a partially distributed site, but can we do better? My next plan is to work on using a distributed DNS service as well. My plan is to check out [unstoppable dns](https://unstoppabledomains.com/). I love the idea of dns through NFT's.

After that, what about a backend running on smart contracts? Maybe a user login with a comment board? Or maybe the posts going on chain instead of in ipfs? Then the posts are forever and you dont need to deploy the entire site any time you do a new post!

### Helpful links

https://toha-guides.netlify.app/posts/getting-started/github-pages/

https://steemit.com/ipfs/@steefmin/hugo-and-ipfs

https://alphasec.io/how-to-host-a-website-on-ipfs-using-pinata-and-cloudflare/

This sites github page
  https://github.com/derage/derage.github.io