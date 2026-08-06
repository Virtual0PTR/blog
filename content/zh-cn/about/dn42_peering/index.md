+++
title = "DN42对等信息"
date = 2025-02-02T17:25:00+08:00
draft = false
+++

## 基本信息 {#基本信息}

-   ASN: [AS4242420555](https://explorer.burble.com/#/aut-num/AS4242420555)
-   DN42 IPv4: [172.22.122.128/27](https://explorer.burble.com/#/route/172.22.122.128_27)
-   DN42 IPv6: [fdee:9bff:b001::/48](https://explorer.burble.com/#/route6/fdee:9bff:b001::_48)
-   Domain: [0xptr.dn42](https://explorer.burble.com/#/domain/0xptr.dn42)
-   Looking glass: [lg-dn42](https://lg-dn42.0xptr.top/)
-   IPV6 本地链路地址: fe80::d555
-   Wireguard 监听端口: 你的 ASN 的后五位


## 联系方式 {#联系方式}

[VIRTUAL0PTR-DN42](https://explorer.burble.com/#/person/VIRTUAL0PTR-DN42)


## 注意事项 {#注意事项}

-   (目前)仅支持Wireguard隧道
-   我偏好使用MP-BGP与ENH(但是可选)
-   没有境内节点，故不接受境内对等
-   _如果网络炸了请告我一声_

对等时您需要使用以下模板:

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


## 我的节点 {#我的节点}

-   HK-01
    -   端点: hkg-hk-1.node.0xptr.top
    -   DN42地址:
        -   172.22.122.129
        -   fdee:9bff:b001::1
        -   fe80::d555
    -   Wireguard公钥: xin3e9mT1MVzPUVGG7pVIZ/FIkwOIGQNN2cVpCTLNS8=
    -   带宽: 1 Gbps(Communities 可能要用)

{{< details summary="[hugo build log]" >}}
~~呜哇，还真有人打开啊!~~ <br />

后记:                 <br />
尽管个人精力有限，维护也不很勤快，不过…… <br />
我大抵是不会让 AS4242420555 从路由表里消失的 <br />
毕竟 IANA 是[很现实而又商业化的](https://zerowolf.cn/bgp/)，而[DN42 却不尽然](https://0x7f.cc/what-is-dn42/)，它更像是架在现实网络之上的另一层世界 <br />
既然好不容易找到了这样一个地方，不折腾一下、多学点东西，岂不是亏大发了 <br />
无论什么时候，我都愿意被动成为各位的 对等数+1
{{< /details >}}
