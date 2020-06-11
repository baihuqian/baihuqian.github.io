---
layout: post
title: 'DNS: a Crash Course'
date: '2020-06-11 16:27'
tags:
 - Networking
---

DNS, or the Domain Name System, translates human readable domain names (for example, `www.amazon.com`) to machine readable IP addresses (for example, `192.0.2.44`). DNS system is a public, hierarchical, distributed and heavily cached database. DNS queries go through the following steps:
1. User sends a recursive DNS query to a resolver.
2. Resolver sends the request to a root nameserver based on domain name. Root nameserver responds with the domain's TLD nameserver.
3. Resolver sends the request to the TLD nameserver, which returns the authoritative nameserver of the domain name.
3. Resolver sends the requesst to the authoritative nameserver which responds with a DNS record.
4. If the returned record is a CNAME record, the recursive resolver starts a new DNS request with the domain name replaced with the alias.

# Types of DNS Server

## Recursive Resolver
Clients typically do not make queries directly to authoritative DNS services. Instead, they generally connect to another type of DNS service known a **resolver**, or a **recursive DNS** service. While it doesn't own any DNS records, it acts as an intermediary who can get the DNS information on your behalf. If a recursive DNS has the DNS reference cached, or stored for a period of time, then it answers the DNS query by providing the source or IP information. If not, it passes the query to one or more authoritative DNS servers to find the information.

## Root Name Server
The 13 DNS root nameservers are known to every recursive resolver, and they are the first stop in a recursive resolver's quest for DNS records. A root server accepts a recursive resolver’s query which includes a domain name, and the root nameserver responds by directing the recursive resolver to a TLD (Top Level Domain) nameserver, based on the extension of that domain (`.com`, `.net`, `.org`, etc.). The root nameservers are overseen by a nonprofit called the Internet Corporation for Assigned Names and Numbers (ICANN).

## TLD Name Server
A TLD nameserver maintains information for all the domain names that share a common domain extension, such as `.com`, `.net`, or whatever comes after the last dot in a url. For example, a `.com` TLD nameserver contains information for every website that ends in `.com`. If a user was searching for `amazon.com`, after receiving a response from a root nameserver, the recursive resolver would then send a query to a `.com` TLD nameserver, which would respond by pointing to the authoritative nameserver (see below) for that domain.

Management of TLD nameservers is handled by the Internet Assigned Numbers Authority (IANA), which is a branch of ICANN. The IANA breaks up the TLD servers into two main groups:
* Generic top-level domains: These are domains that are not country specific, some of the best-known generic TLDs include `.com`, `.org`, `.net`, `.edu`, and `.gov`.
* Country code top-level domains: These include any domains that are specific to a country or state. Examples include `.uk`, `.us`, `.ru`, and `.jp`.

## Authoritative Name Server
When a recursive resolver receives a response from a TLD nameserver, that response will direct the resolver to an authoritative nameserver. The authoritative nameserver is usually the resolver’s last step in the journey for an IP address. The authoritative nameserver contains information specific to the domain name it serves (e.g. `amazon.com`). AWS Route 53 is an authoritative DNS system and it provides an update mechanism that developers use to manage their public DNS names. Cloudflare DNS (`1.1.1.1`) distributes authoritative nameservers, which come with Anycast routing to make them more reliable.

# DNS Record Types
DNS servers create a DNS record to provide important information about a domain or hostname, particularly its current IP address. The most common DNS record types are:
* Address Mapping record (**A** Record) — also known as a DNS host record, stores a hostname and its corresponding IPv4 address.
* IP Version 6 Address record (**AAAA** Record) — stores a hostname and its corresponding IPv6 address.
* Canonical Name record (**CNAME** Record) — can be used to alias a hostname to another hostname. When a DNS client requests a record that contains a CNAME, which points to another hostname, the DNS resolution process is repeated with the new hostname.
* Mail exchanger record (**MX** Record) — specifies an SMTP email server for the domain, used to route outgoing emails to an email server.
* Name Server records (**NS** Record) — specifies that a DNS Zone, such as “example.com” is delegated to a specific Authoritative Name Server, and provides the address of the name server.
* Reverse-lookup Pointer records (**PTR** Record) — allows a DNS resolver to provide an IP address and receive a hostname (reverse DNS lookup).
* Certificate record (**CERT** Record) — stores encryption certificates such as PKIX, SPKI, PGP, and so on.
* Service Location (**SRV** Record) — a service location record, like MX but for other communication protocols.
* Text Record (**TXT** Record) — typically carries machine-readable data such as opportunistic encryption, sender policy framework, DKIM, DMARC, etc.
* Start of Authority (**SOA** Record) — this record appears at the beginning of a DNS zone file, and indicates the Authoritative Name Server for the current DNS zone, contact details for the domain administrator, domain serial number, and information on how frequently DNS information for this zone should be refreshed.

A DNS zone is any distinct, contiguous portion of the domain name space in the Domain Name System (DNS) for which administrative responsibility has been delegated to a single manager. [Hosted Zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-hosted-zone) provided by Amazon Route 53 is a DNS zone.

# DNS TTL
The internet is a dynamically changing place and the DNS records ensure users can always access the websites from the right place. Time To Live, or TTL for short, is the sort of expiration date that is put on a DNS record. The TTL serves to tell the recursive server or local resolver how long it should keep said record in its cache.

TTLs are nothing to take lightly - they can directly affect the amount of query volume that is attributable to your authoritative service, and in the event of needing to quickly change the record, can result in longer than expected change propagation to all users.

# DNS Query
A DNS query (also known as a DNS request) is a demand for information sent from a user's computer (DNS client) to a DNS server. In most cases a DNS request is sent, to ask for the IP address associated with a domain name.

In general, there are two ways of resolving a host or a domain name to an IP address, using the domain name system – a Recursive query and a non-Recursive query.
* The Recursive query is, when a DNS client directly gets the IP address of a domain, by asking the name server system to perform the complete translation.
* The non-Recursive query is, when a DNS client contacts the name servers, one by one, until it finds the server, containing the needed information.

DNS query primarily uses the User Datagram Protocol (UDP) on port number 53 to serve requests.

# Security
DNS responses traditionally do not have a cryptographic signature, allowing forging responses with incorrect IP and long TTL, such as man-in-the-middle attack and DNS cache poisoning. The Domain Name System Security Extensions (DNSSEC) attempts to add security, while maintaining backward compatibility. DNSSEC does not provide confidentiality of data; in particular, all DNSSEC responses are authenticated but not encrypted.

# Privacy and Tracking
DNS protocol has no confidentiality controls. User queries and nameserver responses are being sent unencrypted which enables network packet sniffing, DNS hijacking, DNS cache poisoning and man-in-the-middle attacks. This deficiency is commonly used by cybercriminals and network operators for marketing purposes, user authentication on captive portals and censorship.

The main approaches are in use to counter privacy issues with DNS:
* VPNs, which move DNS resolution to the VPN operator and hide user traffic from local ISP,
* Tor, which replaces traditional DNS resolution with anonymous `.onion` domains, hiding both name resolution and user traffic behind onion routing counter-surveillance,
* Proxies and public DNS servers, which move the actual DNS resolution to a third-party provider, who usually promises little or no request logging and optional added features, such as DNS-level advertisement or pornography blocking. Public DNS servers can be queried using traditional DNS protocol, in which case they provide no protection from local surveillance, or DNS-over-HTTPS, DNS-over-TLS and DNSCrypt, which do provide such protection.

I wrote a post on [Pi-Hole with DNS-over-HTTPS]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) that attempts to stop my ISP from selling my DNS query information for marketing purposes.

# References
* [How DNS works by VeriSign](https://www.verisign.com/en_US/website-presence/online/how-dns-works/index.xhtml)
* [What is DNS by Amazon Route 53](https://aws.amazon.com/route53/what-is-dns/)
* [DNS: Types of DNS Records, DNS Servers and DNS Query Types](https://ns1.com/resources/dns-types-records-servers-and-queries)
* [DNS Record Types](https://en.wikipedia.org/wiki/List_of_DNS_record_types)
* [DNS TTL](https://ns1.com/resources/understanding-ttl-values-in-dns-records)
