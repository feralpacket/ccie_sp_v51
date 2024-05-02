# Inter-AS VPN Option (10) B - Class Notes

**Inter-AS VPN Option (10) B** (16 Sept 2014)

Lab: Inter-AS A, Inter-AS B, Inter-AS C

 - ASBR VPNv4 MP-eBGP

     -> From PE to PE, it must be a Labeled Switched Path (LSP)

     -> At every next-hop rewrite on a BGP VPNv4 router, the VPN label is also rewritten

     -> When updates come from MP-eBGP neighbor and go to MP-iBGP neighbor, the next-hop will be unchanged by default

     -> Between MP-eBGP neighbors, a /32 bit route for other is required to generate transport labels

          -> With IOS routers, eBPG neighbors automatically install /32 bit routes for the interface connected to the eBGP

          -> With XR routers, a static route for the neighbors IP addresses, /32 bit, must be created

               -> The label must be created for the /32 bit route

     -> POP shows the neighbor does MPLS

          -> no label says the neighbor does not do MPLS

![20141012_172435-1.jpeg](image/20141012_172435-1.jpeg)

![20141012_172448-1.jpeg](image/20141012_172448-1.jpeg)

**IOS Router**

R3(config)# <span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 1.1.1.1 remote-as 100**</span>

<span style="background-color: #ffaaaa">** neighbor 1.1.1.1 update-source lo0**</span>

<span style="background-color: #ffaaaa">** neighbor 35.0.0.5 remote-as 200**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">**  neighbor 1.1.1.1 activate**</span>

<span style="background-color: #ffaaaa">**  neighbor 1.1.1.1 next-hop-self**</span>

<span style="background-color: #ffaaaa">**  neighbor 35.0.0.5 activate**</span>

**XR Router**

R5(config)# <span style="background-color: #ffaaaa">**route-policy PASS**</span>

<span style="background-color: #ffaaaa">** pass**</span>

<span style="background-color: #ffaaaa">**router static**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span>

<span style="background-color: #ffaaaa">**  35.0.0.3/32 s0/0**</span>

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">** neighbor 7.7.7.7**</span>

<span style="background-color: #ffaaaa">**  remote-as 200**</span>

<span style="background-color: #ffaaaa">**  update-source lo0**</span>

<span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">**   next-hop-self**</span>

<span style="background-color: #ffaaaa">** neighbor 35.0.0.3**</span>

<span style="background-color: #ffaaaa">**  remote-as 100**</span>

<span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span>

<span style="background-color: #ffaaaa">**   route-policy PASS in**</span>

<span style="background-color: #ffaaaa">**   route-policy PASS out**</span>

**To check the transport label**

<span style="background-color: #ffaaaa">**sh mpls forwarding-table**</span>

     -> IOS

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

     -> XR

**To check the VPN label**

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast all labels**</span>

     -> IOS

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast labels**</span>

     -> XR

**To check updates received**

<span style="background-color: #ffaaaa">**sh bpg vpnv4 unicast all**</span>

     -> IOS

     -> On any VPNv4 router

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast**</span>

     -> XR

**The check updates sent**

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast all neighbor <ip add> advertised-routes**</span>

     -> IOS

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast neighbor <ip add> advertised-routes**</span>

**To see which routes are received after incoming filtering**

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast all neighbor <ip add> routes**</span>

     -> IOS

<span style="background-color: #ffaaaa">**sh bgp vpnv4 unicast neighbor <ip add> routes**</span>

     -> XR

![Screenshot_(13).png](image/Screenshot_(13).png)

**! R1**

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa">** ip ospf 1 area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 1.1.1.1**</span></span>

**! R2**

<span style="background-color: #ffaaaa">**mpls label protocol ldp**</span>

<span style="background-color: #ffaaaa">**mpls ldp router-id lo0 force**</span>

<span style="background-color: #ffaaaa">**int lo0**</span>

<span style="background-color: #ffaaaa">** ip ospf 100 area 0**</span>

<span style="background-color: #ffaaaa">**int e1/0**</span>

<span style="background-color: #ffaaaa">** ip ospf 100 area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 100**</span></span>

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

<span style="background-color: #ffaaaa">**router ospf 1 vrf ABC**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** network 12.0.0.2 0.0.0.0 area 0**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 4.4.4.4 remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 4.4.4.4 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 activate**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 4.4.4.4 send-community extended**</span></span>

!

<span style="background-color: #ffaaaa">**router ospf 1 vrf ABC**</span>

<span style="background-color: #ffaaaa">** redistribute bgp 100 subnets**</span>

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf ABC**</span>

<span style="background-color: #ffaaaa">**  redistribute ospf 1 vrf ABC match internal external 1 external 2**</span>

<span style="background-color: #ffaaaa">**vrf definition ABC**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target import 2:2**</span>

**! R3**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa">**router ospf 100**</span>

<span style="background-color: #ffaaaa">** router-id 3.3.3.3**</span>

<span style="background-color: #ffaaaa">** mpls ldp auto-config**</span>

<span style="background-color: #ffaaaa">** area 0**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int gi0/0/0/1**</span></span>

**! R4**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 4.4.4.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp auto-config**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** area 0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 4.4.4.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  retain route-target all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 100**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 2.2.2.2**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   next-hop-self**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 45.0.0.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all in**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all out**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router static**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  45.0.0.5/32 gi0/0/0/1**</span></span>

<span style="background-color: #ffaaaa">**route-policy allow-all**</span>

<span style="background-color: #ffaaaa">** pass**</span>

<span style="background-color: #ffaaaa">** end**</span>

**! R5**

<span style="background-color: #ffaaaa">**mpls ldp**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 5.5.5.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp auto-config**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** area 0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 5.5.5.5**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  retain route-target all**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   next-hop-self**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 45.0.0.4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  remote-as 100**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  address-family vpnv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all in**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**   route-policy allow-all out**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router static**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family ipv4 unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  45.0.0.4/32 gi0/0/0/1**</span></span>

<span style="background-color: #ffaaaa">**route-policy allow-all**</span>

<span style="background-color: #ffaaaa">** pass**</span>

<span style="background-color: #ffaaaa">** end**</span>

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

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** mpls ldp autoconfig**</span></span>

!

<span style="background-color: #ffaaaa">**vrf definition DEF**</span>

<span style="background-color: #ffaaaa">** rd 2:2**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target both 2:2**</span>

<span style="background-color: #ffaaaa">**int e1/1**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** vrf forwarding DEF**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** ip address 78.0.0.7 255.255.255.0**</span></span>

!

<span style="background-color: #ffaaaa">**router ospf 1 vrf DEF**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** network 78.0.0.7 0.0.0.0 area 0**</span></span>

!

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** bgp router-id 7.7.7.7**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** no bgp default ipv4-unicast**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 5.5.5.5 remote-as 200**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** neighbor 5.5.5.5 update-source lo0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** address-family vpnv4**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 activate**</span></span>

<span style="background-color: #ffaaaa">**  neighbor 5.5.5.5 send-community extended**</span>

!

<span style="background-color: #ffaaaa">**router ospf 1 vrf DEF**</span>

<span style="background-color: #ffaaaa">** redistribute bgp 200 subnets**</span>

<span style="background-color: #ffaaaa">**router bgp 200**</span>

<span style="background-color: #ffaaaa">** address-family ipv4 vrf DEF**</span>

<span style="background-color: #ffaaaa">**  redistribute ospf 1 vrf DEF match internal external 1 external 2**</span>

<span style="background-color: #ffaaaa">**vrf definition DEF**</span>

<span style="background-color: #ffaaaa">** address-family ipv4**</span>

<span style="background-color: #ffaaaa">**  route-target import 1:1**</span>

**! R8**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**router ospf 1**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** router-id 8.8.8.8**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">** area 0**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**  int lo0**</span></span>

<span style="background-color: #ffaaaa">**  int gi0/0/0/0**</span>

**Verification:**

**! R1**

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf int bri**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip ospf nei**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**sh ip route ospf**</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">**ping 8.8.8.8**</span></span>

**! R2**

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa">**sh vrf detail**</span>

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf ABC ospf**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u all sum**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf ABC ospf**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf ABC**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf ABC vpnv4 u all**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf ABC vpnv4 u all labels**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf ABC bgp**</span>

<span style="background-color: #ffaaaa">**sh ip ospf database**</span>

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

**! R3**

<span style="background-color: #ffaaaa">**sh ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ospf nei**</span>

<span style="background-color: #ffaaaa">**sh route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

**! R4**

<span style="background-color: #ffaaaa">**sh ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ospf nei**</span>

<span style="background-color: #ffaaaa">**sh route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u sum**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u labels**</span>

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

<span style="background-color: #ffaaaa">

</span>

**! R5**

<span style="background-color: #ffaaaa">**sh ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ospf nei**</span>

<span style="background-color: #ffaaaa">**sh route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei bri**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u sum**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u labels**</span>

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

**! R6**

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

**! R7**

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route ospf**</span>

<span style="background-color: #ffaaaa">**sh mpls int**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp nei**</span>

<span style="background-color: #ffaaaa">**sh mpls ldp dis**</span>

<span style="background-color: #ffaaaa">**sh vrf detail**</span>

<span style="background-color: #ffaaaa">**sh ip ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ip ospf nei**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf DEF ospf**</span>

<span style="background-color: #ffaaaa">**sh bgp vpnv4 u all sum**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf DEF ospf**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf DEF**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf DEF vpnv4 u all**</span>

<span style="background-color: #ffaaaa">**sh bgp vrf DEF vpnv4 u all labels**</span>

<span style="background-color: #ffaaaa">**sh ip route vrf DEF bgp**</span>

<span style="background-color: #ffaaaa">**sh ip ospf database**</span>

<span style="background-color: #ffaaaa">**sh mpls forwarding**</span>

**! R8**

<span style="background-color: #ffaaaa">**sh ospf int bri**</span>

<span style="background-color: #ffaaaa">**sh ospf nei**</span>

<span style="background-color: #ffaaaa">**sh route ospf**</span>

<span style="background-color: #ffaaaa">**ping 1.1.1.1**</span>
