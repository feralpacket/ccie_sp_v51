# IS-IS - Class Notes

**IS-IS** (8 Sept 2014)
Lab: IS-IS

- Link state routing protocol

     -> 0xFEFE

     -> 0x0800

- Connectionless Network Protocol (CLNP)

- Like an IP protocol suite in OSI model

     -> Router

          -> Intermediate System (IS)

     -> End host

          -> End System (ES)

     -> OSI protocol

          -> Directly works over layer 2

     -> Integrated IS-IS can have payload of IPv4 or IPv6

**IS-IS uses the following parameters**

- IS-IS Hello (IIH)

- Link State Packet (LSP)

     -> Database IS-IS

- Complete Sequence Number PDU (CSNP)

     -> A list of database entries

- Partial Sequence Number PDU (PSNP)

     -> A request to send LSPs

![20141006_124520-1.jpeg](image/20141006_124520-1.jpeg)

**Neighbor Formation**
 - IIH must match the following
     -> Authentication
     -> IS type
     -> MTU
     -> Priority
     -> system-id / area-id

**Link can be point-to-point or multi-access**
 - Network types
     -> Point-to-point
     -> Broadcast
          -> DIS is elected (Designated Intermediate System)
          -> Highest priority selected
          -> Highest Subnet Point of Attachment (SNPA)
          -> MAC address (ethernet)
          -> Frame-relay DLCI
          -> Highest system-id
          -> DIS election is pre-emptive

**SNAP Address**
 - Subnetwork Access Point
 - 20 butes represented in HEX
     -> Bytes 1 - 13
          -> area-id
     -> Bytes 14 - 20
          -> Network Selector Field
               -> Always equal to "00"

A SNAP address with a NSEL part 0 Network Entity Title (NET) address
     -> Must be an even number of bytes

**IS-IS Area and Backbone Network**
 - Level-1
     -> Router configuration mode
     -> Similar to a NSSA
 - Level-2
     -> Interface configuration mode
     -> Similar to Area 0 routers
 - Level-1-2
 - The level decides which updates can be received
 - Two routers can be in different areas and still form neighbors
     -> This means IS-IS areas are per router, as opposed to OSPF which is area per-link

![20141006_124553-1.jpeg](image/20141006_124553-1.jpeg)

![20141006_124559-1.jpeg](image/20141006_124559-1.jpeg)

All routers and links, by default, are Level-1-2
 - Within an area, L-1-2 neighbors can be formed
 - Between areas, only L-2 neighbors can be formed
 - A consecutive set of Level-2 adjacencies is called a backbone, which may include several routers
     -> The Level-2 adjacencies cannot be discontiguous
 - The Level-1-2 router on the edge will send all Level-1 LSP with attached it set towards Level-1 routers

**IS-IS Data Flow Procedures**
 - Receive operation
     -> Updates are received as LSP
 - Update operation
     -> Updates are sent as LSP
 - Decision operation
     -> SPF algorithm finding the best routes
 - Forward operation
     -> Create CEF table entries with the best routes

**IS-IS Metric**
 - Metric is cost
 - Cost is constant 10 everywhere
     -> No calculation
 - By default, "narrow metric" is used
     -> 6 bit
     -> 1 - 63
 - Can be changed to "wide metric"
     -> 2^24
 - “Wide metric” is needed for MPLS TE

**IS-IS Topology**
 - Single topology
     -> IPv4 and IPv6 address-families share the same path calculation
     -> IPv4 and IPv6 has to be enabled on all interfaces configured for IS-IS
          -> The interfaces doesn’t necessarily need both IPv4 and / or IPv6 addresses configured
 - Multi topology
     -> IPv4 and IPv6 address-families calculate best paths independently
     -> IPv4 and IPv6 configuration independent
          -> Interfaces configured for IS-IS can have IPv4 enabled, IPv6 enabled, or both
 - For IOS routers, the default configuration is single topology
 - For XR routers, the default configuration is multi topology

![20141006_124610-1.jpeg](image/20141006_124610-1.jpeg)

**Configuring IS-IS**

IOS Router

(config)# <span style="background-color: #ffaaaa">router isis [<name>}</span>
     -> default name is NULL
<span style="background-color: #ffaaaa"> net 49.001.0000.0000.1111.00</span>
<span style="background-color: #ffaaaa">int lo0</span>
<span style="background-color: #ffaaaa"> ip routing isis</span>
<span style="background-color: #ffaaaa"> ipv6 routing isis</span>
<span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> ip routing isis</span>
<span style="background-color: #ffaaaa"> ipv6 routing isis</span>

XR Router

(config)# <span style="background-color: #ffaaaa">router isis <name></span>
<span style="background-color: #ffaaaa"> net 49.0001.0000.0000.1111.00</span>
     -> area: area 49.0001
     -> system:  .0000.0000.1111
     -> NSEL:  .00

<span style="background-color: #ffaaaa"> int lo0</span>
<span style="background-color: #ffaaaa">  address-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>
<span style="background-color: #ffaaaa"> int g0/0/0/0</span>
<span style="background-color: #ffaaaa">  address-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  address-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">show clns neighbor</span>
<span style="background-color: #ffaaaa">show isis neighbor</span>
     -> "L1 L2 neighbors"

IOS (Level type, interface)

<span style="background-color: #ffaaaa">int e0/0</span>
<span style="background-color: #ffaaaa"> isis circuit-type level-1</span>

XR (Level type, interface)

<span style="background-color: #ffaaaa">router isis ABC</span>
<span style="background-color: #ffaaaa"> int g0/0/0/0</span>
<span style="background-color: #ffaaaa">  circuit-type level-1</span>

IOS (Level type, entire router)

<span style="background-color: #ffaaaa">router isis</span>
<span style="background-color: #ffaaaa"> is-type level-1</span>

XR (Level type, entire router)

router isis ABC
 is-type level-1

IOS (Timers)

<span style="background-color: #ffaaaa">int e0/0</span>
<span style="background-color: #ffaaaa"> isis hello-interval <sec></span>
<span style="background-color: #ffaaaa"> isis hello-multiplier <count></span>

XR (Timers)

<span style="background-color: #ffaaaa">router isis ABC</span>
<span style="background-color: #ffaaaa"> int g0/0/0/0</span>
<span style="background-color: #ffaaaa">  hello-interval <sec></span>
<span style="background-color: #ffaaaa">  hello-multiplier <count></span>

IOS 

<span style="background-color: #ffaaaa">int e0/0</span>
<span style="background-color: #ffaaaa"> isis priority <value></span>
<span style="background-color: #ffaaaa">     -> 0 - 127</span>
<span style="background-color: #ffaaaa">     -> 64 is default</span>
<span style="background-color: #ffaaaa"> isis metric <value> [ level-1 | level-2 ]</span>
<span style="background-color: #ffaaaa"> isis password <password></span>
<span style="background-color: #ffaaaa"> isis network { point-to-point | broadcast }</span>

**Route Leaking**
 - Routes can be leaked from Level-1 to Level-2
 - and visa versa

On IOS Level-1-2 Router

<span style="background-color: #ffaaaa">router isis</span>
<span style="background-color: #ffaaaa"> redistribute isis ip level-2 into level-1 { distribute-list <name> | route-map <name> }</span>

<span style="background-color: #ffaaaa">access-list <number> permit | deny <protocol> <source> <wildcard> <destination> <wildcard></span>
     -> network leaked - <source> <wildcard>
     -> subnet mask to match - <destination> <wildcard>

Scenario -> Leak the loopback of R3 to R1

R2(config)# <span style="background-color: #ffaaaa">access-list 100 permit ip 3.3.3.3 0.0.0.0 255.255.255.255 0.0.0.0</span>
<span style="background-color: #ffaaaa">ip prefix-list ABC permit 3.3.3.3 255.255.255.255</span>
<span style="background-color: #ffaaaa">route-map LEAK</span>
<span style="background-color: #ffaaaa"> match ip add prefix-list ABC</span>

XR

(config)# r<span style="background-color: #ffaaaa">outer-policy POLICY1</span>
<span style="background-color: #ffaaaa"> if destination in (3.3.3.3/32)</span>
<span style="background-color: #ffaaaa">  pass</span>
<span style="background-color: #ffaaaa">  end if</span>
<span style="background-color: #ffaaaa">router isis ABC</span>
<span style="background-color: #ffaaaa"> address-family ipv4</span>
<span style="background-color: #ffaaaa"> propagate level-2 into level-1 route-policy POLICY1</span>

IOS

(config)# <span style="background-color: #ffaaaa">mpls ip</span>
<span style="background-color: #ffaaaa"> mpls label protocol ldp</span>
<span style="background-color: #ffaaaa"> ip cef</span>
<span style="background-color: #ffaaaa"> int fa0/0</span>
<span style="background-color: #ffaaaa">  mpls ip</span>

XR

(config)# <span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa"> int fa0/0</span>
<span style="background-color: #ffaaaa"> int s0/0</span>
<span style="background-color: #ffaaaa"> root</span>
<span style="background-color: #ffaaaa"> commit</span>

<span style="background-color: #ffaaaa">sh run mpls ldp</span>
