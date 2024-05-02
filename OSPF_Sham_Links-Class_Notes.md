# OSPF Sham Links - Class Notes

**OSPF Sham Links** (3 Sept 2014)

Lab:  MPLS 1 - 3

![20141003_154440-1.jpeg](image/20141003_154440-1.jpeg)

If the customer is using a back door link that is in area 0, then this link will always be preferred over the MPLS L3VPN connection

 - The back door link updates type 1 LSAs

     -> Intra-Area

 - The MPLS L3VPN updates are

     -> type 3 LSAs (Inter-Area) if the OSPF process-ids match

     -> type 5 LSAs (External 2) if the OSPF process-ids do not match

The solution is to change the MPLS L3VPN updates to type 1 LSAs

 - Achieved by using a sham-link

**Configuration**

 1. Create new loopback interfaces on the PE routers

 2. Associate the loopbacks with the VRF

 3. Advertise the loopbacks in the BGP address-family VRF

     -> Do not advertise the loopbacks into the PE - CE OSPF process

 4. Create the sham-link in the VRF OSPF on the PE routers

 5. Increase the cost on the interface of the backdoor link

Sham-link loopback interfaces must be /32 bit

R1(config)# <span style="background-color: #ffaaaa">int lo10</span>

 <span style="background-color: #ffaaaa">ip vrf forwarding c1b1</span>

 <span style="background-color: #ffaaaa">ip add 50.0.0.1 255.255.255.255</span>

<span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">address-family ipv4 vrf c1b1</span>

  <span style="background-color: #ffaaaa">network 50.0.0.1 mask 255.255.255.255</span>

<span style="background-color: #ffaaaa">router ospf 10 vrf c1b1</span>

 <span style="background-color: #ffaaaa">area 0 sham-lnk 50.0.0.1 50.0.0.2 cost 1</span>

     -> 50.0.0.2 is the loopback IP address on the other PE router

R4(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ip ospf cost 10000</span>
