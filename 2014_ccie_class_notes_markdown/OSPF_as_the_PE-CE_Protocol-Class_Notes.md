# OSPF as the PE - CE Protocol - Class Notes

**OSPF as the PE - CE Protocol** (3 Sept 2014)

Lab:  MPLS 1 - 3

![20141003_154406-1.jpeg](image/20141003_154406-1.jpeg)

Configuration assumes IGP + LDP is complete, MP-BGP is complete, and CE IGP is complete

R1(config)# <span style="background-color: #ffaaaa">router ospf 10 vrf c1b1</span>

 <span style="background-color: #ffaaaa">network 14.0.0.1 0.0.0.0 area 0</span>

 <span style="background-color: #ffaaaa">redistribute bgp 100 subnets</span>

R3(config)# <span style="background-color: #ffaaaa">router ospf 10 vrf c1b2</span>

 <span style="background-color: #ffaaaa">network 35.0.0.3 0.0.0.0 area 0</span>

 <span style="background-color: #ffaaaa">redistribute bgp 100 subnets</span>

R1(config)# <span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">address-family ipv4 vrf c1b1</span>

  <span style="background-color: #ffaaaa">redistribute ospf 100 match internal external 1 external 2</span>

R3(config)# <span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">address-familty ipv4 vrf c1b2</span>

  <span style="background-color: #ffaaaa">redistribute ospf 10 match internal external 1 external 2</span>

sh ip route

 On R4 -> O IA 5.5.5.5

 On R5 -> O IA 4.4.4.4

If the OSPF process number on the CE routers do not match, the routes will be external.

 - The PE router's OSPF process-id is carried into BGP as the "domain-id" extended community to the other PE router

 - The domain-id is compared with the local process-id

     -> If they match, the OSPF routes will be Inter-Area

     -> If the do not match, the OSPF routes will be external (E2)

![20141003_154419-1.jpeg](image/20141003_154419-1.jpeg)

The PE network acts as a super backbone area for the customer's OSPF.

Instructor comment, "The super backbone is like a higher, more intellignt area 0."

![20141003_154425-1.jpeg](image/20141003_154425-1.jpeg)

If the customer has area 0 anywhere on the network, it has to be directly connected with the super backbone

     -> If area 0 is not directly connected with the super backbone, virtual-links are required
