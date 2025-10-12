+++
date = '2000-01-01T00:00:00+00:00'
draft = false
title = 'Dn42 对等信息'
image = 'img/dn42.jpg'
tags = ["DN42"]
+++

## 基本信息

ASN:[`AS4242420555`](https://explorer.burble.com/#/aut-num/AS4242420555)

DN42 IPv4:[`172.22.122.128/27`](https://explorer.burble.com/#/route/172.22.122.128_27)

DN42 IPv6:[`fdee:9bff:b001::/48`](https://explorer.burble.com/#/route6/fdee:9bff:b001::_48)

Domain:[`0xptr.dn42`](https://explorer.burble.com/#/domain/0xptr.dn42)

Looking glass:[`lg-dn42`](https://lg-dn42.0xptr.top)

IPV6 本地链路地址: `fe80::d555`

Wireguard 监听端口：你的 ASN 的后五位

## 联系方式

[`VIRTUAL0PTR-DN42`](https://explorer.burble.com/#/person/VIRTUAL0PTR-DN42)

## 注意事项

+  (目前)仅支持Wireguard隧道
+  我偏好使用MP-BGP与ENH(但是可选)
+  没有境内节点，故不接受境内对等
+  `如果网络炸了请告我一声`

对等时您需要使用以下模板:

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

## 我的节点

1. **HK-01**
  + 端点: hk-01.node.dn42.0xptr.top
  + DN42地址:
    + 172.22.122.129
    + fdee:9bff:b001::1
    + fe80::d555
  + Wireguard公钥:\
  xin3e9mT1MVzPUVGG7pVIZ\
  /FIkwOIGQNN2cVpCTLNS8=
  + 带宽:  1 Gbps([`Communities`](https://dn42.dev/howto/BGP-communities) 可能要用)

