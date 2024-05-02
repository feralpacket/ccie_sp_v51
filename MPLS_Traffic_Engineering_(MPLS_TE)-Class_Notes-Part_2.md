# MPLS Traffic Engineering (MPLS TE) - Class Notes - Part 2

- Note: Split on 8 April 2024, because the original export was 33 MB in size. Github file size limit is 25 MB.

**Fast Reroute Protection (FRR)**

 - Fault tolerance mechanism

     -> Link protection

     -> Node protection

 - FRR allows for the tunnel to continue being used in the case of link or node failure

 - For this, it uses the concept of a backup tunnel which gets activated in the case of a link or node failure

 - For any router that wants to protect the link, a backup tunnel is configured to become active when the protected link goes down

 - The backup tunnel is inactive as long as the protected link is active

     -> Very fast switch over

          -> Less than 50 ms

 - Starting point of the backup tunnel is called the Point of Local Repair (PLR)

 - End point of the backup tunnel is called the merge point

![20141010_105723-1.jpeg](image/20141010_105723-1.jpeg)

An explicit tunnel is created on the PLR to reach the merge point by using the backup path

 - This tunnel is then flagged as a backup tunnel inside the protected link's interface

     -> R2's fa0/1

     -> By doing this, the backup tunnel becomes inactive as long as the protected link is up

![20141010_105733-1.jpeg](image/20141010_105733-1.jpeg)

R1 must be configured for FRR

 - The head-end router of the main tunnel must be configured with the fast reroute feature

     -> So that it will continue sending traffic after receiving a path error message

Fast Reroute Configuration 

R1(config)# **int tu1**

** tunnel mpls traffic-eng fast-reroute**

R2(config)# **ip explicit-path name 243 enable**

** next-address 24.0.0.4**

** next-address 34.0.0.3**

**int tu100**

** ipv4 unnumbered lo0**

** tunnel mode mpls traffic-eng**

** tunnel mpls traffic-eng path-option 1 explicit name 243**

** tunnel destination 3.3.3.3**

     -> The merge point

     -> The bandwidth command is not needed

**int fa0/1**

** mpls traffic-eng backup-path tu100**

PLR

 - **sh mpls traffic-eng backup**

     -> 1 configured (0 active)

XR Routers

R1(config)# **int tunnel-te 1**

** fast-reroute**

** root**

** commit**

R2(config)# **explicit-path name 243**

** index 10 next-address strict ipv4 unicast 24.0.0.4**

** index 20 n s i u 34.0.0.3**

**int tunnel-te 100**

** ipv4 unnumbered lo0**

** path-option 1 explicit 243**

** destination 3.3.3.3**

**mpls traffic-eng**

** int fa0/1**

**  backup-path tunnel-te 100**

**  root**

**  commit**

![20141010_105748-1.jpeg](image/20141010_105748-1.jpeg)

**Node Protection**

 - Next-to-next hop protection

     -> NNHOP

 - In node protection, next-to-next hop router from PLR is the merge point

 - The backup tunnel is created with the merge point as the destination and explicit path-option excluding the failed node

 - When the merge point receives the PATH message, it replies with the RESV message including it's "incoming label" expected from the failed node

 - The main tunnel's head-end router should be configured with the fast reroute option

IOS Routers

R1(config)# **int tu1**

** tunnel mpls traffic-eng fast-reroute node-protect**

R2(config)# **ip explicit-path 245 enable**

** exclude-address 3.3.3.3**

**int tu100**

** ip unnumbered lo0**

** tunnel mode mpls traffic-eng**

** tunnel destination 5.5.5.5**

** tunnel mpls traffic-eng path-option 1 explicit name 245**

**int fa0/0**

** mpls traffic-eng backup-path tu100**

On PLR

 - **sh mpls traffic-eng fast-reroute database [detail]**

**Inter-Area Traffic Engineering**

 - OPSF

     -> Multiple areas

     -> Opaque LSA 11

          -> Scope - global (inter-area)

 - IS-IS

     -> Level-1 and Level-2 domains

          -> TLV 22 (type-length-value)

               -> within a level

          -> TLV 125

               -> Between levels (Level-1 and Level-2)

 - Behaves like a link state protocol only within the same area

 - Behaves like a distance vector protocol between multiple areas

![20141010_105800-1.jpeg](image/20141010_105800-1.jpeg)

R1(config)# **int tu1**

** ip unnumbered lo0**

** tunnel mode mpls traffic-eng**

** tunnel mpls traffic-eng bandwidth 2000**

** tunnel mpls traffic-eng path-option 1 explicit name ABC**

** tunnel destination 6.6.6.6**

**ip explicit-path name ABC enable**

** next-address loose 3.3.3.3**

     -> ABR

** next-address loose 5.5.5.5**

     -> ABR

** next-address loose 6.6.6.6**

     -> Destination

![20141010_105808-1.jpeg](image/20141010_105808-1.jpeg)

 - On head-end router, the tunnel is configured with an explicit path-option with loose next-address

 - Loose next-addresses are the mpls router-ids of ABRs in the path or Layer-1-2 routers

 - When the path message is to be sent, these loose addresses are expanded to a collection of strict addresses by head-end router and every ABR

XR Routers

 

**explicit-path name ABC**

** index 10 next-address loose ipv4 unicast <ABR address>**

**Restriction on Inter-Area Traffic Engineering**

 1. Cannot use dynamic path-option

 2. Cannot use affinity / attribute-flag

 3. Cannot use autoroute announce

![Screenshot_(17).png](image/Screenshot_(17).png)

**! R1**

**mpls ldp**

**router ospf 1**

** router-id 1.1.1.1**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

!

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**  exit**

**  exit**

!     -> One of the problems with typing the configuration into a text editor and copying and pasting the configuration into the router

!     -> “root” also works;  if not, “mpls traffic-eng” and the interfaces in the next section will get pasted into the “area 0"

!     -> The other option is to configure “mpls traffic” before “router ospf 1"

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 25000**

** int gi0/0/0/1**

**  bandwidth 25000**

!

**int tunnel-te 16**

** ipv4 unnumbered lo0**

** destination 6.6.6.6**

** signalled-bandwidth 4000**

** path-option 1 explicit name 16**

**explicit-path name 16**

** index 10 next-address strict ipv4 unicast 18.0.0.8**

** index 20 n s i u 78.0.0.7**

** index 30 n s i u 27.0.0.2**

** index 40 n s i u 23.0.0.3**

** index 50 n s i u 34.0.0.4**

** index 60 n s i u 46.0.0.6**

!

**int lo1**

** ipv4 add 1.1.1.11/32**

**router static**

** address-family ipv4 unicast**

** 6.6.6.66/32 tunnel-te 16**

 

**! R2**

**mpls ldp**

**router ospf 1**

** router-id 2.2.2.2**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**  exit**

**  exit**

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 25000**

** int gi0/0/0/1**

**  bandwidth 25000**

** int gi0/0/0/2**

**  bandwidth 25000**

**! R3**

**mpls ldp**

**router ospf 1**

** router-id 3.3.3.3**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**  exit**

**  exit**

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 25000**

** int gi0/0/0/1**

**  bandwidth 25000**

** int gi0/0/0/2**

**  bandwidth 25000**

**! R4**

**mpls ldp**

**router ospf 1**

** router-id 4.4.4.4**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**  exit**

**  exit**

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 25000**

** int gi0/0/0/1**

**  bandwidth 25000**

** int gi0/0/0/2**

**  bandwidth 25000**

**! R5**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 5.5.5.5**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnels**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** mpls traffic-eng area 0**

**! R6**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 6.6.6.6**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnels**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** mpls traffic-eng area 0**

!

**int tu61**

** ip unnumbered lo0**

** tunnel mode mpls traffic-eng**

** tunnel mpls traffic-eng bandwidth 4000**

** tunnel mpls traffic-eng path-option 1 explicit name 61**

** tunnel destination 1.1.1.1**

**ip explicit-path name 61 enable**

** next-address 56.0.0.5**

** ne 35.0.0.3**

** ne 23.0.0.2**

** ne 27.0.0.7**

** ne 78.0.0.8**

** ne 18.0.0.1**

!

**int lo1**

** ip add 6.6.6.66 255.255.255.255**

**ip route 1.1.1.11 255.255.255.255 tu61**

**! R7**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**int s2/0**

** ip ospf 1 area 0**

** ip ospf cost 1**

**router ospf 1**

** router-id 7.7.7.7**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnels**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**int s2/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** mpls traffic-eng area 0**

**! R8**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int s2/0**

** ip ospf 1 area 0**

** ip ospf cost 1**

**router ospf 1**

** router-id 8.8.8.8**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnels**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**int s2/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 25000**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** mpls traffic-eng area 0**

**Verification:**

**IOS:**

**sh ip ospf int bri**

**sh ip ospf nei**

**sh ip route ospf**

**sh mpls int**

**sh mpls ldp nei**

**sh mpls ldp dis**

**sh mpls traffic-eng topology**

     -> You configured the MPLS TE router-id right?

**sh mpls traffic-eng tunnels**

**sh ip explicit-paths**

**sh ip rsvp nei**

**sh ip rsvp int**

**sh ip rsvp sender**

**sh ip rsvp reservation**

**XR:**

**sh ospf int rbi**

**sh ospf nei**

**sh route ospf**

**sh mpls int**

**sh mpls ldp nei bri**

**sh mpls ldp dis**

**sh mpls traffic-eng topology**

**sh mpls traffic-eng tunnels**

**sh explicit-paths**

**sh rsvp nei**

**sh rsvp int**

**sh rsvp sender**

**sh rsvp reservation**

**! R1**

**sh ip int bri**

     -> Is the tunnel up?

**sh mpls traffic-eng tunnels**

     -> Will tell you why the tunnel is not up

**ping 6.6.6.66**

**traceroute 6.6.6.66**

**sh rsvp int**

     -> Did the bandwidth get allocated?

**sh rsvp sender**

**sh rsvp reservation**

**! R6**

**sh ip int bri**

**sh mpls traffic-eng tunnels**

**ping 1.1.1.11**

**traceroute 1.1.1.11**

**sh rsvp int**

**sh ip rsvp sender**

**sh ip rsvp reservation**

![Screenshot_(18).png](image/Screenshot_(18).png)

**! R1**

**mpls ldp**

**router ospf 1**

** router-id 1.1.1.1**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

!

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 15000**

** int gi0/0/0/1**

**  bandwidth 15000**

!

**int tunnel-te 16**

** ipv4 unnumbered lo0**

** destination 6.6.6.6**

** signalled-bandwidth 4000**

** path-option 1 explicit name 16**

**explicit-path name 16**

** index 10 next-address strict ipv4 unicast 12.0.0.2**

** index 20 n s i u 23.0.0.3**

** index 30 n s i u 35.0.0.5**

** index 40 n s i u 56.0.0.6**

!

**int tunnel-te 16**

** fast-reroute**

!

**router static**

** address-family ipv4 unicast**

**  6.6.6.66/32 tunnel-te 16**

**! R2**

**mpls ldp**

**router ospf 1**

** router-id 2.2.2.2**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 15000**

** int gi0/0/0/1**

**  bandwidth 15000**

** int gi0/0/0/2**

**  bandwidth 15000**

!

**int tunnel-te 100**

** ipv4 unnumbered lo0**

** destination 3.3.3.3**

** path-option 1 explicit name 2743**

**explicit-path name 2743**

** index 10 n s i u 27.0.0.7**

** index 20 n s i u 47.0.0.4**

** index 30 n s i u 34.0.0.3**

**mpls traffic-eng**

** int gi0/0/0/1**

**  backup-path tunnel-te 100**

**! R3**

**mpls ldp**

**router ospf 1**

** router-id 3.3.3.3**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 15000**

** int gi0/0/0/1**

**  bandwidth 15000**

** int gi0/0/0/2**

**  bandwidth 15000**

**! R4**

**mpls ldp**

**router ospf 1**

** router-id 4.4.4.4**

** mpls ldp auto-config**

** area 0**

**  int lo0**

**  int gi0/0/0/0**

**  int gi0/0/0/1**

**  int gi0/0/0/2**

!

**mpls traffic-eng**

** int gi0/0/0/0**

** int gi0/0/0/1**

** int gi0/0/0/2**

**router ospf 1**

** mpls traffic-eng router-id lo0**

** area 0**

**  mpls traffic-eng**

**rsvp**

** int gi0/0/0/0**

**  bandwidth 15000**

** int gi0/0/0/1**

**  bandwidth 15000**

** int gi0/0/0/2**

**  bandwidth 15000**

**! R5**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 5.5.5.5**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnel**

**router ospf 1**

** mpls traffic-eng area 0**

** mpls traffic-eng router-id lo0**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**! R6**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 6.6.6.6**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnel**

**router ospf 1**

** mpls traffic-eng area 0**

** mpls traffic-eng router-id lo0**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 105000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

!

**int lo1**

** ip add 6.6.6.66 255.255.255.255**

**! R7**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int e1/1**

** ip ospf 1 area 0**

**int s2/0**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 7.7.7.7**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnel**

**router ospf 1**

** mpls traffic-eng area 0**

** mpls traffic-eng router-id lo0**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**int e1/1**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**int s2/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**! R8**

**mpls label protocol ldp**

**mpls ldp router-id lo0 force**

**int lo0**

** ip ospf 1 area 0**

**int e1/0**

** ip ospf 1 area 0**

**int s2/0**

** ip ospf 1 area 0**

**router ospf 1**

** router-id 8.8.8.8**

** mpls ldp autoconfig**

!

**mpls traffic-eng tunnel**

**router ospf 1**

** mpls traffic-eng area 0**

** mpls traffic-eng router-id lo0**

**int e1/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

**int s2/0**

** mpls traffic-eng tunnels**

** ip rsvp bandwidth 15000**

Verification:

**IOS:**

**sh ip ospf int bri**

**sh ip ospf nei**

**sh ip route ospf**

**sh mpls int**

**sh mpls ldp nei**

**sh mpls ldp dis**

**sh mpls traffic-eng topology**

**sh mpls traffic-eng tunnels**

**sh mpls traffic-eng tunnels protection**

**sh mpls traffic-eng tunnels backup**

**sh mpls traffic-eng fast-reroute database state complete**

**sh ip explicit-paths**

**sh ip rsvp nei**

**sh ip rsvp int**

**sh ip rsvp sender**

**sh ip rsvp reservation**

**XR:**

**sh ospf int rbi**

**sh ospf nei**

**sh route ospf**

**sh mpls int**

**sh mpls ldp nei bri**

**sh mpls ldp dis**

**sh mpls traffic-eng topology**

**sh mpls traffic-eng tunnels**

**sh mpls traffic-eng tunnels protection**

**sh mpls traffic-eng tunnels backup**

**sh mpls traffic-eng fast-reroute database state complete**

**sh explicit-paths**

**sh rsvp nei**

**sh rsvp int**

**sh rsvp sender**

**sh rsvp reservation**

**! R1**

**sh ip int bri**

**sh mpls traffic-eng tunnels**

**ping 6.6.6.66**

**traceroute 6.6.6.66**

     -> Shutdown interface gi0/0/0/0 on R2 and run traceroute again

**sh mpls traffic-eng tunnels protection**

**sh rsvp int**

**sh rsvp sender**

**sh rsvp reservation**

**! R2**

**sh mpls traffic-eng tunnels backup**

**sh mpls traffic-eng fast-reroute database state complete**

     -> Shutdown interface gi0/0/0/0 and run this commands again

**! R6**

**sh mpls traffic-eng tunnels**
