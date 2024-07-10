---
layout: single
title: Authenticated File Upload to Restricted Tickets (CVE-2024-37147)
excerpt: "I've found a way to upload a file to a restricted ticket in GLPI."
date: 2024-07-10
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
- Patched versions: 10.0.16
- Vulnerability: Authenticated File Upload to Restricted Tickets
- Impact: <strong>An authenticated user can attach a document to any item, even if he has no write access on it.</strong>.
- Security Advisory: https://github.com/glpi-project/glpi/security/advisories/GHSA-f2cg-fc85-ffmh
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), Sérgio Charruadas

Hello Friend, today I'm presenting my first Published CVE but not my first reported CVE.
First I'll explain what GLPI is and what it's for:

GLPI is an open-source software tool used for IT asset management and service management. It helps organizations keep track of their hardware and software assets, manage IT support requests, and handle maintenance tasks.

Three months after the report, GLPI released version 10.0.16 and with it came the fix for the vulnerability I reported earlier.

Patch Code:

![](/assets/images/glpi_file_upload/patch_code.png)


But without further ado, let's get to the PoC of this vulnerability:

The exploitation of this vulnerability was quite interesting and not too complicated.

I started by creating a ticket as a user without any permissions.

![](/assets/images/glpi_file_upload/image2.png)

Now in the account with administrator privileges I've checked that the ticket was created with the id 2024052702.

![](/assets/images/glpi_file_upload/image3.png)

For this proof of concept, I have also created a ticket in the administrator account so that we can test the upload from a normal user account. This ticket is associated with the ID 2024052703, which will be needed later to execute the file upload in a Restricted Ticket.

![](/assets/images/glpi_file_upload/image5.png)

With the admin ticket id we can check that the normal user doesn't have access to the content of the ticket.

![](/assets/images/glpi_file_upload/image6.png)

When I realized that I had no way of knowing the content of the administrator ticket, I decided to explore its other features besides the chat. That's when I discovered I could upload documents into the ticket chat.

![](/assets/images/glpi_file_upload/image7.png)

So I immediately went to intercept the request that was made to the server when we add a file.

![](/assets/images/glpi_file_upload/image8.png)

![](/assets/images/glpi_file_upload/image9.png)

When I came across the ID of my ticket in the request, I immediately thought, <strong>"Could it be that by changing my ticket ID to the ID of another ticket, I can assign the file I uploaded to a ticket that isn't mine?"</strong>.

I proceeded to test whether it would be possible to associate the file with the ticket we previously created using the administrator account.

![](/assets/images/glpi_file_upload/image10.png)

And Voila! We received a success message on the frontend.

![](/assets/images/glpi_file_upload/image11.png)

We were also able to check in the admin panel that our user had added a file to the ticket that the normal user didn't have access to.

![](/assets/images/glpi_file_upload/image12.png)


<strong>The body cannot live without the mind</strong>

- Morpheus

