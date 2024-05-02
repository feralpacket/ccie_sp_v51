# Multicast on XR Routers - Class Notes

**Multicast on XR Routers** (15 Sept 2014)

To active

R1(config)# <span style="background-color: #ffaaaa">multicast-routing</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">   enable</span>
<span style="background-color: #ffaaaa">  int g0/0/0/0</span>
<span style="background-color: #ffaaaa">   enable</span>

**PIM Configuration**
 - The only mode supported is sparse mode

<span style="background-color: #ffaaaa">router pim</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">   enable</span>
<span style="background-color: #ffaaaa">  int g0/0/0/0</span>
<span style="background-color: #ffaaaa">   enable</span>

**IGMP Configuration**

<span style="background-color: #ffaaaa">router igmp</span>
<span style="background-color: #ffaaaa"> int lo0</span>
<span style="background-color: #ffaaaa">  join-group 255.1.1.1</span>

**MSDP Configuration**

<span style="background-color: #ffaaaa">router msdp</span>
<span style="background-color: #ffaaaa"> peer <ip add></span>
<span style="background-color: #ffaaaa"> connect-source <ip add></span>

**RP Configuration**
 1. Manual

<span style="background-color: #ffaaaa">router pim</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa">  rp-address <ip add></span>

 2. Auto-RP

<span style="background-color: #ffaaaa">router pim</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa">  auto-rp { candidate-rp | candidate-bsr } <ip add></span>

 3. BSR

<span style="background-color: #ffaaaa">router pim</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa">  bsr { candidate-rp | candidate-bsr } <ip add></span>

During the lab, manual configuration or BSR will be asked 90% of the time.

**Verification:**

<span style="background-color: #ffaaaa">sh pim rp mapping</span>
<span style="background-color: #ffaaaa">sh pim vrf ABC rp mapping</span>
<span style="background-color: #ffaaaa">sh route ipv4 multicast</span>
<span style="background-color: #ffaaaa">sh route vrf ABC ipv4 multicast</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mfib route</span>

<span style="background-color: #ffaaaa">sh mfib vrf ABC route</span>
<span style="background-color: #ffaaaa">sh mfib vrf ABC route 239.255.172.11</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh pim int</span>
<span style="background-color: #ffaaaa">sh pim nei</span>
<span style="background-color: #ffaaaa">sh pim vrf ABC int</span>
<span style="background-color: #ffaaaa">sh pim vrf ABC nei</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh igmp int</span>
<span style="background-color: #ffaaaa">sh igmp groups</span>
<span style="background-color: #ffaaaa">sh igmp vrf ABC int</span>
<span style="background-color: #ffaaaa">sh igmp vrf ABC groups</span>
