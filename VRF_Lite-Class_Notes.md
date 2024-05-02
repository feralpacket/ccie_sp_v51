# VRF Lite - Class Notes

**VRF Lite** (3 Sept 2014)Lab:  MPLS 1 - 3

 - VRF without MPLS

 - Causes problems if used on CE routers

![20141003_154542-1.jpeg](image/20141003_154542-1.jpeg)

![20141003_154552-1.jpeg](image/20141003_154552-1.jpeg)

The down-bit is set in OSPF update, so R4 drops the update.

Scenario -> One branch of the customer is using VRF lite with OSPF as the PE - CE protocol.  The other branch is not using VRF.

Problem, the VRF branch will not accept any updates from the other branch because of the down-bit.

Solution #1

 - Change the domain-id of any of the PE routers

     -> Routes will get exchanged as type 5 LSAs, the routes being External 2

R1(config)# <span style="background-color: #ffaaaa">router ospf 10 vrf c1b1</span>

 <span style="background-color: #ffaaaa">domain-id 0.0.0.15</span>

Domain-id -> preferable to "no router ospf 10 vrf c1b1", "router ospf <new process-id> vrf c1b1"

Solution #2

 - On the CE router, disable down-bit checking on the VRF

R4(config)# <span style="background-color: #ffaaaa">router ospf 1</span>

 <span style="background-color: #ffaaaa">capability vrf-lite</span>

capability vrf-lite

 - You are not a PE router, don't check the down-bit
