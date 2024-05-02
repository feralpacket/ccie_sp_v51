# EIGRP - Class Notes

**EIGRP** (21 Aug 2014)

- IP protocol 88

- Multiple L3 protcols (Protocol Independent)

- Distance Vector Protocol

- AD

     -> Internal

          -> 90

     -> External

          -> 170

     -> Summary

          -> 5

- Composite metric which is calculated by bandwidth and delay by default

     -> Can also use reliability and load

- Uses 3 tables

     -> Neighbor table

     -> Topology table

     -> Routing table

- 5 message types

     -> Hello

     -> Update

     -> Query

     -> Reply

     -> Acknowledgement

- Muilticast 224.0.0.10 to communicate with neighbors

- Hello time

     -> LAN interface

          -> 5

     -> WAN interface

          -> 60

- Holddown

     -> LAN interface

          -> 15

     -> WAN interface

          -> 180

- Incremental and change based updates

- Supports only MD5 authentication

CCNP covers 95% of EIGRP used in CCIE

**Neighbors**

![20141119_203715-1.jpeg](image/20141119_203715-1.jpeg)

Hello message contains

- Autonomous System Number (ASN)

     -> Must match

- Network of both ends

     -> Must match

     -> EIGRP is not strict about subnet masks matching (unlike OSPF)

          -> R1 s0/0 - 12.0.0.1 /30

          -> R2 s0/0 - 12.0.0.2 /24

- Authentication

     -> Must match

- K-values

     -> Must match

**Change Timers** (hello | holddown)

- To speed up or slow down convergence

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa">ip hello-interval eigrp 1 8</span>

<span style="background-color: #ffaaaa">ip hold-time eigrp 1 16</span>

- There is not command that will show what the hold-time is set to

     -> Except <span style="background-color: #ffaaaa">sh run</span>

     -> Watching the timers on a neighbor can be used to get a hint as to what the hold-time is set to

- Hello-time is used locally

     -> But hold-down time is sent to neighbors

- To check hello-interval

<span style="background-color: #ffaaaa">sh ip eigrp int s0/0 detail</span>

**Passive Interface**

- Stops hello processing on the specific interface

- The network associated with the interface is still advertiseed

R1(config)# <span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa">passive-interface { default | <interface> }</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

- Shows the passive interfaces

**Manual Neighbors**

- Uses unicast

- Stops sending multicast hellos

- Starts sending unicast hellos

- Must be enabled on all routers connected to the same segment

![20141119_203740-1.jpeg](image/20141119_203740-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa">neighbor 10.0.0.2 fa0/0</span>

<span style="background-color: #ffaaaa">neighbor 10.0.0.3 fa0/0</span>

<span style="background-color: #ffaaaa">neighbot 10.0.0.4 fa0/0</span>

<span style="background-color: #ffaaaa">sh ip eigrp nei detail</span>

- Shows if neighbor is dynamic or manual

**Authentication**

- MD5

<span style="background-color: #ffaaaa">key chain CISCO</span>

<span style="background-color: #ffaaaa">key 1</span>

<span style="background-color: #ffaaaa">  key-string CCIE</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa">ip authentication mode eigrp 1 md5</span>

<span style="background-color: #ffaaaa">ip authentication key-chaing eigrp 1 CISCO</span>

<span style="background-color: #ffaaaa">debup ip eigrp</span>

<span style="background-color: #ffaaaa">sh run</span>

<span style="background-color: #ffaaaa">sh ip eigrp int detail</span>

<span style="background-color: #ffaaaa">debug eigrp packets</span>

- Will display all 5 message types

<span style="background-color: #ffaaaa">debug eigrp packets hello</span>

<span style="background-color: #ffaaaa">debug eigrp packets update</span>

<span style="background-color: #ffaaaa">debug eigrp packets query</span>

<span style="background-color: #ffaaaa">debug eigrp packets reply</span>

<span style="background-color: #ffaaaa">debug eigrp packets sia-reply</span>

     -> Stuch In Active

K-values

- Used to increase | decrease the importance of specific metric components

- 0 - 255

- K1

     -> Bandwidth

- K2

     -> Load

- K3

     -> Delay

- K4 and K5

     -> Reliability

- MTU matching is not necessary

     -> MTU is only an issue in redistribution

![20141119_203751-1.jpeg](image/20141119_203751-1.jpeg)

Default K-values

- K1 = 1

- K2 = 0

- K3 = 1

- K4 = 0

- K5 = 0

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa">metric weight <type of service> <k1> <k2> <k3> <k4> <k5></span>

     -> Type of service is always 0

     -> All routers in the AS must have the same k-values

<span style="background-color: #ffaaaa">sh ip protocols</span>

**Topology Database Exchange**

![20141119_203803-1.jpeg](image/20141119_203803-1.jpeg)

Update message

- prefix / prefix-length

- metric component

     -> Bandwidth

     -> Load

     -> Delay

     -> Reliability

- Non-metric components

     -> Number of hops

     -> Router-id

     -> MTU

![20141120_175500-1.jpeg](image/20141120_175500-1.jpeg)

![20141119_203812-1.jpeg](image/20141119_203812-1.jpeg)

![20141119_203827-1.jpeg](image/20141119_203827-1.jpeg)

Questions, will ask you to change K-values so that only delay is considered

     -> 256 * Delay = _______

**EIGRP Terminology**

 - Best route

     -> Successor route

 - Backup route

     -> Feasible successor route

 - Lowest Metric

     -> Feasible Distance (FD)

 - Downstream neighbor metric

     -> Reported Distance (RD)

Procedure to form topology and calculate the best route

 - 1. When update packet is received, the route is installed in the "topology table" and FD / RD is calculated for the route

 - 2. Router is considered "Passive" if it is reachable

     -> Passive - Good

     -> Active - Bad

 - 3. If multiple updates are received from different neighbors to reach the same destination, multiple FD / RD combinations are calculated

 - 4. Lowest FD goes to the routing table

 - 5. Remaining routes may or may not be considered feasible successors depending on passing the feasibility condition

 

Feasibility Condition

 - Backup route's RD < current best route's FD

![20141119_203837-1.jpeg](image/20141119_203837-1.jpeg)

R2's best metric (FD) = 50

R3's best metric (FD) = 40

R1 has two route

 - First route via R2

     -> FD = 70 / RD = 50

 - Second route via R3

     -> FD = 65 / RD = 40

          -> Best route, goes to the routing table

 - FC

     -> 1st route RD < current route's FD

          -> 50 < 65

     -> 1st route is considered a backup route

<span style="background-color: #ffaaaa">sh ip eigrp topology</span>

 - Show all routes except non-feasible routers

     -> Routes the fail the feasibility condition

<span style="background-color: #ffaaaa">sh ip eigrp topology all-links</span>

 - Will display non-feasible routes

<span style="background-color: #ffaaaa">sh ip eigrp topology</span>

 - Route

 - FD

 - RD

<span style="background-color: #ffaaaa">sh ip eigrp topology 1.1.1.1/32</span>

 - All components

     -> FD

     -> RD

     -> Minimum BW

     -> Cumulative Delay

     -> Number of hops

<span style="background-color: #ffaaaa">sh ip eigrp topology zero-successors</span>

 - Shows best route that cannot be added to the routing table

 - RIB failure

 - The route is already in the routing table

     -> From another routing protocol

     -> Or static route

     -> AD for route was better than EIGRP

<span style="background-color: #ffaaaa">sh ip eigrp topology</span>

 - If route shows

     -> FD inaccessable

          -> Route experienced RIB failure

If the FDs for two routes are equal

 - Both routes are placed in the routing table and equal-cost load balancing is performed

**Unequal Cost Load-Balancing**

 - 1. Whenever there are multiple routes with the same FD value, EIGRP performs equal cost load balancing

 - 2. When unequal cost load balancing is activated, EIGRP considers the best and backup routes if their FD fall into a specific range (variance)

 - 3. To calculate the range, a variable called variance is used

     -> 1 - 128

     -> Lowest FD * variance = End point of the range

     -> Lowest FD = Start point of the range

Route 1 FD = 10 (lowest)

Route 2 FD = 15

Route 3 FD = 18

Route 4 FD = 25

Variance = 2

Range = 10 - 20

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> variance 5</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

 - Displays variance

**Traffic Engineering**

 - Metric tuning

 - Most likely to be asked in the lab

**Metric Tuning**

 - Change bandwidth of an interface

 - Change delay of an interface

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> bandwidth <kbps></span>

<span style="background-color: #ffaaaa"> delay <10th of microseconds></span>

All show commands display microseconds

 - Configuration and calculations are done in 10ths of microseconds

 - Change K-values

     -> Changes metric calculation

 - Use offset-lists

**Offset List**

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> offset-list <acl> in | out <offset number> <interface></span>

![20141119_203851-1.jpeg](image/20141119_203851-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">access-list 1 permit 50.0.0.0</span>

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> offset-list 1 in 15 fa0/0</span>

Scenario -> Make sure R1 - R3 route is the best route;  you cannot use offset-lists;

On R1 / R2 / R3:

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> metric weight 0 0 0 1 0 0</span>

![20141119_203901-1.jpeg](image/20141119_203901-1.jpeg)

Cumulative delay of 

 - R1 - R2

     -> 200 microseconds

 - R1 - R3

     -> 20100 microseconds

R1(config)#<span style="background-color: #ffaaaa"> int fa0/0</span>

<span style="background-color: #ffaaaa"> delay 2010</span>

Scenario -> Configure unequal cost load balancing in a manner such that there will be 2:1 load balancing between the R1 - R2 route and the R1 - R3 route

![20141119_203910-1.jpeg](image/20141119_203910-1.jpeg)

![20141119_203918-1.jpeg](image/20141119_203918-1.jpeg)

R1 - R2

 - Delay - 200 microseconds

     -> x = 200 microseconds

R1 - R3

 - Delay - 20000 microseconds

     -> 2x = 400 microseconds

          -> So, 400 - 100 = the delay the interface on R1 needs to be set to

R1(config)# <span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> variance 2</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> delay 30 </span>

**EIGRP Summarization**

 - Per interface

 - Metric of summarized route is the least metric among specific routes

 - AD of summarized route is 5

     -> AD is only locally significant

     -> Other routers will use AD of 90 for the summarized route

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip summary-address eigrp 1 <network> <subnet mask> [AD]</span>

EIGRP Default Routing

 - Network 0.0.0.0

 - Problems

     -> Requires an existing default route in the routing table

     -> It also advertises connected networks

R1(config)# <span style="background-color: #ffaaaa">int lo0</span>

<span style="background-color: #ffaaaa"> ip add 15.0.0.1/24</span>

<span style="background-color: #ffaaaa">router eirgp 1</span>

<span style="background-color: #ffaaaa"> network 15.0.0.0</span>

<span style="background-color: #ffaaaa">ip default-network 15.0.0.0</span>

<span style="background-color: #ffaaaa">sh ip route</span>

 - D* 15.0.0.0

     -> Star means candidate default route

Scenario -> Inject a default route by using summarization

![20141119_203926-1.jpeg](image/20141119_203926-1.jpeg)

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip summary-address eigrp 1 0.0.0.0 0.0.0.0</span>

If a question asks to suppress all routes except the default route, use a leak-map

**Leak Map**

 - This feature allows for a summary route to be sent along with one or more specific routes

![20141119_203933-1.jpeg](image/20141119_203933-1.jpeg)

Scenario -> Summarize 10.0.0.0/24 and 10.0.1.0/24 to both R2 and R3 and make sure R2 uses the path R2-> R3 -> R1 to reach 10.0.1.0

R1(config)# <span style="background-color: #ffaaaa">access-list 1 permit 10.0.1.0 0.0.0.255</span>

<span style="background-color: #ffaaaa">route-map LEAK</span>

<span style="background-color: #ffaaaa"> match ip add 1</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip summary-address eigrp 1 10.0.0.0 255.255.254.0</span>

<span style="background-color: #ffaaaa">int fa0/1</span>

<span style="background-color: #ffaaaa"> ip summary-address eigrp 1 10.0.0.0 255.255.254.0 leak-map LEAK</span>

**EIGRP Filtering**

 - **See RIPv2 notes for details on filtering as the methods are the same**

 - Passive interface

 - Distribute-list

     -> ACL

     -> Prefix-list

     -> Route-map

 - Distance

Scenario -> Filter all routes with FD in the range 400000 to 600000

R1(config)# <span style="background-color: #ffaaaa">route-map FILTER deny 10</span>

<span style="background-color: #ffaaaa"> match metric 500000 +- 100000</span>

<span style="background-color: #ffaaaa">route-map FILTER permit 20</span>

     -> No other config needed as everything else matches and is permitted

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> distribute-list route-map FILTER in</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

**EIGRP Stub Router**

 - It is used to control the query | reply mechanism

 - A stub router never receives any query message

     -> Can be used to control SIA problems

![20141119_203946-1.jpeg](image/20141119_203946-1.jpeg)

On R2 / R3 / R4:

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> eigrp stub [ connected | summary | static | redistribute | receive-only | leak-map <leak> ]</span>

 - Static

     -> Sends all redistributed static routes to the hub

 - Redistributed

     -> Sends all redistributed routes to the hub

 - Recieve-only

     -> Does not send any updates to the hub router

![20141119_203956-1.jpeg](image/20141119_203956-1.jpeg)

Scenario -> Link between R1 and R2 is down;  Traffic from R2 destined for Project should route through R4

 - Leak-map

     -> This feature allows a stub router to leak some learned routes to other neighbors.  This may be used if multiple hubs are connected to the stub router

R3(config)# **access-list 1 permit 50.0.0.0**

**route-map LEAK**

** match ip add 1**

**router eigrp 1**

** eigrp stub leak-map LEAK**

<span style="background-color: #ffaaaa">sh ip eigrp neighbor detail</span>

 - To display if neighbor is a stub router

Instructor comment, "DUAL is a joke.  It's not an algorithm."

**Diffusion Update Algorithm (DUAL)**

 - This is the process of query / reply

 - It is only used if the successor route is down and there is no feasible successor

![20141119_204004-1.jpeg](image/20141119_204004-1.jpeg)

The diagram shows diffusing the query

1. Whenever the successor route is lost and there is no feasible successor, the router sends a query message to multicast address 224.0.0.10

2. An acknowledgment is expected from neighbor within "multicast flow timer"

<span style="background-color: #ffaaaa">sh ip eigrp int</span>

     -> To display the multicast flow timer

3. If one or more neighbors do not reply, a unicast query is sent to those specific neighbors and an acknowledgment is expected within the Retransmission Time Out (RTO)

<span style="background-color: #ffaaaa">sh ip eigrp nei</span>

     -> To display the RTO

4. If 16 consecutive queries are not acknowledged, then the neighbor relationship is broken

5.  If acknowledgment is received from all neighbors, then a timer starts for receiving the reply

 - Timer

     -> 180 seconds

 - SIA Timer

     -> 90 secconds

          -> sia-query is sent

          -> If a reply is received, then the main query timer is reset to 180 seconds

!!! Note to self !!!

!!! This part needs to be verified !!!

query - > 90 seconds

sia-query -> 90 seconds

sia-query -> 90 seconds

sia-query -> 90 seconds

sia-query -> 90 seconds

     -> 90 seconds - then neighbor relationship is broken

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> timers active-time <sec></span>

Router-id in EIGRP does not need to be unique

 - Unless redistribution is being done

     -> The redistribution router needs a unique router-id

     -> The other conflicting router will not accept the updates

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> eigrp router-id <ip add></span>

<span style="background-color: #ffaaaa"> maximum-path <number></span>

<span style="background-color: #ffaaaa"> metric maximum-hops <number></span>

On stub router:

<span style="background-color: #ffaaaa">sh ip protocol | in Stub</span>

 - Stub, receive-only

On peer:

<span style="background-color: #ffaaaa">sh ip eigrp neighbor detail</span>

 - Receive-Only Peer Advertising (No) Routes

 - Suppressing queries

Verify unequal cost load balancing

<span style="background-color: #ffaaaa">sh ip route 24.0.0.0 255.0.0.0</span>

 - traffic share count is 1

 - traffic share count is 4
