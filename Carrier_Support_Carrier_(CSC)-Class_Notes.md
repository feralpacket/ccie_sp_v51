# Carrier Support Carrier (CSC) - Class Notes

**\ Carrier Support Carrier (CSC)** (16 Sept 2014)

Lab: Carrier Supporting Carrier

![20141013_084654-1.jpeg](image/20141013_084654-1.jpeg)

ISP1

 - Main ISP

     -> Core ISP

ISP2

 - Second level ISP

     -> Tier 2 ISP

Clients

 - Connected to ISP2

1. ISP1 provides L3VPN connection to ISP2

2. Because of this connection, ISP2 PE routers can ping each other loopback to loopback

3. VPNv4 neighbors between ISP2 PE routers require end-to-end Labeled Switch Path (LSP)

4. The LSP is missing on VRF links of ISP1

     -> CSC-PE to CSC-CE

5. To activate LSP, there are two options

 - If CSC-PE to  CSC-CE protocol is IGP

     -> IGP + label

 - If CSC-PE to CSC-CE protocol is eBGP

     -> eBGP + label

6. For VRF interfaces, if LSP is required, then the only option on XR routers is eBGP

**Configuring CSC**

 1. Configure ISP1 with L3VPN

 2. Enable communication between ISP2 PE routers

 3. Configure "label processing" between ISP1 and ISP2

 4. Configure L3VPN between ISP2 PE routers

 5. Enable communication between client routers by CSC-CE to CE configuration

 6. In case ISP1 and ISP2 has XR routers, then two things to remember

 - Static route is needed for next-hop IP address between ASBRs

 - Only eBGP supported as VRF + label protocol

![Screenshot_(15).png](image/Screenshot_(15).png)

**! R1**

<span style="background-color: #ffaaaa">router rip

 version 2</span>

<span style="background-color: #ffaaaa"> network 1.0.0.0</span>

<span style="background-color: #ffaaaa">

</span>

**! R2**

<span style="background-color: #ffaaaa">router bgp 200</span>

<span style="background-color: #ffaaaa"> address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">  network 2.2.2.2/32</span>

<span style="background-color: #ffaaaa">  allocate-label all</span>

<span style="background-color: #ffaaaa"> neighbor 23.0.0.3</span>

<span style="background-color: #ffaaaa">  remote-as 100

  address-family ipv4 labeled-unicast

   route-policy allow-all in

   route-policy allow-all out

route-policy allow-all

 pass

 end

router static</span>

<span style="background-color: #ffaaaa"> address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">  23.0.0.3/32 gi0/0/0/1</span>

!

<span style="background-color: #ffaaaa">router bgp 200</span>

<span style="background-color: #ffaaaa"> address-family vpnv4 unicast</span>

<span style="background-color: #ffaaaa"> neighbor 7.7.7.7</span>

<span style="background-color: #ffaaaa">  remote-as 200

  update-source lo0

  address-family vpnv4 unicast</span>

!

<span style="background-color: #ffaaaa">vrf DEF

 address-family ipv4 unicast

  export route-target 2:2

  import route-target 2:2

router bgp 200

 vrf DEF</span>

<span style="background-color: #ffaaaa">  rd 2:2</span>

<span style="background-color: #ffaaaa">int gi0/0/0/0</span>

<span style="background-color: #ffaaaa"> no ipv4 address 12.0.0.2 255.255.255.0</span>

<span style="background-color: #ffaaaa"> vrf DEF</span>

<span style="background-color: #ffaaaa"> ipv4 address 12.0.0.2 255.255.255.0</span>

!

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> vrf DEF</span>

<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>

<span style="background-color: #ffaaaa">  redistribute bgp 200

router bgp 200

 vrf DEF

  address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">   redistribute rip</span>

<span style="background-color: #ffaaaa">

</span>

**! R3**

<span style="background-color: #ffaaaa">mpls ldp</span>

<span style="background-color: #ffaaaa">router ospf 1</span>

<span style="background-color: #ffaaaa"> router-id 3.3.3.3</span>

<span style="background-color: #ffaaaa"> mpls ldp auto-config

 area 0

  int lo0

  int gi0/0/0/1</span>

!

<span style="background-color: #ffaaaa">vrf ABC

 address-family ipv4 unicast

  export route-target 1:1

  import route-target 1:1</span>

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> bgp router-id 3.3.3.3</span>

<span style="background-color: #ffaaaa"> vrf ABC</span>

<span style="background-color: #ffaaaa">  rd 1:1</span>

<span style="background-color: #ffaaaa">int gi0/0/0/0</span>

<span style="background-color: #ffaaaa"> no ipv4 address 23.0.0.3 255.255.255.0</span>

<span style="background-color: #ffaaaa"> vrf ABC</span>

<span style="background-color: #ffaaaa"> ipv4 address 23.0.0.3 255.255.255.0</span>

!

<span style="background-color: #ffaaaa">router bgp 100

 address-family vpnv4 unicast

 vrf ABC

  address-family ipv4 unicast

   redistribute connected</span>

<span style="background-color: #ffaaaa">   allocate-label all</span>

<span style="background-color: #ffaaaa">  neighbor 23.0.0.2</span>

<span style="background-color: #ffaaaa">   remote-as 200

   address-family ipv4 labeled-unicast

    route-policy allow-all in

    route-policy allow-all out

    as-override

route-policy allow-all

 pass

 end

router static

 vrf ABC</span>

<span style="background-color: #ffaaaa">  address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">   23.0.0.2/32 gi0/0/0/1</span>

!

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> neighbor 6.6.6.6

  remote-as 100

  update-source lo0

  address-family vpnv4 unicast</span>

**! R4**

<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 router-id 4.4.4.4

 mpls ldp auto-config

 area 0</span>

<span style="background-color: #ffaaaa">  int lo0</span>

<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>

<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>

**! R5**

<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0</span>

<span style="background-color: #ffaaaa"> ip ospf 1 area 0</span>

<span style="background-color: #ffaaaa">int e1/1</span>

<span style="background-color: #ffaaaa"> ip ospf 1 area 0</span>

<span style="background-color: #ffaaaa">router ospf 1</span>

<span style="background-color: #ffaaaa"> router-id 5.5.5.5</span>

<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>

**! R6**

<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router-id lo0 force

int lo0</span>

<span style="background-color: #ffaaaa"> ip ospf 1 area 0</span>

<span style="background-color: #ffaaaa">int e1/0</span>

<span style="background-color: #ffaaaa"> ip ospf 1 area 0

router ospf 1

 router-id 6.6.6.6

 mpls ldp autoconfig</span>

!

<span style="background-color: #ffaaaa">vrf definition ABC

 rd 1:1

 address-family ipv4

  route-target both 1:1

int e1/1

 vrf forwarding ABC</span>

<span style="background-color: #ffaaaa"> ip address 67.0.0.6 255.255.255.0</span>

!

<span style="background-color: #ffaaaa">router bgp 100

 bgp router-id 6.6.6.6

 no bgp default ipv4-unicast

 address-family ipv4 vrf ABC</span>

<span style="background-color: #ffaaaa">  neighbor 67.0.0.7 remote-as 200</span>

<span style="background-color: #ffaaaa">  neighbor 67.0.0.7 activate</span>

<span style="background-color: #ffaaaa">  neighbor 67.0.0.7 as-override</span>

<span style="background-color: #ffaaaa">  neighbor 67.0.0.7 send-label</span>

<span style="background-color: #ffaaaa">  redistribute connected</span>

!

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> neighbor 3.3.3.3 remote-as 100</span>

<span style="background-color: #ffaaaa"> neighbor 3.3.3.3 update-source lo0</span>

<span style="background-color: #ffaaaa"> address-family vpnv4</span>

<span style="background-color: #ffaaaa">  neighbor 3.3.3.3 activate</span>

<span style="background-color: #ffaaaa">  neighbor 3.3.3.3 send-community extended</span>

**! R7**

<span style="background-color: #ffaaaa">router bgp 200</span>

<span style="background-color: #ffaaaa"> address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">  network 7.7.7.7/32</span>

<span style="background-color: #ffaaaa">  allocate-label all</span>

<span style="background-color: #ffaaaa"> neighbor 67.0.0.6</span>

<span style="background-color: #ffaaaa">  remote-as 100

  address-family ipv4 labeled-unicast

   route-policy allow-all in

   route-policy allow-all out

route-policy allow-all

 pass

 end

router static</span>

<span style="background-color: #ffaaaa"> address-family ipv4 unicast</span>

<span style="background-color: #ffaaaa">  67.0.0.6/32 gi0/0/0/1</span>

!

<span style="background-color: #ffaaaa">router bgp 200</span>

<span style="background-color: #ffaaaa"> address-family vpnv4 unicast</span>

<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>

<span style="background-color: #ffaaaa">  remote-as 200

  update-source lo0

  address-family vpnv4 unicast</span>

!

<span style="background-color: #ffaaaa">vrf DEF

 address-family ipv4 unicast

  export route-target 2:2

  import route-target 2:2

router bgp 200

 vrf DEF</span>

<span style="background-color: #ffaaaa">  rd 2:2</span>

<span style="background-color: #ffaaaa">int gi0/0/0/0</span>

<span style="background-color: #ffaaaa"> no ipv4 address 78.0.0.7 255.255.255.0</span>

<span style="background-color: #ffaaaa"> vrf DEF</span>

<span style="background-color: #ffaaaa"> ipv4 address 78.0.0.7 255.255.255.0</span>

!

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> vrf DEF</span>

<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>

<span style="background-color: #ffaaaa">  redistribute bgp 200

router bgp 200

 vrf DEF

  address-family ipv4 unicast

   redistribute rip</span>

**! R8**

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> version 2</span>

<span style="background-color: #ffaaaa"> network 78.0.0.0</span>

<span style="background-color: #ffaaaa"> network 8.0.0.0</span>

**Verification:**

**! R1**

<span style="background-color: #ffaaaa">sh rip database</span>

<span style="background-color: #ffaaaa">sh ip route rip</span>

<span style="background-color: #ffaaaa">ping 8.8.8.8</span>

**! R2**

<span style="background-color: #ffaaaa">sh bgp ipv4 labeled-unicast sum</span>

<span style="background-color: #ffaaaa">sh bgp ipv4 labeled-unicast</span>

<span style="background-color: #ffaaaa">sh route bgp</span>

<span style="background-color: #ffaaaa">sh route static</span>

<span style="background-color: #ffaaaa">ping 7.7.7.7</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u sum</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u</span>

<span style="background-color: #ffaaaa">sh vrf DEF detail</span>

<span style="background-color: #ffaaaa">sh rip vrf DEF database</span>

**! R3**

<span style="background-color: #ffaaaa">sh ospf int bri</span>

<span style="background-color: #ffaaaa">sh ospf nei</span>

<span style="background-color: #ffaaaa">sh route ospf</span>

<span style="background-color: #ffaaaa">sh mpls int</span>

<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>

<span style="background-color: #ffaaaa">sh mpls ldp dis</span>

<span style="background-color: #ffaaaa">sh vrf ABC detail</span>

<span style="background-color: #ffaaaa">sh bgp vrf ABC ipv4 labeled-unicast sum</span>

<span style="background-color: #ffaaaa">sh bgp vrf ABC ipv4 labeled-unicast</span>

<span style="background-color: #ffaaaa">sh route vrf ABC static</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u sum</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u</span>

<span style="background-color: #ffaaaa">sh route vrf ABC bgp</span>

<span style="background-color: #ffaaaa">sh mpls forwarding</span>

**! R4**

<span style="background-color: #ffaaaa">sh ospf int bri</span>

<span style="background-color: #ffaaaa">sh ospf nei</span>

<span style="background-color: #ffaaaa">sh route ospf</span>

<span style="background-color: #ffaaaa">sh mpls int</span>

<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>

<span style="background-color: #ffaaaa">sh mpls ldp dis</span><span style="background-color: #ffaaaa">

</span>

**! R5**

<span style="background-color: #ffaaaa">sh ip ospf int bri</span>

<span style="background-color: #ffaaaa">sh ip ospf nei</span>

<span style="background-color: #ffaaaa">sh ip route ospf</span>

<span style="background-color: #ffaaaa">sh mpls int</span>

<span style="background-color: #ffaaaa">sh mpls ldp nei</span>

<span style="background-color: #ffaaaa">sh mpls ldp dis</span><span style="background-color: #ffaaaa">

</span>

**! R6**

<span style="background-color: #ffaaaa">sh ip ospf int bri</span>

<span style="background-color: #ffaaaa">sh ip ospf nei</span>

<span style="background-color: #ffaaaa">sh ip route ospf</span>

<span style="background-color: #ffaaaa">sh mpls int</span>

<span style="background-color: #ffaaaa">sh mpls ldp nei</span>

<span style="background-color: #ffaaaa">sh mpls ldp dis</span><span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">sh vrf detail</span>

<span style="background-color: #ffaaaa">sh bgp vrf ABC all sum</span>

<span style="background-color: #ffaaaa">sh bgp vrf ABC all</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u all sum</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u all</span>

<span style="background-color: #ffaaaa">sh mpls forwarding</span>

**! R7**

<span style="background-color: #ffaaaa">sh bgp ipv4 labeled-unicast sum</span>

<span style="background-color: #ffaaaa">sh bgp ipv4 labeled-unicast</span>

<span style="background-color: #ffaaaa">sh route bgp</span>

<span style="background-color: #ffaaaa">sh route static</span>

<span style="background-color: #ffaaaa">ping 2.2.2.2</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u sum</span>

<span style="background-color: #ffaaaa">sh bgp vpnv4 u</span>

<span style="background-color: #ffaaaa">sh vrf DEF detail</span>

<span style="background-color: #ffaaaa">sh rip vrf DEF database</span>

**! R8**

<span style="background-color: #ffaaaa">sh rip database</span>

<span style="background-color: #ffaaaa">sh ip route rip</span>

<span style="background-color: #ffaaaa">ping 1.1.1.1</span>
