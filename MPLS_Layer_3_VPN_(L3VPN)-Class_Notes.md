# MPLS Layer 3 VPN (L3VPN) - Class Notes

**MPLS Layer 3 VPN (L3VPN)** (12 Sept 2014)

Lab: L3VPN

![20141003_113210-1.jpeg](image/20141003_113210-1.jpeg)

MP-BGP

- Multiprotocol Border Gateway Protocol

PE

- Provider Edge router

P

- Provider router

CE

- Customer Edge router

Control Plane

- PE - CE

     -> IGP or eBGP

- PE - PE

     -> MP-BGP

Data Plane

- When traffic will be sent, MPLS will be used

PE routers create one VRF (Virtual Routing and Forwarding) instance per customer

- VRF has a separate FIB table per customer

- VRF parameters

     -> Name of VRF table

     -> Route Distinguisher (RD)

          -> 64 bit value represented as <32 bit>:<32 bit>

          -> 50:60

          -> Used to make customer routes unique

RD + IPv4 address = VPNv4 router

     -> 64 bit + 32 bits = 96 bits
![20141003_113222-1.jpeg](image/20141003_113222-1.jpeg)

     -> Route Target (RT)

          -> 64 bit value represented as <32 bit>:<32 bit>

          -> To control the distination of routes, the concept of import and export is used
![20141003_113230-1.jpeg](image/20141003_113230-1.jpeg)

**Configuration on PE routers**

**Phase 1**

1. Create the VRF

2. Adding RD value

     -> Only one RD value per VRF

3. One or more RTs

     -> Import

     -> Export

4. Interface association with the VRF

5. PE - CE protocol association

R1 (config)# **ip vrf c1b1**

**rd 100:100**

**route-target import 100:100**

**route-target export 100:100**

**int s0/0**

**  ip vrf forwarding c1b1**

**  ip add 14.0.0.1 255.0.0.0**

**sh ip route**

  C     12.0.0.0

  C     1.0.0.0

**sh ip route vrf c1b1**

  C     14.0.0.0

R1(config)# **router rip**

**address-family ipv4 vrf c1b1**

**  version 2**

**  no auto-summary**

**  network 14.0.0.0**

**sh ip route vrf c1b1**

  C     14.0.0.0

  R     4.4.4.4

Configure RIP on the CE routers

After Phase 1, the PE routers will have the local client routes
![20141003_113248-1.jpeg](image/20141003_113248-1.jpeg)

**Phase 2**

1. Configure MP-BGP on PE routers

2. Form VPNv4 neighbor relationship between PE routers using /32 loopback interfaces

3. Do mutual redistribution between the PE - CE IGP and the MP-BGP VRF-to-VRF

RD + IPv4 Address = VPNv4 route

 100:100:4.4.4.4 

R1(config)# r**outer bgp 100**

**neighbor 3.3.3.3 remote-as 100**

**neighbor 3.3.3.3 update-source lo0**

**address-family vpnv4 unicast**

**  neighbor 3.3.3.3 activate**

**  neighbor 3.3.3.3 send-community extended**

     -> This is automatically added to the configuration when the neighbor is activated

**  address-family ipv4 vrf c1b1**

**   redistribute rip**

Extended community is used to send and receive IGP parameters from one customer site to another customer site

R1(config)# **router rip**

**address-family ipv4 vrf c1b1**

**  redistribute bgp 100 metric { transparent | <value> }**

Transparent

  RIP -> BGP's MED -> BGP's MED -> RIP metric
![20141003_113258-1.jpeg](image/20141003_113258-1.jpeg)

BGP adds a VPN label when routes are redistributed into BGP

BGP VPN Label = 100 for 4.4.4.4
![20141003_113311-1.jpeg](image/20141003_113311-1.jpeg)

**sh mpls forwarding-table**

     -> To check the LFIB

**!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!**

**Note (7 April 2024): Everything below here was added later**

BGP

- IPv4

 - IPv6 

**XR Routers**

**router bgp <asn>**

** address-family <afi> <safi>**

     -> Address-family Identifier

          -> ipv4

               -> 32 bit

          -> ipv6

               -> 128 bit

          -> vpnv4

               -> RD + ipv4

               -> 64 bits + 32 bits = 96 bits

               -> Client IPv4 addresses

          -> vpnv6

               -> RD + ipv6

               -> 64 bits + 128 bits = 192 bits

               -> Client IPv6 addresses

     -> Subsequent Address-family Identifier

          -> unicast

          -> multicast

          -> mdt

               -> Multicast Distribution Tree

**router bgp <asn>**

**address-family <afi> <safi>**

**  network <ip add>/<nm>**

**  redistribute <protocol>**

**  exit**

**neighbor <ip add>**

**  remote-as <asn>**

**  update-source <int>**

**  ebgp-multihop <ttl>**

**address-family <afi> <safi>**

**  route-reflector-client**

**  next-hop-self**

**  route-policy <name> in | out**

**  as-override**

**  default-originate**

![20141010_140829-1.jpeg](image/20141010_140829-1.jpeg)

Configure BGP between R1 and R2 for IPv4 and IPv6

 - Advertise loopbacks

 - Disable the default address-familty

     -> This is commonly seen in the lab

R1(config)# **router bgp 100**

** no bgp default ipv4-unicast**

** neighbor 12.0.0.2 remote-as 100**

** neighbor 2002:12::2 remote-as 100**

** address-family ipv4 unicast**

**  neighbor 12.0.0.2 activate**

**  network 12.0.0.0 mask 255.0.0.0**

**  network 1.1.1.1 mask 255.255.255.255**

** address-family ipv6 unicast**

**  neighbor 2002:12::2 activate**

**  network 2002:12::/64**

**  network 2002:1:1:1::1/128**

R2(config)# **router bgp 100**

      -> On the XR router, the is no default address-family

** address-family ipv4 unicast**

**  network 2.2.2.2/32**

**  network 12.0.0.0/8**

** address-family ipv6 unicast**

**  network 2002:12::/8**

**  network 2002:2:2:2::2/128**

** neighbor 12.0.0.1**

**  remote-as 100**

**  address-family ipv4 unicast**

** neighbor 2002:12::1**

**  remote-as 100**

**  address-family ipv6 unicast**

**sh bgp <afi> <safi>**

**sh bgp ipv4 unicast**

**sh bgp ipv6 unicast**

eBGP

 - By default, no IPv4 or IPv6 updates can be exchanged

     -> A route-policy has to be applied to the incoming and outgoing directions inside the address-family

 - Create the route-policy in global config

**route-policy PASS**

** pass**

** exit**

 - Apply the route-policy to the address-family

**router bgp 100**

** neighbor <ip add>**

**  address-family <afi> <safi>**

**   route-policy PASS in**

**   route-policy PASS out**

![20141010_140838-1.jpeg](image/20141010_140838-1.jpeg)

![20141010_140858-1.jpeg](image/20141010_140858-1.jpeg)

L3VPN

 1. VRF creation

 2. PE - CE interface association

 3. IGP association ( PE - CE )

 4. MP-BGP ( PE - PE )

 5. Mutual redistribution between VRF IGP and MP-BGP

 - ISP Core

     -> IGP + LDP

XR Routers

R1(config)# **vrf ABC**

** address-family ipv4 unicast**

**  import route-target 1:1**

**  export route-target 1:1**

RD value is configured inside BGP on XR routers

 **- This is easy to forget**

VRF interface association

 - On XR routers, the IPv4 and IPv6 addresses must be removed before associating the interface to the VRF

R1(config)# **int g0/0/0/0**

** no ipv4 add**

** no ipv6 add**

** vrf ABC**

** ipv4 <ip add>/<nm>**

** ipv6 <ip add>/<nm>**

**router rip**

** vrf ABC**

**  int g0/0/0/0**

**router eigrp 100**

** vrf ABC**

**  address-family ipv4 unicast**

**   autonomous-system 100**

**   int g0/0/0/0**

**  address-family ipv6 unicast**

**   autonomous-system 100**

**   int g0/0/0/0**

**router ospf 1**

** vrf ABC**

**  area 0**

**   int g0/0/0/0**

**root**

**show config**

**commit**

MP-BGP

 - VPNv4 address-family

     -> PE - PE

 - Mutual redistribution with VRF IGP

R1(config)# **router bgp 100**

** address-family ipv4 unicast**

** address-family vpn4 unicast**

** neighbor 2.2.2.2**

**  remote-as 100**

**  update-source lo0**

**  address-family vpn4 unicast**

** vrf ABC**

**  rd 1:1**

     **-> This is easy to forget**

**  address-family ipv4 unicast**

**   redistribute ospf 1**

**router ospf 1**

** vrf ABC**

**  redistribute bgp 100**

**  area 0**

**  int g0/0/0/0**

During the SP lab, there will be 6 PE routers and 6 CE routers

If PE - CE protocol is BGP

R1(config)# **router bgp 100**

** address-family ipv4 unicast**

** address-family vpnv4 unicast**

** neighbor 2.2.2.2**

**  remote-as 100**

**  update-source lo0**

**  address-family vpnv4 unicast**

** vrf ABC**

**  rd 1:1**

**  address-family ipv4 unicast**

**   redistribute connected**

**  neighbor 13.0.0.3**

**   remote-as 50**

**   address-family ipv4 unicast**

**    route-policy PASS in**

**    route-policy PASS out**

**    as-override**

![Screenshot_(3).png](image/Screenshot_(3).png)

**! R1**

**router eigrp 1**

** address-family ipv4**

**  int lo0**

**  int gi0/0/0/1**

**! R2**

**router ospf 1**

** router-id 2.2.2.2**

** area 0**

**  int lo0**

**  int gi0/0/0/1**

**!**

**mpls ldp**

**router ospf 1**

** mpls ldp auto-config**

!

**router bgp 100**

** bgp router-id 2.2.2.2**

** address-family vpnv4 unicast**

** neighbor 5.5.5.5**

**  remote-as 100**

**  update-source lo0**

**  address-family vpnv4 unicast**

!

**vrf ABC**

** address-family ipv4 unicast**

**  import route-target 1:1**

**  export route-target 1:1**

**router bgp 100**

** vrf ABC**

**  rd 1:1**

**  address-family ipv4 unicast**

!

**int g0/0/0/0**

** no ipv4 address 12.0.0.2 255.255.255.0**

** vrf ABC**

** ipv4 address 12.0.0.2 255.255.255.0**

!

**router eigrp 1**

** vrf ABC**

**  address-family ipv4**

**   autonomous-system 1**

**   int gi0/0/0/0**

!

**router bgp 100**

** vrf ABC**

** address-family ipv4 unicast**

**  redistribute eigrp 1**

**router eigrp 1**

** vrf ABC**

** address-family ipv4**

**  redistribute bgp 100**

**! R3**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 3.3.3.3**

!

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int e1/0**

** mpls ip**

**int e1/1**

** mpls ip**

 

**! R4**

**router ospf 1**

** router-id 4.4.4.4**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

!

**mpls ldp**

**router ospf 1**

** mpls ldp auto-config**

**! R5**

**router ospf 1**

** router-id 5.5.5.5**

** area 0**

**  int lo0**

**  int gi0/0/0/1**

!

**mpls ldp**

**router ospf 1**

** mpls ldp auto-config**

!

**router bgp 100**

** bgp router-id 5.5.5.5**

** address-family vpnv4 unicast**

** neighbor 2.2.2.2**

**  remote-as 100**

**  update-source lo0**

**  address-family vpnv4 unicast**

!

**vrf ABC**

** address-family ipv4 unicast**

**  import route-target 1:1**

**  export route-target 1:1**

**router bgp 100**

** vrf ABC**

**  rd 1:1**

**  address-family ipv4 unicast**

!

**int gi0/0/0/0**

** no ipv4 address 35.0.0.3 255.255.255.0**

** vrf ABC**

** ipv4 address 35.0.0.3 255.255.255.0**

!

**router eigrp 1**

** vrf ABC**

**  address-family ipv4**

**   autonomous-system 1**

**   int gi0/0/0/0**

!

**router bgp 100**

** vrf ABC**

** address-family ipv4 unicast**

**  redistribute eigrp 1**

**router eigrp 1**

** vrf ABC**

** address-family ipv4**

**  redistribute bgp 100**

**! R6�**�

**router eigrp 1**

** network 6.6.6.6 0.0.0.0**

** network 56.0.0.6 0.0.0.0**

**Verification**

**! R1**

**sh eigrp int**

**sh eigrp nei**

**sh ip route**

**ping 6.6.6.6**

**! R2 and R5**

**sh ospf nei**

**sh route ospf**

**sh mpls int**

**sh mpls ldp nei bri**

**sh bgp vpnv4 u sum**

**sh bgp vpnv4 u**

**sh vrf ABC**

**sh eigrp vrf ABC nei**

**sh route vrf ABC eigrp**

**ping vrf ABC 1.1.1.1**

**ping vrf ABC 6.6.6.6**

**sh bgp vrf ABC**

**sh route vrf ABC bgp**

**sh eigrp vrf ABC topology**

**! R6**

**sh ip eigrp int**

**sh ip eigrp nei**

**sh ip route**

**ping 1.1.1.1**
