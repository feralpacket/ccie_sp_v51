# Inter-AS VPN Option (10) C - Class Notes

**Inter-AS VPN Option (10) C** (16 Sept 2014)

Lab: Inter-AS A, Inter-AS B, Inter-AS C

- Multihop MP-BGP

- VPNv4 neighbor relationship between PE routers or router reflectors

     -> Must be able to ping each others loopback interfaces

![20141012_182207-1-1.jpeg](image/20141012_182207-1-1.jpeg)

PE Routers | Route Reflectors should be able to ping loopback to loopback (IPv4)

 - Control Plane

     -> PE - ASBR

          -> iBGP IPv4

     -> ASBR - ASBR

          -> eBGP IPv4

     -> Advertise loopback into BGP

          -> On PE router

 - Data Plane

     -> On XR routers, static routes are needed on the ASBR routers for the transport labels

 - VPNv4 neighbor relationship between R1 and R7 will use loopbacks

     -> 1.1.1.1

     -> 7.7.7.7

     -> Labels must be associated with the next-hop address

          -> 1.1.1.1, 7.7.7.7

 - PE - PE communications

     -> Labels

          -> iBPG / eBGP + IPv4

 - CE - CE communications

     -> Labels

          -> eBGP VPNv4 

          -> PE routers

![20141012_182223-1.jpeg](image/20141012_182223-1.jpeg)

![20141012_182232-1.jpeg](image/20141012_182232-1.jpeg)

**Configuration**

 1. IPv4 - iBGP + label between PE - ASBR

 2. IPv4 - eBGP + label between ASBRs

     -> Configure next-hop-self on both ASBRs towards the PE routers

 3. Static route for each other on ASBRs

     -> XR routes only

 4. MP-eBGP VPNv4 neighbors between PE routers | Router Reflectors loopback to loopback

     -> Multihop

 5. PE - CE VRF and IGP configuration

 6. Mutual redistribution between VRF IGP and VRF BGP

![20141012_182243-1.jpeg](image/20141012_182243-1.jpeg)

![20141012_182248-1.jpeg](image/20141012_182248-1.jpeg)

**Option C with Router Reflectors**

 1. MP-eBGP VPNv4 is configured between Router Reflectors

 2. MP-iBGP VPNv4 between Router Reflector and PE routers

 3. IPv4 - eBGP + label between ASBRs

 4. IPv4 - iBGP between ASBR - RR - PE routers

 5. Loopback addresses of PE routers and Route Reflectors are exchanged

 6. PE - CE stuff

![Screenshot_(14).png](image/Screenshot_(14).png)

**! R1**

<span style="background-color: #ffaaaa">**router bgp 50**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 1.1.1.1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 12.0.0.1 remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  network 1.1.1.1 mask 255.255.255.255**</span></span>

<span style="background-color: #ffaaaa">**  neighbor 12.0.0.1 activate**</span>

**! R2**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span></span>

!

<span style="background-color: #ffaaaa">**vrf definition ABC**</span>

<span style="background-color: #ffaaaa">** rd 1:1**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target both 1:1**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** vrf forwarding ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** ip address 12.0.0.2 255.255.255.0**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 12.0.0.1 remote-as 50**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 12.0.0.1 activate**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 12.0.0.1 as-override**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  redistribute connected**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 4.4.4.4 remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 4.4.4.4 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  network 2.2.2.2 mask 255.255.255.255**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 activate**</span></span>

<span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 send-label**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 7.7.7.7 remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 7.7.7.7 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 7.7.7.7 ebgp-multihop 255**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 7.7.7.7 activate**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 7.7.7.7 send-community extended**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**vrf definition ABC**</span></span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target import 2:2**</span>

**! R3**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa">**router ospf 1**</span>

<span style="background-color: #ffaaaa">** router-id 9.9.0.3**</span>

<span style="background-color: #ffaaaa">** mpls ldp auto-config**</span>

<span style="background-color: #ffaaaa">** area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int gi0/0/0/1**</span></span>

**! R4**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 4.4.4.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp auto-config**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** area 0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 4.4.4.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  allocate-label all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 45.0.0.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family ipv4 labeled-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all in**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all out**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**route-policy allow-all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** pass**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** exit**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router static**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa">**  45.0.0.5/32 gi0/0/0/1**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa">**  remote-as 100**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 labeled-unicast**</span>

<span style="background-color: #ffaaaa">**   next-hop-self**</span>

**! R5**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 5.5.5.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp auto-config**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** area 0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 5.5.5.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  allocate-label all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 45.0.0.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family ipv4 labeled-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all in**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all out**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**route-policy allow-all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** pass**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** exit**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router static**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  45.0.0.4/32 gi0/0/0/1**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** neighbor 7.7.7.7**</span>

<span style="background-color: #ffaaaa">**  remote-as 200**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 labeled-unicast**</span>

<span style="background-color: #ffaaaa">**   next-hop-self**</span>

 

**! R6**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 6.6.6.6**</span></span>

<span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span>

**! R7**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span></span>

!

<span style="background-color: #ffaaaa">**vrf definition DEF**</span>

<span style="background-color: #ffaaaa">** rd 2:2**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target both 2:2**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** vrf forwarding DEF**</span>

<span style="background-color: #ffaaaa">** ip address 78.0.0.7 255.255.255.0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 vrf DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 78.0.0.8 remote-as 50**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 78.0.0.8 activate**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 78.0.0.8 as-override**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  redistribute connected**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 5.5.5.5 remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 5.5.5.5 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  network 7.7.7.7 mask 255.255.255.255**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 activate**</span></span>

<span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 send-label**</span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2 remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2 ebgp-multihop 255**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 2.2.2.2 activate**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 2.2.2.2 send-community extended**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**vrf definition DEF**</span></span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target import 1:1**</span>

**! R8**

<span style="background-color: #ffaaaa">**router bgp 50**</span>

<span style="background-color: #ffaaaa">** bgp router-id 8.8.8.8**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  network 8.8.8.8/32**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 78.0.0.7**</span></span>

<span style="background-color: #ffaaaa">**  remote-as 200**</span>

<span style="background-color: #ffaaaa">**  address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**   route-policy allow-all in**</span>

<span style="background-color: #ffaaaa">**   route-policy allow-all out**</span>

<span style="background-color: #ffaaaa">**route-policy allow-all**</span>

<span style="background-color: #ffaaaa">** pass**</span>

<span style="background-color: #ffaaaa">** end**</span>

**Verification:**

**! R1**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**ping 8.8.8.8 source lo0**</span></span>

**! R2**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf detail**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf ABC all sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf ABC all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

**! R3**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

** ! R4**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp ipv4 labeled-unicast sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp ipv4 labeled-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

**! R5**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp ipv4 labeled-unicast sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp ipv4 labeled-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

**! R6**

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

**! R7**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls int**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls ldp nei**</span></span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh vrf detail**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf DEF all sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vrf DEF all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp vpnv4 u all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh mpls forwarding**</span></span>

**! R8**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp sum**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route bgp**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**ping 1.1.1.1 source 8.8.8.8**</span></span>
