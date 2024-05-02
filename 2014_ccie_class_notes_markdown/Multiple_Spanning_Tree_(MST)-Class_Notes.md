# Multiple Spanning Tree (MST) - Class Notes

**Multiple Spanning Tree (MST)**
 - 802.1S
 - Called MSTP by Cisco previously
 - Can map VLANs to STP instances
     -> e.g. - 
          -> Instance 1
               -> VLAN 1 - 32
          -> Instance 2
               -> VLAN 33 - 65
          -> Instance 3
               -> VLAN 66 - 100
 - Only supports RSTP for convergence
     -> Activated automatically
 - Problem
     -> Every switch needs to be configured the same
          -> Manually
     -> VTP cannot be used
     -> Use notepad
 - MST creates a set of switches using the same configuration
     -> Called a Region
     -> All have the exact same configuration
     -> Allows up to 16 instances per Region
          -> Instance 0 - 15
          -> Instance 0
               -> Internal Spanning Tree (IST)
               -> By default, all VLANs map to IST
 - Every MST configuration has 3 variables
     -> Region name
          -> Default is NULL
     -> Revision number
          -> Default is 0
     -> Instance to VLAN mapping
          -> Default, all VLANs map to IST
 - Configure one switch, then copy that configuration to all other switches
 - Can configure all of the MST parameters, then activate MST
 - Only one BPDU is sent
     -> Controlled by the IST
     -> Specific information about instances attaches as “M-Records"

![20141222_141111-1.jpeg](image/20141222_141111-1.jpeg)

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mst configuration</span>
SW1(config-mst)# <span style="background-color: #ffaaaa">name CISCO</span>
<span style="background-color: #ffaaaa"> revision 10</span>
<span style="background-color: #ffaaaa"> instance 1 vlan 1 - 32</span>
<span style="background-color: #ffaaaa"> instance 2 vlan 33 - 65</span>
<span style="background-color: #ffaaaa"> instance 3 vlan 66 - 100</span>
<span style="background-color: #ffaaaa"> show pending</span>
<span style="background-color: #ffaaaa"> exit</span>
SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mode mst</span>

Scenario -> In instance 0, configure all VLANs except VLANs 1 - 100.
 - If the question asks you to configure all other VLANs to one instance (after first two), only configure two instances.
     -> All other VLANs will be in instance 0

<span style="background-color: #ffaaaa">show spanning-tree mst configuration</span>
<span style="background-color: #ffaaaa">show spanning-tree mst [id]</span>

**MST introduces MAX HOPS feature**
 - Allows equal to MAX-AGE time
 - Change MAX-AGE time to configure the maximum hops

**MST Interface Costs**
 - 10 Mbps —> 2000000
 - 100 Mbps —> 200000
 - 1 Gbps —> 20000
 - 10 Gbps —> 2000

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mst 1 priority 4096</span>

     - or -

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mst 1 root primary</span>

     - or -

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mst 1 root secondary</span>

Can configure who will be the root for IST (instance 0)

![20141222_141128-1.jpeg](image/20141222_141128-1.jpeg)

Only the IST communicates between Regions
 - Sends a single BPDU
     -> Removes M-Records
          -> This hides the topology of the Region
          -> Similar to the ABR in OSPF
 - One Region appears as a single switch to another Region

![20141222_141135-1.jpeg](image/20141222_141135-1.jpeg)

**CIST Root**
 - The best Bridge-ID switch among all Regions

**Region Root**
 - Instance 0 root bridge in every Region
 - In any Region, only a boundary switch can be a Region root
     -> Exception, the CIST root is also the Region root and does not have to be a boundary switch
     1.  Root bridge
     2.  Cost to reach root bridge (CIST)
          -> Internal cost within the Region are not considered to determine the Region root
          -> Only external cost is considered
     3.  Sender Bridge-ID
     4.  Sender Port-ID

![20141222_141142-1.jpeg](image/20141222_141142-1.jpeg)

**Master Port**
 - Root port of any Region
 - On Region root
 - Seen as “master port” by non-IST instances
     -> Going toward the master

<span style="background-color: #ffaaaa">show spanning-tree mst 0</span>
 - Shows the root port

<span style="background-color: #ffaaaa">show spanning-tree mst 1</span>
 - Show the master port

![20141222_141149-1.jpeg](image/20141222_141149-1.jpeg)

<span style="background-color: #ffaaaa">show run | in mst|name|rev|ins</span>
<span style="background-color: #ffaaaa">show spanning-tree mst configuration</span>
 - Should be the same as
     -> <span style="background-color: #ffaaaa">show pending</span>
<span style="background-color: #ffaaaa">show spanning-tree mst</span>

<span style="background-color: #ffaaaa">clear spanning-tree detected-protocols</span>
 - To switch to MST faster

![20141222_141157-1.jpeg](image/20141222_141157-1.jpeg)

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree mode pvst+</span>
<span style="background-color: #ffaaaa">no spanning-tree mst configuration</span>
<paste MST configuration from Notepad>
 <span style="background-color: #ffaaaa">show pending</span>
<span style="background-color: #ffaaaa"> exit</span>
<span style="background-color: #ffaaaa">spanning-tree mode mst</span>

SW2# <span style="background-color: #ffaaaa">show spanning-tree mst</span>
 E3/0     Root     FWD     Shr Bound (RSTP)

SW3# <span style="background-color: #ffaaaa">show spanning-tree mst</span>
 E3/0     Altn     BLK     Shr Bound (RSTP)
 E3/1     Altn     BLK     Shr Bound (RSTP)

SW2# <span style="background-color: #ffaaaa">show spanning-tree mst 1</span>
 E3/0     Mstr     FWD     Shr Bound (RSTP)
