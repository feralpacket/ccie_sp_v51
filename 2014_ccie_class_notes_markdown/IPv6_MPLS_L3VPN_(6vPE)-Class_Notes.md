# IPv6 MPLS L3VPN (6vPE) - Class Notes

**IPv6 MPLS L3VPN (6vPE)** (15 Sept 2014)

Lab: 6vPE

- with VRFs on PE routers

- VPNv6 address-family between PE - PE routers

- IPv6 address-family between PE - CE routers

RD + IPv6 = VPNv6

64 bits + 128 bits = 192 bits

**VPNv6**

- VRF required on PE - CE

- VPN label exchange is automatic

![20141010_141005-1.jpeg](image/20141010_141005-1.jpeg)

**6vPE on IOS Routers**

1. VRF creation

2. Interface association

3. VRF IGP association

4. Mutual redistribution between VRF IGP and MP-BGP

R1(config)# v<span style="background-color: #ffaaaa">rf definition ABC</span>

<span style="background-color: #ffaaaa">rd 1:1</span>

<span style="background-color: #ffaaaa">address-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">  route-target both 1:1</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa">vrf forwarding ABC</span>

<span style="background-color: #ffaaaa">ip add 14.0.0.1 255.0.0.0</span>

<span style="background-color: #ffaaaa">ipv6 router ospf 1 vrf ABC</span>

<span style="background-color: #ffaaaa">exit</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa">ipv6 ospf 1 area 0</span>

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa">no bgp default ipv4-unicast</span>

<span style="background-color: #ffaaaa">neighbor 3.3.3.3 remote-as 100</span>

<span style="background-color: #ffaaaa">neighbor 3.3.3.3 update-source lo0</span>

<span style="background-color: #ffaaaa">address-family vpnv6 unicast</span>

<span style="background-color: #ffaaaa">  neighbor 3.3.3.3 activate</span>

<span style="background-color: #ffaaaa">address-family ipv6 vrf ABC</span>

<span style="background-color: #ffaaaa">  redistribute ospf 1</span>

<span style="background-color: #ffaaaa">ipv6 router ospf 1</span>

<span style="background-color: #ffaaaa">redistribute bgp 100</span>

**6vPE on XR Routers**

R1(config)# <span style="background-color: #ffaaaa">vrf ABC</span>

<span style="background-color: #ffaaaa">address-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">  import route-target 1:1</span>

<span style="background-color: #ffaaaa">  export route-target 1:1</span>

<span style="background-color: #ffaaaa">int g0/0/0/0</span>

<span style="background-color: #ffaaaa">no ipv6 add</span>

<span style="background-color: #ffaaaa">vrf ABC</span>

<span style="background-color: #ffaaaa">ipv6 add <ip add>/<nm></span>

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa">address-family vpnv6 unicast</span>

<span style="background-color: #ffaaaa">neighbor 3.3.3.3</span>

<span style="background-color: #ffaaaa">  remote-as 100</span>

<span style="background-color: #ffaaaa">  update-source lo0</span>

<span style="background-color: #ffaaaa">  address-family vpnv6 unicast</span>

<span style="background-color: #ffaaaa">vrf ABC</span>

<span style="background-color: #ffaaaa">  rd 1:1</span>

<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">neighbor 2002:14::4</span>

<span style="background-color: #ffaaaa">  remote-as 50</span>

<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">  route-policy PASS in</span>

<span style="background-color: #ffaaaa">  route-policy PASS out</span>

<span style="background-color: #ffaaaa">  as-override</span>

![Screenshot_(8).png](image/Screenshot_(8).png)

**! R1**
<span style="background-color: #ffaaaa">router bgp 50</span>
<span style="background-color: #ffaaaa"> bgp router-id 1.1.1.1</span>
<span style="background-color: #ffaaaa"> address-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  network 2002:1:1:1::1/128</span>
<span style="background-color: #ffaaaa"> neighbor 2002:12::2</span>
<span style="background-color: #ffaaaa">  remote-as 100

  address-family ipv6 unicast

   route-policy allow-all in

   route-policy allow-all out

route-policy allow-all

 pass

 end</span>

**! R2**
<span style="background-color: #ffaaaa">router ospf 1

 router-id 2.2.2.2

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
!
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto-config</span>
!
<span style="background-color: #ffaaaa">vrf ABC

 address-family ipv6 unicast

  export route-target 1:1</span>
<span style="background-color: #ffaaaa">  import route-target 1:1</span>
<span style="background-color: #ffaaaa">int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> no ipv4 address 9.9.12.2 255.255.255.0

 no ipv6 address 2002:9:9:12::2/64

 vrf ABC

 ipv4 address 9.9.12.2 255.255.255.0

 ipv6 address 2002:9:9:12::2/64

router bgp 100

 vrf ABC

   rd 1:1

</span>!

<span style="background-color: #ffaaaa">router bgp 100

 bgp router-id 9.9.0.2

 address-family vpnv6 unicast

</span> ! -> needed before “address-family ipv6 unicast” can be configured inside “vrf ABC”

<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">   network 2002:2:2:2::2/128</span>
<span style="background-color: #ffaaaa">  neighbor 2002:12::1</span>
<span style="background-color: #ffaaaa">   remote-as 50

   address-family ipv6 unicast

    route-policy allow-all in

    route-policy allow-all out

    as-override

route-policy allow-all

 pass

 end</span>
!
<span style="background-color: #ffaaaa">router bgp 100

 neighbor 3.3.3.3

  remote-as 100

  update-source lo0</span>
<span style="background-color: #ffaaaa">  address-family vpnv6 unicast</span>

**! R3**
<span style="background-color: #ffaaaa">int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0

router ospf 1

 router-id 3.3.3.3</span>
!
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router-id lo0 force

router ospf 1

 mpls ldp autoconfig</span>

**! R4**
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router-id 4.4.4.4</span>
<span style="background-color: #ffaaaa"> area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
!
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1</span>
<span style="background-color: #ffaaaa"> mpls ldp auto-config</span>

**! R5**
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router-id 5.5.5.5</span>
<span style="background-color: #ffaaaa"> area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
!

<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto-config</span>
!
<span style="background-color: #ffaaaa">vrf ABC

 address-family ipv6 unicast

  export route-target 1:1</span>
<span style="background-color: #ffaaaa">  import route-target 1:1</span>
<span style="background-color: #ffaaaa">int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> no ipv6 address 2002:56::5/64</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa"> ipv6 address 2002:56::5/64</span>
<span style="background-color: #ffaaaa">router bgp 100

 vrf ABC

   rd 1:1</span>
!
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> bgp router-id 5.5.5.5</span>
<span style="background-color: #ffaaaa"> address-family vpnv6 unicast</span>
! -> needed before “address-family ipv6 unicast” can be configured inside “vrf ABC”

<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  neighbor 200256::6</span>
<span style="background-color: #ffaaaa">   remote-as 50

   address-family ipv6 unicast

    route-policy allow-all in

    route-policy allow-all out

    as-override

route-policy allow-all

 pass

 end</span>
!
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>
<span style="background-color: #ffaaaa">  remote-as 100

  update-source lo0

  address-family vpnv6 unicast</span>

**! R6**
<span style="background-color: #ffaaaa">router bgp 50</span>
<span style="background-color: #ffaaaa"> bgp router-id 6.6.6.6</span>
<span style="background-color: #ffaaaa"> no bgp default ipv4-unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2002:56::5 remote-as 100</span>
<span style="background-color: #ffaaaa"> address-family ipv6</span>
<span style="background-color: #ffaaaa">  network 2002:6:6:6::6/128</span>
<span style="background-color: #ffaaaa">  neighbor 2002:56::5 activate</span>
