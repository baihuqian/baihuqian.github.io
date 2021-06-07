---
layout: post
title: 'Secure Home Network: Access Your Home Assistant from Internet'
date: '2021-06-06 21:04'
tags:
 - HomeNetwork
 - NAS
 - IoT
---

Home Assistant is great for home automation. I set it up on my [Synology NAS]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %}) [in a Docker container]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %}) and have been gradually adding more integrations and automations to it. Home Assistant's iOS app is another powerful way to add input to my home automation. It can provide my location as an input so I can configure automations to do things when I leave or arrive home. However, the default installation of Home Assistant does not allow remote access (i.e., access from Internet). This post details the process of setting up secure and reliable remote access for my Home Assistant.

# Security Considerations
Before we get started, it is important to consider the security implications.

Internet is the most unsecure network of all, and your home network where Home Assistant sits is the secure zone. Exposing Home Assistant must not expose any of your home network to the Internet.

Home Assistant grants access to your home, such as turning on and off lights and applicances, watching camera feed, and even opening locks. You want to ensure the access is verified and authenticated. This goes both ways: the Home Assistant server must verify the client identity and the client must verify the server. This requires a strong password and potentially two-factor authentication for your Home Assistant user and a valid certificate for your domain.

# Network Considerations
Most home network obtains a dynamic IP from the ISP. With the scarcity of IPv4 addresses, it is possible that ISP allocates private IP addresses to residential network by putting carrier-grade NAT between your router and the Internet. If your IP obtained from your ISP is a private IP, then you are out of luck unless you contact your ISP to allocate you a public IP.

Even if a public IP is provided, dynanmic IP is updated frequently. It is difficult to keep track of this IP and update clients accordingly. Dynamic DNS (DDNS) providers allows you to register a domain and update it with your IP periodically. My EdgeRouter and Synology NAS is able to perform DDNS updates so that when my public IP changes, the DNS record is updated.

After a domain is registered, a valid SSL certificate should be obtained from a certificate authority (CA) instead using a self-signed one. This allows the client to verify the authenticity of the server.

Certificates must be obtained and renewed periodically by the server. Certificate rotation must be handled by the web application of Home Assistant, or a reverse proxy can be used to terminate the SSL/TLS connection and forward requests to the web application in plaintext HTTP.

To not expose your home network to the Internet, port forwarding should be used to forward only the required ports to the IP address in the home network. This avoids adding any "allow" firewall rules on the WAN interface.

An IP address or a host can use a port for a single purpose. When multiple web applications running on the same host, such as my Synology NAS, finding an unused port for additional functionality is important.

# Setup Overview
I'm using the following setup:
* DDNS from [afraid](https://freedns.afraid.org), a free DNS hosting and DDNS provider. It is not affiliated with an for-profit enterprise. Subdomains can be obtained from a list of free top-level domain names for free.
* EdgeRouter updates the DDNS registry because it can do so right after obtaining a new IP address from the ISP, thus minimizing the chance of outdated DDNS entry. On the other hand, Synology NAS runs in my private network so it can only detect IP address changes periodically.
* Obtain a certificate from [Let's Encrypt](https://letsencrypt.org), a non-profit and free CA. Certificate issued by Let's Encrypt expires every 90 days.
* Leverage Synology DSM to obtain and renew certificates. The setup is easy and error-free in the DSM UI compared to manual operations using command line tools.
* Leverage the reverse proxy functionality in Synology SDM. Because I run Home Assistant in a docker container, and Home Assistant expects the certificate in a pre-defined path in `configurations.yaml`, it is too much trouble exporting certificate from Synology DSM and import it into the Docker container, and handle the refreshing and restart of the container myself. This also allows me to continue accessing the HTTP endpoint of Home Assistant within my home network.
* Set up port forwarding.

# DDNS using EdgeRouter
EdgeOS has built-in support for DDNS in its UI. Go to the "**Services**" and "**DNS**" tab, and expand on "Dynamic DNS" section. Input the WAN interface (usually `eth0`), select `afraid` as the service, and input the domain name into `hostname` and your username and password. Leave `Protocol` and `Server` empty.

After clicking "Apply", you should see the domain name is updated with your current public IP address in afraid's console.

# Port Forwarding
You can configure port forwarding under the "**Firewall/NAT**" tab. Enable the "hairpin NAT" setting. At minimum you should map port 80 to port 80 of your NAS for certificate renewal, and an unused public port (I use 8123) to an used port on your NAS (I use 8443 because 8123 is Home Assistant's HTTP port).

# Obtain and Renew Let's Encrypt Certificate
Synology DSM has automated the certificate renew process in its UI. Please refer to [its documentation](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/Network/How_to_enable_HTTPS_and_create_a_certificate_signing_request_on_your_Synology_NAS) for setup. Note, it should already have a self-signed certificate so you should select what functionality you wish the certificate applies to by clicking "**Configure**".

# Set Up Reverse Proxy
You can find the reverse proxy setting under the "**Application Portal**" in the **Control Panel**. I created one with the following:

```
Description: Home Assistant
Source
    Protocol: HTTPS
    Hostname: <your DNS name>
    Port: 8443 // match the port in the port forwarding configuration
Destination
    Protocol: HTTP
    Hostname: localhost
    Port: 8123
```

And add the following in Custom Header to support WebSocket (used by Home Assistant):

```
Name: "Upgrade", Value: "$http_upgrade"
Name: "Connection", Value: "$connection_upgrade"
```

Synology actually created a shortcut in the dropdown of the "**Create**" button. Without this, you will be able to log in but then get an error of "Unable to connect to Home Assistant".

# Set Up Internal URL on Home Assistant iOS
The Home Assistant iOS app can actually configure different URLs when connected to your home WiFi. Go to the "**App Configuration**"  on the sidebar and click on your name. Input the private HTTP endpoint in "Internal URL" and your home WiFi's SSID, and the public HTTPS endpoint in "External URL". This could save a round-trip time to your internet when at home.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
