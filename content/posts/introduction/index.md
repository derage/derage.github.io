---
title: "Creating My first website"
date: 2020-06-08T08:06:25+06:00
description: Creating My first website
menu:
  sidebar:
    name: Creating My first website
    identifier: serverless
    weight: 10
tags: ["Basic"]
categories: ["Basic"]
---


## The journey of creating My first website

EDIT: this blog below you can find on my old website: https://www.cafarelli.org. Checkout the new post where I use IPFS to make it even cheaper

This website I have started and stopped multiple times. Choosing multiple different technology stacks. Slowly but surly failing to get it started till I landed here. Here was the journey I embarked on, and where I am so far.

My First website I ever tried to approach was a [serverless blog site](https://github.com/derage/serverless-blog-workshop). Wanting to learn about cloud and serverless technologies, I started building out this basic workshop first in AWS. Spinning up multiple components and starting to hit the website I started to get excited with where it was going, and this is where my first mistake happened. My projected cost to run this website was $50 a month, which was to much for something I was just testing out. Wanted to aim for something that was cheaper I ripped it all down with plans on coming back.

My next website I started was 3 years ago using [react and nodejs](https://github.com/derage/resume_website). The idea was I would [store any blogs in a backend database](https://github.com/derage/blog_backend), [find a template I liked](https://themes.3rdwavemedia.com/bootstrap-templates/resume/orbit-free-resume-cv-bootstrap-theme-for-developers/), then remake it using nodejs and react. I was successful in finally setting up my first resume site. The problems started to hit when I need to make changes. At this point there were no free continous deployment technologies available without having to pay for it. The archtechture was also very complex for someone trying to learn nodejs, microservice archtechture, and aws networking. When I went to make my next change, I couldnt get it to load properly in my browser. At this time work started to consume alot of my time. I needed something simpler to deploy and maintain.

And this lead us to today. A launched website that also has a blog engine, and this only runs out of a GCP bucket and uses cloudflare for security. The extra part is it auto deploys on commits so I dont need to worry any further about troubleshooting issues. Coming soon will be a repo and tutorials on how I achieved this.
