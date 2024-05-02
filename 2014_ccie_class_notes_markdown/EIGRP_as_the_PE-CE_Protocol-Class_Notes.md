# EIGRP as the PE - CE Protocol - Class Notes

**EIGRP as the PE to CE Protocol** (2 Sept 2014)Lab:  MPLS 1 - 3

R1(config)# <span style="background-color: #ffaaaa">router eigrp 1</span>
 <span style="background-color: #ffaaaa">address-family ipv4 vrf c1b1</span>
  <span style="background-color: #ffaaaa">autonomous-system 10</span>
  <span style="background-color: #ffaaaa">network 14.0.0.0</span>
  <span style="background-color: #ffaaaa">redistribute bgp metric 1 1 1 1 1</span>

<span style="background-color: #ffaaaa">sh mpls interface</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 unicast all</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh ip eigrp vrf int</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh ip eigrp vrf ABC nei</span>
<span style="background-color: #ffaaaa">sh ip route vrf ABC eigrp</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">ping vrf ABC 172.9.195.15</span>
<span style="background-color: #ffaaaa">ping vrf ABC 172.9.0.15</span>
<span style="background-color: #ffaaaa">ping vrf ABC ipv6 2002:172:9::16</span>
<span style="background-color: #ffaaaa">ping ipv6 2002:172:9::10</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
**Verify redistribution:**
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh ip route vrf ABC eigrp</span>
<span style="background-color: #ffaaaa">sh bgp vrf ABC all</span>
     -> Are the EIGRP routes in the BGP table?<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh ip route vrf ABC bgp</span>
<span style="background-color: #ffaaaa">sh ip eigrp vrf ABC topology</span>
     -> Are the BGP routes in the EIGRP topology?<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
