# L2VPN Interworking - Class Notes

**L2VPN Interworking** (20 Sept 2014)

Lab: EoMPLS, PPP_ETHERNET, PPP_ETHERNET

 - Mechanism to connect two different layer 2 protocols over a L2VPN network

![20141014_102112-1.jpeg](image/20141014_102112-1.jpeg)

R1(config)# pweudowire-class ABC

 encapsulation mpls

 interworking ip

int s0/0

 encapsulation ppp

 xconnect 3.3.3.3 100 pw-class ABC

XR Router

l2vpn

 pw-class ABC

  encapsulation mpls

  interworking ip

 xconnect group GROUP1

  p2p R1-to-R3

   int g0/0/0/0

    neighbor 3.3.3.3 pw-id 100 pw-class ABC

Frame-relay special scenario

![20141014_102124-1.jpeg](image/20141014_102124-1.jpeg)

R2(config)# int s0/0

 encapsulation frame-relay

do sh frame-relay pvc

     -> To get the DLCI information

 frame-relay interface-dlci 200 switched

connect fr x0/0 200 l2transport

 xconnect 1.1.1.1 100 encapsulation mpls

During the lab, you will only be asked to configure VPLS on the XR routers

 - All other L2VPN configuration will be done on the IOS routers

     -> Virtual 7200 for IOS doesn't support VPLS

sh mpls l2transport vc <id> (IOS)

sh l2vpn xconnect [detail] (XR)

![20141014_102137-1.jpeg](image/20141014_102137-1.jpeg)

**VPLS**

 - It is a pure MPLS solution

  -> Not supported with other encapsulations

 - Is is a virtual switch connecting all client devices

 - PE - CE link must be ethernet

 - PE routers maintain MAC-address-table for client routes

 - The collection of all PE routers servicing to the same client is called a "bridge-domain"

 - Bridge-domain

  -> Virtual switch for a client

  -> Attachment circuits information

  -> Virtual Forwarding Instance ( VFI )

    -> List of PE neighbors

Note:  Didn't take notes on the configuration
