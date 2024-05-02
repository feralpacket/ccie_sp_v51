# MPLS Layer 2 VPN (MPLS L2VPN) - Class Notes

**MPLS Layer 2 VPN (MPLS L2VPN)** (20 Sept 2014) 

Lab: EoMPLS, PPP_ETHERNET, PPP_ETHERNET

- Point-to-point

     -> Any Transport over MPLS (AToM)

     -> L2TPv3

 - Point-to-multipoint

     -> Virtual Private LAN Service (VPLS)

 - Used to transport layer 2 payloads transparently

     -> HDLC

     -> PPP

     -> ATM

     -> Frame-relay

     -> Ethernet

<insert picture from page 456>

L2VPN Label

 - Label exchange between PE routers

Transport Label

 - Label to reach next-hop

Targeted LDP

 - Multihop LDP session

     -> Uses LDP router-ids to form TCP connection

 - Pseudowire-id

     -> 32 bit number which must match on both PE routers for a single connection

int fa0/0

     -> Attachment circuit

 xconnect <remote LDP router-id> <pseudowire-id> encapsulation mpls

**Ethernet Configuration**

R1(config)# int fa0/0

 no ip add

 xconnect 3.3.3.3 100 encapsulation mpls

R3(config)# int fa0/0

 no ip add

 xconnect 1.1.1.1 100 encapsulation mpls

sh mpls l2transport vc <id> [detail]

**PPP Configuration**

R1(config)# int s0/1

 encap ppp

 no ip add

 xconnect 3.3.3.3 100 encapsulation mpls

R3(config)# int s0/1

 encap ppp

 no ip add

 xconnect 1.1.1.1 100 encapsulation mpls

**HDLC Configuration**

R1(config)# int s0/1

 encap hdlc

 no ip add

 xconnect 3.3.3.3 encapsulation mpls

R3(config)# int s0/1

 encap hdlc

 no ip add

 xconnect 1.1.1.1 encapsultation mpls

**FRoAToM**

 - Frame-relay over AToM

R1(config)# int s0/1

 no ip add

 encap frame-relay

 frame-relay intf-type dce

 exit

int s0/1 100 l2transport

     -> 100 = DLCI

 xconnect 3.3.3.3 200 encapsulation

     -> 200 = pseudowire-id

R4(config)# int s0/0

 encap frame-relay

 ip add 10.0.0.4 255.0.0.0

 frame-relay ip 10.0.0.5 100 broadcast

     -> 100 = DLCI

     -> broadcast (optional)

**Pseudowire Class**

 - Set of AToM / L2TPv3 specific parameters which are applied to xconnect

pseudowire-class <name>

 encapsulation { mpls | l2tpv3 }

 control-word

     -> Keeps properties of client and sent to the other end

     -> Similar to extended communities in BGP

     -> Used by some layer 2 protocols

          -> Frame-relay

               - FECN

               - BECN

               -DE-bits

          -> ATM

               - Cell loss priority (CLP)

int fa0/0

 xconnect <ip add> <pw-id> pw-class <name>
