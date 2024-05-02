# Multicast Anycast RP - Class Notes

**Multicast Anycast RP** (1 Sept 2014)Lab: Multicast 1 - 4

 - Allows the load balancing between RPs based on IGP metric (proximity) instead of groups

![20141015_085806-1.jpeg](image/20141015_085806-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">int lo1</span>

<span style="background-color: #ffaaaa"> ip add 10.0.0.1 255.255.255.255</span>

<span style="background-color: #ffaaaa">ip pim rp-candidate lo1</span>

<span style="background-color: #ffaaaa">ip pim bsr-candidate lo1</span>

<span style="background-color: #ffaaaa">router eigrp 100</span>

<span style="background-color: #ffaaaa"> network 10.0.0.1 0.0.0.0</span>

<span style="background-color: #ffaaaa">ip msdp peer 4.4.4.4 connect-source lo0</span>

R4(config)# <span style="background-color: #ffaaaa">int lo1</span>

<span style="background-color: #ffaaaa"> ip add 10.0.0.1 255.255.255.255</span>

<span style="background-color: #ffaaaa">ip pim rp-candidate lo1</span>

<span style="background-color: #ffaaaa">ip pim bsr-candidate lo1</span>

<span style="background-color: #ffaaaa">router eigrp 100</span>

<span style="background-color: #ffaaaa"> network 10.0.0.1 0.0.0.0</span>

<span style="background-color: #ffaaaa">ip msdp peer 1.1.1.1 connect-source lo0</span>

<span style="background-color: #ffaaaa">ip msdp originator-id lo0</span>

     -> **Needed on one side, otherwise the other RP will drop the MSDP packets**
