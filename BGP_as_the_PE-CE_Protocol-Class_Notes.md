# BGP as the PE - CE Protocol - Class Notes

**BGP as the PE - CE Protocol** (3 Sept 2014)

Lab:  MPLS 1 - 3

![20141003_152749-1.jpeg](image/20141003_152749-1.jpeg)

When R1 sends an update to R4 with R5's information, the update will include AS 50 in the AS-PATH.  R4 will see 50 in the AS-PATH and drop the update.  There are two solutions that will allow R4 to accept the update.

 - osi

     -> PE side solution

 - allowas-in

     -> CE side solution

R1(config)# **ip vrf c1b1**

** rd 1:1**

** route-target both 1:1**

**int s0/0**

** ip vrf forwarding c1b1**

**router bgp 100**

** neighbor 3.3.3.3 remote-as 100**

** neighbor 3.3.3.3 update-source lo0**

** address-family vpnv4 unicast**

**  neighbor 3.3.3.3 activate**

** address-familty ipv4 vrf c1b1**

**  neighbor 14.0.0.1 remote-as 50**

**  neighbor 14.0.0.1 as-override**

     -> PE side solution

R4(config)# **router bgp 50**

** neighbor 14.0.0.1 remote-as 100**

** neighbor 14.0.0.1 allowas-in**

     -> CE side solution

**sh bgp vpnv4 unicast all**

**sh bgp vpnv4 unicast all summary**

**sh bgp vpnv4 unicast all [ neighbor <ip address> | <ip address> ]**
