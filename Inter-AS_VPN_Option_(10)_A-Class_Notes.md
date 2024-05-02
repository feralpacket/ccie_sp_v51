# Inter-AS VPN Option (10) A - Class Notes

I**nter-AS VPN Option (10) A** (16 Sept 2014)

Lab: Inter-AS A, Inter-AS B, Inter-AS C

 - Back-to-back VRF

     -> The client's routes are exhchanged by ASBRs by configuring each other as VRF clients

          -> ASBR1 <--> ASBR2

     -> This solution can have scalability issues as one layer 3 interface / VRF will be required on the ASBRs

     -> The client routes are sent by the PE router to the ASBR router over VPNv4 neighbor connection

![20141012_172414-1.jpeg](image/20141012_172414-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">**vrf definition ABC**</span>

<span style="background-color: #ffaaaa">** rd 1:1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  route-target both 1:1**</span>

<span style="background-color: #ffaaaa">**int s0/0**</span>

<span style="background-color: #ffaaaa">** vrf forwarding ABC**</span>

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span>

<span style="background-color: #ffaaaa">**  network 14.0.0.1 0.0.0.0**</span>

<span style="background-color: #ffaaaa">**  redistribute bgp 100 metric 1 1 1 1 1**</span>

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 3.3.3.3 remote-as 100**</span>

<span style="background-color: #ffaaaa">** neighbor 3.3.3.3 update-source lo0**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">**  neighbor 3.3.3.3 activate**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span>

<span style="background-color: #ffaaaa">** redistribute eigrp 1**</span>

XR Router

R3(config)# <span style="background-color: #ffaaaa">**vrf ABC**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  import route-target 1:1**</span>

<span style="background-color: #ffaaaa">**  export route-target 1:1**</span>

<span style="background-color: #ffaaaa">**int s0/1**</span>

<span style="background-color: #ffaaaa">** no ipv4 add**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">** ipv4 add <ip add>**</span>

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**   autonomous-system 1**</span>

<span style="background-color: #ffaaaa">**   int s0/1**</span>

<span style="background-color: #ffaaaa">**   redistribute bgp 100**</span>

<span style="background-color: #ffaaaa">**    default-metric 1 1 1 1 1**</span>

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 1.1.1.1**</span>

<span style="background-color: #ffaaaa">**  remote-as 100**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">**  rd 1:1**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  redistribute eigrp 1**</span>

Rinse and repeat for R5 and R7

![20141012_172426-1.jpeg](image/20141012_172426-1.jpeg)

![Screenshot_(12).png](image/Screenshot_(12).png)

**! R1**

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** eigrp router-id 1.1.1.1**</span>

<span style="background-color: #ffaaaa">** network 1.1.1.1 0.0.0.0**</span>

<span style="background-color: #ffaaaa">** network 12.0.0.1 0.0.0.0**</span>

**! R2**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 100 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 100 area 0**</span>

<span style="background-color: #ffaaaa">**router ospf 100**</span>

<span style="background-color: #ffaaaa">** router-id 2.2.2.2**</span>

<span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span>

!

<span style="background-color: #ffaaaa">**vrf definition ABC**</span>

<span style="background-color: #ffaaaa">** rd 1:1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target both 1:1**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** vrf forwarding ABC**</span>

<span style="background-color: #ffaaaa">** ip address 12.0.0.2 255.255.255.0**</span>

!

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** eigrp router-id 2.2.2.2**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span>

<span style="background-color: #ffaaaa">**  autonomous-system 1**</span>

<span style="background-color: #ffaaaa">**  network 12.0.0.2 0.0.0.0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** bgp router-id 2.2.2.2**</span>

<span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 4.4.4.4 remote-as 100**</span>

<span style="background-color: #ffaaaa">** neighbor 4.4.4.4 update-source lo0**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4**</span>

<span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 activate**</span>

<span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 send-community extended**</span>

!

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router eigrp 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  redistribute bgp 100 metric 1 1 1 1 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router bgp 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  redistribute eigrp 1**</span></span>

**! R3**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa">**router ospf 100**</span>

<span style="background-color: #ffaaaa">** router-id 3.3.3.3**</span>

<span style="background-color: #ffaaaa">** mpls ldp auto-config**</span>

<span style="background-color: #ffaaaa">** area 0**</span>

<span style="background-color: #ffaaaa">**  int lo0**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/1**</span>

**! R4**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa">**router ospf 100**</span>

<span style="background-color: #ffaaaa">** router-id 4.4.4.4**</span>

<span style="background-color: #ffaaaa">** mpls ldp auto-config**</span>

<span style="background-color: #ffaaaa">** area 0**</span>

<span style="background-color: #ffaaaa">**  int lo0**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

!

<span style="background-color: #ffaaaa">**vrf ABC**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  export route-target 1:1**</span>

<span style="background-color: #ffaaaa">**  import route-target 1:1**</span>

<span style="background-color: #ffaaaa">**int gi0/0/0/1**</span>

<span style="background-color: #ffaaaa">** no ipv4 address 45.0.0.4 255.255.255.0**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">** ipv4 address 45.0.0.4 255.255.255.0**</span>

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">**  rd 1:1**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** bgp router-id 4.4.4.4**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 2.2.2.2**</span>

<span style="background-color: #ffaaaa">**  remote-as 100**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**   redistribute eigrp 1**</span>

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** vrf ABC**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  default-metric 1 1 1 1 1**</span>

<span style="background-color: #ffaaaa">**  autonomous-system 1**</span>

<span style="background-color: #ffaaaa">**  redistribute bgp 100**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/1**</span>

**! R5**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa">**router ospf 200**</span>

<span style="background-color: #ffaaaa">** router-id 5.5.5.5**</span>

<span style="background-color: #ffaaaa">** mpls ldp auto-config**</span>

<span style="background-color: #ffaaaa">** area 0**</span>

<span style="background-color: #ffaaaa">**  int lo0**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

!

<span style="background-color: #ffaaaa">**vrf DEF**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  export route-target 2:2**</span>

<span style="background-color: #ffaaaa">**  import route-target 2:2**</span>

<span style="background-color: #ffaaaa">**int gi0/0/0/1**</span>

<span style="background-color: #ffaaaa">** no ipv4 address 45.0.0.5 255.255.255.0**</span>

<span style="background-color: #ffaaaa">** vrf DEF**</span>

<span style="background-color: #ffaaaa">** ipv4 address 45.0.0.5 255.255.255.0**</span>

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** vrf DEF**</span>

<span style="background-color: #ffaaaa">**  rd 2:2**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** bgp router-id 5.5.5.5**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 7.7.7.7**</span>

<span style="background-color: #ffaaaa">**  remote-as 200**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** vrf DEF**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**   redistribute eigrp 1**</span>

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** vrf DEF**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4**</span>

<span style="background-color: #ffaaaa">**   default-metric 1 1 1 1 1**</span>

<span style="background-color: #ffaaaa">**   autonomous-system 1**</span>

<span style="background-color: #ffaaaa">**   redistribute bgp 200**</span>

<span style="background-color: #ffaaaa">**   int gi0/0/0/1**</span>

**! R6**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 200 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 200 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** ip ospf 200 area 0**</span>

<span style="background-color: #ffaaaa">**router ospf 200**</span>

<span style="background-color: #ffaaaa">** router-id 6.6.6.6**</span>

<span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span>

**! R7**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 200 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 200 area 0**</span>

<span style="background-color: #ffaaaa">**router ospf 200**</span>

<span style="background-color: #ffaaaa">** router-id 7.7.7.7**</span>

<span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span>

!

<span style="background-color: #ffaaaa">**vrf definition DEF**</span>

<span style="background-color: #ffaaaa">** rd 2:2**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target both 2:2**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** vrf forwarding DEF**</span>

<span style="background-color: #ffaaaa">** ip address 78.0.0.7 255.255.255.0**</span>

!

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** eigrp router-id 7.7.7.7**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf DEF**</span>

<span style="background-color: #ffaaaa">**  autonomous-system 1**</span>

<span style="background-color: #ffaaaa">**  network 78.0.0.7 0.0.0.0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** bgp router-id 7.7.7.7**</span>

<span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 5.5.5.5 remote-as 200**</span>

<span style="background-color: #ffaaaa">** neighbor 5.5.5.5 update-source lo0**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4**</span>

<span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 activate**</span>

<span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 send-community extended**</span>

!

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf DEF**</span>

<span style="background-color: #ffaaaa">**  redistribute bgp 200 metric 1 1 1 1 1**</span>

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf DEF**</span>

<span style="background-color: #ffaaaa">**  redistribute eigrp 1**</span>

**! R8**

<span style="background-color: #ffaaaa">**router eigrp 1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  router-id 8.8.8.8**</span>

<span style="background-color: #ffaaaa">**  int lo0**</span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

**Verification:**

**! R1**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip eigrp int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip eigrp nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route eigrp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**ping 8.8.8.8**</span></span>

**! R2 and R7**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf detail**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip eigrp vrf DEF int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip eigrp vrf DEF nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route vrf DEF eigrp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route vrf DEF bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip eigrp vrf DEF topology**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all**</span></span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u all labels**</span>

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

**! R3**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

**! R4**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf ABC detail**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip vrf ABC int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf ABC int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf ABC nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route vrf ABC eigrp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route vrf ABC bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf ABC topology**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u**</span></span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u labels**</span>

**! R5**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf DEF detail**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip vrf DEF int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf DEF int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf DEF nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route vrf DEF eigrp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route vrf DEF bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp vrf DEF topology**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u**</span></span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u labels**</span>

**! R6**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

**! R8**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh eigrp nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route eigrp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**ping 1.1.1.1**</span></span>
