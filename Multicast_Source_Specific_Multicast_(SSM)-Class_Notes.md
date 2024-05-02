# Multicast Source Specific Multicast (SSM) - Class Notes

**Multicast Source Specific Multicast (SSM)** (1 Sept 2014)Lab: Multicast 1 - 4

![20141015_085733-1.jpeg](image/20141015_085733-1.jpeg)

**Configuring SSM**

 1. Activate PIM-SM everywhere

     -> Does not work with DM

 2. Configure every router for SSM

 3. On LHR, activate IGMPv3 on client facing interfaces

R1(config)# <span style="background-color: #ffaaaa">ip multicast-routing</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip pim sparse-mode</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip pim sparse-mode</span>

<span style="background-color: #ffaaaa">ip pim ssm { default | range <acl> }</span>

 - default

     ->232.x.x.x only

 - <acl>

     -> Any other group specified

R1(config)# <span style="background-color: #ffaaaa">access-list 1 permit 225.1.1.1</span>

<span style="background-color: #ffaaaa">ip pim ssm range 1</span>

R3(config)# <span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip igmp version 3</span>

<span style="background-color: #ffaaaa"> ip igmp join-group 225.1.1.1 source 1.1.1.1</span>

     -> To configure the router as a receiver
