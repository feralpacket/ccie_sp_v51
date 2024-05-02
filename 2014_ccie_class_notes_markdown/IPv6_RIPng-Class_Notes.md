# IPv6 RIPng - Class Notes

**IPv6 RIPng** (28 Aug 2014)Lab:  IPv6 1 and 2

 - RIP Next Generation

 - RIPv2

 - Classless

 - Supports authentication

 - Multicast FF02::9

 - Timers are the same as RIPv2

 - Network command is no longer present

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 rip <name or number> enable</span>

     -> The name or number is locally significant

<span style="background-color: #ffaaaa">int lo0</span>

 <span style="background-color: #ffaaaa">ipv6 rip CISCO enable</span>

<span style="background-color: #ffaaaa">sh ipv6 route</span>

<span style="background-color: #ffaaaa">sh ipv6 rip CISCO</span>

<span style="background-color: #ffaaaa">sh ipv6 protocol</span>

**Summarization**

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 rip CISCO summary <address/nm></span>

**Default Routing**

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 rip CISCO default-information originate [only]</span>

only - injects a default route and suppresses everything else

**Filtering**

 - Only supports prefix-lists

<span style="background-color: #ffaaaa">ipv6 prefix-list <name> permit | deny <address/nm></span>

<span style="background-color: #ffaaaa">ipv6 router rip CISCO</span>

 <span style="background-color: #ffaaaa">distribute-list prefix <name> in | out <interface></span>
