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
- Patched Versions: 10.0.16
- Vulnerability: Authenticated File Upload to Restricted Tickets
- Impact: <strong>An authenticated user can attach a document to any item without writing access</strong>.
- Security Advisory: [https://github.com/glpi-project/glpi/security/advisories/GHSA-f2cg-fc85-ffmh](https://github.com/glpi-project/glpi/security/advisories/GHSA-f2cg-fc85-ffmh)
- CVE Mitre: [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-37147](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-37147)
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), Sérgio Charruadas

Hello Friend, although I reported other CVEs first, in this article I will be presenting my first published CVE.

First I'll explain what GLPI is and what it's for:

GLPI is an open-source software tool used for IT asset management and service management. It helps organizations keep track of their hardware and software assets, manage IT support requests, and handle maintenance tasks.

Three months after the report, GLPI released version 10.0.16, and the fix for the vulnerability I had reported earlier came with it.

Patched Code:

![](/assets/images/glpi_file_upload/patch_code.png)


But without further ado, let's get to the PoC of this vulnerability:

The exploitation of this vulnerability was quite exciting and not too complicated.

I started by creating a ticket as a user without any permissions.

![](/assets/images/glpi_file_upload/image2.png)

Now in the account with administrator privileges, I've checked that the ticket was created with the ID 2024052702.

![](/assets/images/glpi_file_upload/image3.png)

For this proof of concept, I have also created a ticket in the administrator account so that we can test the upload from a standard user account. This ticket is associated with the ID 2024052703, which will be needed later to execute the file upload in a Restricted Ticket.

![](/assets/images/glpi_file_upload/image5.png)

With the admin ticket ID, we can check that the normal user doesn't have access to the ticket content.

![](/assets/images/glpi_file_upload/image6.png)

When I realized that I had no way of knowing the content of the administrator ticket, I decided to explore its other features besides the chat. That's when I discovered I could upload documents into the ticket chat.

![](/assets/images/glpi_file_upload/image7.png)

So I immediately intercepted the request made to the server when we tried to upload a file.

![](/assets/images/glpi_file_upload/image8.png)

![](/assets/images/glpi_file_upload/image9.png)

When I came across the ID of my ticket in the request, I immediately thought, <strong>"Could it be that by changing my ticket ID to the ID of another ticket, I can assign the file I uploaded to a ticket that isn't mine?"</strong>.

I proceeded to test whether it would be possible to associate the file with the ticket we previously created using the administrator account.

![](/assets/images/glpi_file_upload/image10.png)

And Voila! We received a success message on the front end.

![](/assets/images/glpi_file_upload/image11.png)

We were also check in the admin panel that our user had added a file to the ticket that the normal user couldn’t access.

![](/assets/images/glpi_file_upload/image12.png)


<strong>The body cannot live without the mind</strong>

- Morpheus

