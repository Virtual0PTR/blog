+++
date = '2000-01-01T00:00:00+00:00'
draft = false
title = 'Dn42 Peering Information'
image = '/img/dn42.jpg'
tags = ["DN42"]
+++

## Basic Information

ASN:[`AS4242420555`](https://explorer.burble.com/#/aut-num/AS4242420555)

DN42 IPv4:[`172.22.122.128/27`](https://explorer.burble.com/#/route/172.22.122.128_27)

DN42 IPv6:[`fdee:9bff:b001::/48`](https://explorer.burble.com/#/route6/fdee:9bff:b001::_48)

Domain:[`0xptr.dn42`](https://explorer.burble.com/#/domain/0xptr.dn42)

Looking glass:[`lg-dn42`](https://lg-dn42.0xptr.top)

IPV6 Link-local address: `fe80::d555`

Wireguard Listening Port：The last five digits of your ASN

## Contact Details

[`VIRTUAL0PTR-DN42`](https://explorer.burble.com/#/person/VIRTUAL0PTR-DN42)

## Precautions

+  Only Wireguard tunnels are supported (currently)
+  I prefer MP-BGP and ENH (but optional)
+  There is no mainland China node, so peers from mainland China are not accepted
+  `If the network crashes, please let me know.`

You need to use the following template when peering:

``` bash
ASN:
Public IP:
DN42 IPv4:
DN42 IPv6:
LLA IPv6:
Server:
WireGuard Public Key:
WireGuard Listen Port: 20555
Transmit Routes: (IPv6/IPv4/both)
Multi-Protocol BGP: (true (IPv4/IPv6) /false)
Extended Next Hop: (true/false)
```

## My Node

1. **HK-01**
  + Endpoint: hk-01.node.dn42.0xptr.top
  + DN42 Address:
    + 172.22.122.129
    + fdee:9bff:b001::1
    + fe80::d555
  + Wireguard Public Key:\
  xin3e9mT1MVzPUVGG7pVIZ\
  /FIkwOIGQNN2cVpCTLNS8=
  + Bandwidth: 1 Gbps ([`communities`](https://dn42.dev/howto/BGP-communities) may be used)

