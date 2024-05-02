# Layer 2 Tunneling Protocol Version 3 (L2TPv3) - Class Notes

**Layer 2 Tunneling Protocol Version 3 (L2TPv3)** (20 Sept 2014)Lab: EoMPLS, PPP_ETHERNET, PPP_ETHERNET

 - Uses IP protocol 115 to connect

     -> During the troubleshooting portion of the lab, look for ACLs that deny this protocol

 - Router-id used to connect must be specified manually

     -> source router-id

 - Encapsulation is L2TPv3

     -> No LPD / MPLS is required in the core

R1(config)# <span style="background-color: #ffaaaa">pseudowire-class ABC</span>

<span style="background-color: #ffaaaa"> encapsulation l2tpv3</span>

<span style="background-color: #ffaaaa"> ip local interface lo0</span>

<span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encap ppp</span>

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 100 pw-class ABC</span>

**AToM**

 - XR routers

     -> Ethernet

<span style="background-color: #ffaaaa">int g0/0/0/0</span>

<span style="background-color: #ffaaaa"> no ipv4 add</span>

<span style="background-color: #ffaaaa"> l2transport</span>

<span style="background-color: #ffaaaa"> root</span>

<span style="background-color: #ffaaaa">l2vpn</span>

<span style="background-color: #ffaaaa"> pw-class ABC</span>

<span style="background-color: #ffaaaa">  encapsulation mpls</span>

<span style="background-color: #ffaaaa"> xconnect group GROUP1</span>

<span style="background-color: #ffaaaa">  p2p R1-to-R3</span>

<span style="background-color: #ffaaaa">   int g0/0/0/0</span>

<span style="background-color: #ffaaaa">    neighbor 3.3.3.3 pw-id 100 pw-class ABC</span>

**Frame-relay**

<span style="background-color: #ffaaaa">int pos0/0/0/0.100 point-to-point</span>

<span style="background-color: #ffaaaa">     -> .100 = subinterface</span>

<span style="background-color: #ffaaaa"> pvc 100</span>

<span style="background-color: #ffaaaa">l2vpn</span>

<span style="background-color: #ffaaaa"> pw-class ABC</span>

<span style="background-color: #ffaaaa">  encapsulation mpls</span>

<span style="background-color: #ffaaaa"> xconnect group GROUP1</span>

<span style="background-color: #ffaaaa">  p2p R1-to-R3</span>

<span style="background-color: #ffaaaa">   int pos0/0/0/0.100</span>

<span style="background-color: #ffaaaa">    neighbor 3.3.3.3 pw-id 100 pw-class ABC</span>

**L2TPv3**

 - XR routers

<span style="background-color: #ffaaaa">int g0/0/0/0</span>

<span style="background-color: #ffaaaa"> no ipv4 add</span>

<span style="background-color: #ffaaaa"> l2transport</span>

<span style="background-color: #ffaaaa">l2vpn</span>

<span style="background-color: #ffaaaa"> pw-class ABC</span>

<span style="background-color: #ffaaaa">  encapsulation l2tpv3</span>

<span style="background-color: #ffaaaa">  protocol l2tpv3</span>

<span style="background-color: #ffaaaa">   ipv4 source 1.1.1.1</span>

<span style="background-color: #ffaaaa"> xconnect group GROUP1</span>

<span style="background-color: #ffaaaa">  p2p R1-to-R3</span>

<span style="background-color: #ffaaaa">   int g0/0/0/0</span>

<span style="background-color: #ffaaaa">    neighbor 3.3.3.3 pw-id 100 pw-class ABC</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">

</span>
