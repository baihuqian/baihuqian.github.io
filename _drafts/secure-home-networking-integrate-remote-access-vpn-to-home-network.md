---
layout: "post"
title: "Secure Home Networking: Integrate Remote Access VPN to Home Network"
date: "2023-01-27 20:03"
---

## Multicast DNS
It doesn't work over OpenVPN's TUN.
```
set service mdns repeater interface vtun0
```

## IPv6
```
set firewall ipv6-name IPV6WAN_LOCAL description 'IPV6WAN to router'
set firewall ipv6-name IPV6WAN_LOCAL default-action drop

set firewall ipv6-name IPV6WAN_LOCAL rule 10 action accept
set firewall ipv6-name IPV6WAN_LOCAL rule 10 state established enable
set firewall ipv6-name IPV6WAN_LOCAL rule 10 state related enable
set firewall ipv6-name IPV6WAN_LOCAL rule 10 log disable
set firewall ipv6-name IPV6WAN_LOCAL rule 10 description 'Allow established/related'

set firewall ipv6-name IPV6WAN_LOCAL rule 20 action drop
set firewall ipv6-name IPV6WAN_LOCAL rule 20 state invalid enable
set firewall ipv6-name IPV6WAN_LOCAL rule 20 description 'Drop invalid state'

set firewall ipv6-name IPV6WAN_LOCAL rule 30 action accept
set firewall ipv6-name IPV6WAN_LOCAL rule 30 description openvpn
set firewall ipv6-name IPV6WAN_LOCAL rule 30 log disable
set firewall ipv6-name IPV6WAN_LOCAL rule 30 destination port 1194
set firewall ipv6-name IPV6WAN_LOCAL rule 30 protocol udp

set interfaces ethernet eth0 firewall local ipv6-name IPV6WAN_LOCAL

commit
save
```
