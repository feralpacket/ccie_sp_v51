# EIGRP - Class Notes - Part 2

- Note: Split on 6 April 2024, because the original export was 25.5 MB in size. Github file size limit is 25 MB.

**Offset List**

router eigrp 1

 offset-list <acl> in | out <offset number> <interface>

![20141119_203851-1.jpeg](image/20141119_203851-1.jpeg)

R1(config)# access-list 1 permit 50.0.0.0

router eigrp 1

 offset-list 1 in 15 fa0/0

Scenario -> Make sure R1 - R3 route is the best route;  you cannot use offset-lists;

On R1 / R2 / R3:

router eigrp 1

 metric weight 0 0 0 1 0 0

![20141119_203901-1.jpeg](image/20141119_203901-1.jpeg)

Cumulative delay of 

 - R1 - R2

     -> 200 microseconds

 - R1 - R3

     -> 20100 microseconds

R1(config)# int fa0/0

 delay 2010

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

R1(config)# router eigrp 1

 variance 2

int s0/0

 delay 30 

**EIGRP Summarization**

 - Per interface

 - Metric of summarized route is the least metric among specific routes

 - AD of summarized route is 5

     -> AD is only locally significant

     -> Other routers will use AD of 90 for the summarized route

int fa0/0

 ip summary-address eigrp 1 <network> <subnet mask> [AD]

EIGRP Default Routing

 - Network 0.0.0.0

 - Problems

     -> Requires an existing default route in the routing table

     -> It also advertises connected networks

R1(config)# int lo0

 ip add 15.0.0.1/24

router eirgp 1

 network 15.0.0.0

ip default-network 15.0.0.0

sh ip route

 - D* 15.0.0.0

     -> Star means candidate default route

Scenario -> Inject a default route by using summarization

![20141119_203926-1.jpeg](image/20141119_203926-1.jpeg)

int fa0/0

 ip summary-address eigrp 1 0.0.0.0 0.0.0.0

If a question asks to suppress all routes except the default route, use a leak-map

**Leak Map**

 - This feature allows for a summary route to be sent along with one or more specific routes

![20141119_203933-1.jpeg](image/20141119_203933-1.jpeg)

Scenario -> Summarize 10.0.0.0/24 and 10.0.1.0/24 to both R2 and R3 and make sure R2 uses the path R2-> R3 -> R1 to reach 10.0.1.0

R1(config)# access-list 1 permit 10.0.1.0 0.0.0.255

route-map LEAK

 match ip add 1

int fa0/0

 ip summary-address eigrp 1 10.0.0.0 255.255.254.0

int fa0/1

 ip summary-address eigrp 1 10.0.0.0 255.255.254.0 leak-map LEAK

**EIGRP Filtering**

 - **See RIPv2 notes for details on filtering as the methods are the same**

 - Passive interface

 - Distribute-list

     -> ACL

     -> Prefix-list

     -> Route-map

 - Distance

Scenario -> Filter all routes with FD in the range 400000 to 600000

R1(config)# route-map FILTER deny 10

 match metric 500000 +- 100000

route-map FILTER permit 20

     -> No other config needed as everything else matches and is permitted

router eigrp 1

 distribute-list route-map FILTER in

sh ip protocols

**EIGRP Stub Router**

 - It is used to control the query | reply mechanism

 - A stub router never receives any query message

     -> Can be used to control SIA problems

![20141119_203946-1.jpeg](image/20141119_203946-1.jpeg)

On R2 / R3 / R4:

router eigrp 1

 eigrp stub [ connected | summary | static | redistribute | receive-only | leak-map <leak> ]

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

sh ip eigrp neighbor detail

 - To display if neighbor is a stub router

Instructor comment, "DUAL is a joke.  It's not an algorithm."

**Diffusion Update Algorithm (DUAL)**

 - This is the process of query / reply

 - It is only used if the successor route is down and there is no feasible successor

![20141119_204004-1.jpeg](image/20141119_204004-1.jpeg)

The diagram shows diffusing the query

1. Whenever the successor route is lost and there is no feasible successor, the router sends a query message to multicast address 224.0.0.10

2. An acknowledgment is expected from neighbor within "multicast flow timer"

sh ip eigrp int

     -> To display the multicast flow timer

3. If one or more neighbors do not reply, a unicast query is sent to those specific neighbors and an acknowledgment is expected within the Retransmission Time Out (RTO)

sh ip eigrp nei

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

router eigrp 1

 timers active-time <sec>

Router-id in EIGRP does not need to be unique

 - Unless redistribution is being done

     -> The redistribution router needs a unique router-id

     -> The other conflicting router will not accept the updates

router eigrp 1

 eigrp router-id <ip add>

 maximum-path <number>

 metric maximum-hops <number>

On stub router:

sh ip protocol | in Stub

 - Stub, receive-only

On peer:

sh ip eigrp neighbor detail

 - Receive-Only Peer Advertising (No) Routes

 - Suppressing queries

Verify unequal cost load balancing

sh ip route 24.0.0.0 255.0.0.0

 - traffic share count is 1

 - traffic share count is 4
