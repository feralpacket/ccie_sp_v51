# IPV6 over IPV4 Tunneling - Class Notes

**IPv6 over IPv4 Tunneling** (28 Aug 2014)

Lab:  IPv6 1 and 2

![20141003_110553-1.jpeg](image/20141003_110553-1.jpeg)

R1

 - Source: 12.0.0.1

 - Destination: 23.0.0.3

R3

 - Source: 23.0.0.3

 - Destination: 12.0.0.1

Static tunnel

 - point-to-point tunnel

 - IPv6IP

     -> IPv6 payload only

     -> Smaller header

     -> IP protocol 41

 - GRE IP

     -> Multiprotocol support

     -> Larger header

     -> IP protocol 47

During the troubleshooting section of the R&S lab, there may be an ACL blocking IP protocol 41 or 47

R1(config)# <span style="background-color: #ffaaaa">int tu0</span>

 <span style="background-color: #ffaaaa">ip add 2001::1/64</span>

 <span style="background-color: #ffaaaa">tunnel source 12.0.0.1</span>

 <span style="background-color: #ffaaaa">tunnel destination 23.0.0.3</span>

 <span style="background-color: #ffaaaa">tunnel mode ipv6ip</span>

     -> The tunnel mode will be GRE, by default, if the tunnel mode is not specified
