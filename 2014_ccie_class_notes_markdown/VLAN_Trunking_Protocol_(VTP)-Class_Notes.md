# VLAN Trunking Protocol (VTP) - Class Notes

**VLAN Trunking Protocol** (18 Aug 2014)

Lab:  STP

 - Domain name

 - Configuration revision number (CRN)

     -> 32 bit

     -> To maintain synchronization of VLANs

 - Version 1 | 2 | 3

 - No difference between version 1 and 2

     -> contrary to theory

          -> Version 1 doesn't support tolken ring

 - Version 1 and 2 can only propagate up to VLAN 1001

 - Version 3 supports up to VLAN 4094

 - Version 3 more secure

     -> Concept of a primary server

          -> Only primary server can update VLAN database

**VTP messages**

 -> Summary Advertisement

     -> Sent every 300 seconds

          -> Domain name

          -> CRN

          -> MD5 hash

          -> Version

          -> Number of subset advertisements following

          -> No actual VLAN information is present in this message

 -> Subset Advertisement

     -> VLAN name

     -> VLAN ID

     -> VLAN MTU

 -> Advertisement Request

     -> First client that receives the request can respond

     -> Sent towards the server

 -> Pruning Message

     -> Pruning VLAN information

<span style="background-color: #ffaaaa">conf t</span>

SW1(config)# <span style="background-color: #ffaaaa">vtp version { 1 | 2 | 3 }</span>

<span style="background-color: #ffaaaa">vtp domain <name></span>

<span style="background-color: #ffaaaa">vtp password <password></span>

<span style="background-color: #ffaaaa">vtp pruning</span>

 -> Default domain name is NULL

 -> only the vtp password command is needed on clients

#<span style="background-color: #ffaaaa"> sh vtp status</span>

MD5 hash = password + CRN

 -> make a VLAN change

 -> or wait for 5 minutes

Routers do not send pruning advertisements

 -> use this to prune VLANs instead

 -> SW1(config)# <span style="background-color: #ffaaaa">switchport trunk allow vlan <vlan></span>

Manual disabling of pruning

SW1(config)# <span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> switchport trunk pruning vlan except 40</span>

# <span style="background-color: #ffaaaa">sh int trunk</span>

 -> lists pruned VLANs

# <span style="background-color: #ffaaaa">sh int fa0/0 pruning</span>
