+++
title = "DN42 Peering Information"
date = 2025-02-02T17:25:00+08:00
draft = false
+++

## Basic Information {#basic-information}

-   ASN: [AS4242420555](https://explorer.burble.com/#/aut-num/AS4242420555)
-   DN42 IPv4: [172.22.122.128/27](https://explorer.burble.com/#/route/172.22.122.128_27)
-   DN42 IPv6: [fdee:9bff:b001::/48](https://explorer.burble.com/#/route6/fdee:9bff:b001::_48)
-   Domain: [0xptr.dn42](https://explorer.burble.com/#/domain/0xptr.dn42)
-   Looking glass: [lg-dn42](https://lg-dn42.0xptr.top/)
-   IPv6 Link-Local Address: fe80::d555
-   WireGuard Listening Port: last five digits of your ASN


## Contact {#contact}

[VIRTUAL0PTR-DN42](https://explorer.burble.com/#/person/VIRTUAL0PTR-DN42)


## Notes {#notes}

-   (Currently) only supports WireGuard tunnels
-   I prefer using MP-BGP with ENH (but optional)
-   No nodes within China, so peering within China is not accepted
-   _If the network goes down, please let me know_

When peering, you need to use the following template:

```text
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


## My Nodes {#my-nodes}

-   HK-01
    -   Endpoint: hkg-hk-1.node.0xptr.top
    -   DN42 Addresses:
        -   172.22.122.129
        -   fdee:9bff:b001::1
        -   fe80::d555
    -   WireGuard Public Key: xin3e9mT1MVzPUVGG7pVIZ/FIkwOIGQNN2cVpCTLNS8=
    -   Bandwidth: 1 Gbps (Communities may be used)
