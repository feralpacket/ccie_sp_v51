# Multicast Bootstrap Router (BSR) - Class Notes

**Multicast Bootstrap Router (BSR)** (1 Sept 2014)
Lab: Multicast 1 - 4
 - Open standard
 - PIM version 2 messages
 - RP information is distributed by BSR
 - If there are multiple RPs, then priorities are checked
     -> Lower is better
 - If there are multiple BSRs, then higher priority is better
     -> Then higher IP address

**Hask-mask**
 - Sent by BSR
     -> A value between 0 - 32
     -> Default is 0

**Group Address and RP**
 - Value1 = f (Hash and group, RP1)
 - Value2 = f (Hash and group, RP2)

**Configuring RP**

<span style="background-color: #ffaaaa">ip pim rp-candidate <int> [group-list <acl>] [interval <sec>] [priority <value>]</span>

**Configuring BSR**
 
<span style="background-color: #ffaaaa">ip pim bsr-candidate <int> [interval <sec>] [<hask-mask-length>] [priority <value>]</span>

R2(config)# <span style="background-color: #ffaaaa">ip pim rp-candidate lo0</span>

R3(config)# <span style="background-color: #ffaaaa">ip pim bsr-candidate lo0</span>

**RP-Announce Filter**
 - Controls which routers can become RPs

Configuring MA

<span style="background-color: #ffaaaa">ip pim rp-announce-filter rp-list <acl> [group-list <acl>]</span>

 - <span style="background-color: #ffaaaa">rp-list <acl></span>
     -> Permit - filter the RP
     -> Deny - allow the RP
     -> Reverse logic

**Multicast Data Plane Filtering**

<span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> ip multicast rate-limit { in | out } [limit <kbps>] [source-list <acl>] group-list <acl>]</span>

![20141015_085709-1.jpeg](image/20141015_085709-1.jpeg)

**Auto-RP Filtering**
 - On boundary ports to other organizations, RP announce | discovery messages should be filtered and PIM should not be exchanged
     -> But you still may want to allow multicast traffic

R3(config)# <span style="background-color: #ffaaaa">int s0/0</span>
<span style="background-color: #ffaaaa"> ip multicast boundary <acl> [filter-autorp] [in | out]</span>

 - <span style="background-color: #ffaaaa"><acl></span>
     -> Group for which traffic will be controlled

By default, incoming control plane and outgoing data plane traffic is controlled

Instructor comment, "BSR is better method because is doesn't need dense mode."

**BSR Filtering on Boundary**
 - Specifically filters PIM traffic

<span style="background-color: #ffaaaa">int s0/0</span>
<span style="background-color: #ffaaaa"> ip pim bsr-boundary</span>

<span style="background-color: #ffaaaa">sh ip mroute</span>
     -> You enabled multicast routing, right?
<span style="background-color: #ffaaaa">sh ip pim int</span>
<span style="background-color: #ffaaaa">sh ip pim autorp</span>
<span style="background-color: #ffaaaa">sh ip pim rp mapping</span>
     -> "This system is an RP-mapping agent."
