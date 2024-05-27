---
layout: single
title: Unauthenticated DoS on PIX-LINK LV-WR21Q ("CVE requested")
excerpt: "I've found a way to perform a denial of service and cause service disruption on the router gateway"
date: 2024-05-28
classes: wide
header:
  teaser: /assets/images/pix_link_cve/PIX-LINK-DOS.png
  teaser_home_page: true
  icon: #/assets/images/hackthebox.webp
categories:
  - CVE
tags:
  - Routers
  
---
Vulnerability Information:
- Product: PIX-LINK
- Model: LV-WR21Q
- Firmware Version: V109_109
- Vulnerability: Unauthenticated DoS (Denial Of Services)
- Impact: <strong>Service Disruption</strong>.
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), Sérgio Charruadas

Hello friend, today I'm presenting a DoS (Denial Of Services) found in the gateway of a Pix-Link router (LV-WR21Q).

What's happening in the backend is that the router can't understand which language code we're trying to set and it causes a <strong>Service Disruption</strong>.

We don't need to be authenticated at the gateway to execute this vulnerability. However it will affect the administrator, making it impossible to access the router's settings.

PoC:

1. Access the gateway login and intercept the request to change the interface language.
2. In the "lang" parameter, change the parameter to a random string. e.g. exploit (as shown in the POC video)

![](/assets/images/pix_link_cve/PIXLINK-DOS-request.png)

3. Attack executed. And now there's no way to access the gateway via the GUI (Graphical User Interface).


Check the [POC Video](https://youtu.be/fF0E5SpjEfc).


<strong>My primary goal of hacking was the intellectual curiosity, the seduction of adventure.</strong>

- Kevin Mitnick

