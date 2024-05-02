# L3VPN Loop Prevention Features - Class Notes

**L3VPN Loop Prevention Featurees** (3 Sept 2014)

Lab:  MPLS 1 - 3

**OSPF**

- Uses DOWN-BIT

     -> When a PE router receives an update on a VRF interface, it performs a DOWN-BIT operation

          -> If the down-bit is not set in the update packet, it is set by the PE router

          -> If the down-bit is set, the PE router drops the packet

          -> Down-bit checking is only done on type 3 LSAs (Inter-Area)

          -> Doesn't work for type 1 LSAs used by sham-links

          -> No configuration required

**BGP**

- Can use extended community called "site-of-origin"

     -> Can be set on incoming VRF interface

     -> SoO = 64 bit value represented at [X:Y](file:///X:Y%5D)

          -> Must be different on each PE router

R1(config)# <span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa">address-family ipv4 vrf c1b1</span>

  <span style="background-color: #ffaaaa">neighbor 14.0.0.4 soo 100:100</span>

R3(config)# <span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa">address-family ipv4 vrf c1b2</span>

  <span style="background-color: #ffaaaa">neighbor 35.0.0.5 soo 200:200</span>

![20141003_154519-1.jpeg](image/20141003_154519-1.jpeg)

SoO value should be the same on PE routers connected to the same branch.

**EIGRP**

 - Can use "site-of-origin" to prevent loops

R1(config)# <span style="background-color: #ffaaaa">router-map SOO</span>

 <span style="background-color: #ffaaaa">set ext-community soo 100:100</span>

<span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ip vrf sitemap SOO</span>

Sitemap

 - If SoO is not set, then set

 - If SoO is set, then drop
