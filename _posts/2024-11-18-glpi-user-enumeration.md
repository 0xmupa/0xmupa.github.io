---
layout: single
title: Users email enumeration by unauthenticated user (CVE-2024-43416)
excerpt: "I and my team found a way to enumerate emails without authentication in GLPI"
date: 2024-11-18
classes: wide
header:
  teaser: /assets/images/glpi_file_upload/glpi_logo.png
  teaser_home_page: true
  icon: #/assets/images/hackthebox.webp
categories:
  - CVE
tags:
  - Open Source
  
---
Vulnerability Information:
- Product: GLPI
- Affected Versions: >= 0.85
- Patched Versions: 10.0.17
- Vulnerability: Users email enumeration by unauthenticated user
- Impact: <strong>An unauthenticated user can use an application endpoint to check if an email address corresponds to a valid GLPI user</strong>.
- Security Advisory: [https://github.com/glpi-project/glpi/security/advisories/GHSA-j8gc-xpgr-2ww7](https://github.com/glpi-project/glpi/security/advisories/GHSA-j8gc-xpgr-2ww7)
- CVE Mitre: [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-43416](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-43416)
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), João Martinez (0xASTRA), Sérgio Charruadas Threat Hunter - Bernardo Rendeiro (Try0WR)

Hello friend, today I'm showing you a vulnerability found together with the rest of my team.

The way the vulnerability was found was quite interesting, because at first we saw that when the email didn't exist 
due to a PHP misconfiguration, it returned a PHP error on the frontend. 
This happened because the PHP config had display_errors = on, and to fix it we had to change it to off.

After that, we were pretty sad because we had been bait by a misconfiguration and that's when we thought, “Could it be that this error in the backend, even if it doesn't show up, could affect the response time between the server and the client?”. 

When testing, we found that if the email exists on the server, it takes almost 1.5 seconds longer than when the email doesn't exist.

<strong>When the email doesn't exist:</strong>

![](/assets/images/glpi_email_enumeration/image1.png)

So if the email exists in the database on the backend, since it's a password recovery endpoint, it will send an email and that's why it takes longer to get the response back to the client.

![](/assets/images/glpi_email_enumeration/image2.png)


<strong>If you give a hacker a new toy, the first thing he'll do is take it apart to figure out how it works.</strong>

- Jamie Zawinski

