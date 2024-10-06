---
layout: single
title: Weak Telnet Remote Access on PIX-LINK LV-WR22 (CVE-2024-46280)
excerpt: "I found a way to access the router's telnet using weak credentials"
date: 2024-08-28
classes: wide
header:
  teaser: /assets/images/pix_link_cve/PIX-LINK.png
  teaser_home_page: true
  icon: #/assets/images/hackthebox.webp
categories:
  - CVE
tags:
  - Routers
  
---
Vulnerability Information:
- Product: PIX-LINK
- Model: LV-WR22
- Firmware Version: RE3002-P1-01_V117.0
- Vulnerability: Weak Telnet Remote Access
- Impact: <strong>Root access to the device via the local network.</strong>.
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), Sérgio Charruadas

Hello friend, today I'm showing you a vulnerability that I found while researching the PIX-Link LV-WR22 router, although it's critical, it's quite easy to exploit. 

The vulnerability consists of a misconfiguration on the part of the firmware developers in the telnet configuration and the consumer is unable to change the telnet password without technical knowledge.

<strong>Firmware Version:</strong>

![](/assets/images/pix_link_cve/weak_telnet_firmware_version.png)

PoC:

Accessing telnet via its default port (port 23) we are asked for the username and password.

When I saw that I immediately thought...

![](/assets/images/pix_link_cve/weak_telnet_meme.png)

GOTCHA!!!!

![](/assets/images/pix_link_cve/weak_telnet_poc.png)

Although it's a rather stupid vulnerability, it's a vulnerability that we see in many systems today. 


<strong>Growth hackers don't tolerate waste.</strong>

- Ryan Holiday

