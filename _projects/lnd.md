---
title: "Lightning Network Adapter"
summary: "API Wrapper + Infrastructure"
---

[Github](https://github.com/chadlohrli/scale-lnd)

{% assign image_path = '/assets/images/lnd' %}

### Background
I spent some time brainstorming with Ryan Berkun at [Fabrx](https://www.fabrx.io/) on different
ways of integrating a lightning network adapter into their vast array of API wrappers. The goal of 
Fabrx is to be the ultimate API wrapper for all blockchain services.

### Lightning Network
Running a lightning node ([lnd](https://github.com/lightningnetwork/lnd)) isn't the most straightforward
task. The idea was to make lightning payments as simple as point and click. To accomplish this there needed to
be some backend infrastructure and API re-routing of lnd. Below is an MVP draft of such system where each user interacting
with the platform could spawn their own lnd server. Each of these individual nodes are controlled by a master node which re-routes 
the internal API calls to lnd. This was done on the lnd simnet.


<img src="{{site.url}}{{image_path}}/lnd.jpg" width="700" height="1000" />    
