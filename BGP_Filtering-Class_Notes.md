# BGP Filtering - Class Notes

`classnotes`

**BGP Filtering** (27 Aug 2014)
Lab:  BGP 1 - 6

IP address based (numerical):
 - <span style="background-color: #ffaaaa">neighbor <ip add> prefix-list <name> in | out</span>
 - <span style="background-color: #ffaaaa">neighbor <ip add> distribute-list <acl> in | out</span>
 - <span style="background-color: #ffaaaa">neighbor <ip add> route-map <name> in | out</span>

AS Path based (string):
 - <span style="background-color: #ffaaaa">neighbor <ip add> filter-list <as-path acl number> in | out</span>
     -> e.g. - 100 200 300 i

Regular Expressions are required to match and filter AS-PATH

ACL filtering in BGP with distribute-list
 - Use extended ACLs

![20141001_163544-1.jpeg](image/20141001_163544-1.jpeg)

![20141001_163551-1.jpeg](image/20141001_163551-1.jpeg)

Source doesn't need to be specified in the ACL because the ACL is referenced in the neighbor command.

<span style="background-color: #ffaaaa">access-list <number> permit | deny <protocol> <network> <wildcard> <subnet> <wildcard></span>

Filter-> Any network starting with 10.x.x.x and subnet mask of 255.255.255.0.

<span style="background-color: #ffaaaa">access-list 100 deny ip 10.0.0.0 0.255.255.255 255.255.255.0 0.0.0.0</span>
<span style="background-color: #ffaaaa">access-list 100 permit ip any any</span>

![20141002_113450-1.jpeg](image/20141002_113450-1.jpeg)

<span style="background-color: #ffaaaa">access-list 100 deny ip 10.0.0.0 0.255.255.255 255.0.0.0 0.255.0.0</span>
<span style="background-color: #ffaaaa">access-list 100 permit ip any any</span>

![20141002_113506-1.jpeg](image/20141002_113506-1.jpeg)

Prefix-list and distribute-list cannot be applied to the same neighbor in the same direstion.
      -> A route-map with multiple statements can get around this.

<span style="background-color: #ffaaaa">neighbor <ip add> filter-list <as-path acl> in | out</span>
 - as-path ACL can patch the as--path by using regular expressions

<span style="background-color: #ffaaaa">ip as-path access-list <number> permit | deny REXEXP</span>

REGEXP works on character strings
 - Every string has a start of the string and an end of the string
     -> ^ - start
     -> $ - end

^100 200$ - 9 character string (including the space between 100 and 200)

<span style="background-color: #ffaaaa">ip as-path access-list 1 deny 100</span>
 - Will match
     -> 100
     -> 1100
     -> 1008

REGEXP Operators / Delimiters
     ? - 0 or 1 occurrence of the character
     . - any character 0 - 9, but not a space
     _ - start of string, end of string, or a space
     ^ - start of string
     $ - end of string
     [] - specific single character range, [123], [1-5]
     ^$ - locally originated
     + - 1 or more occurrences
     * - 0 or more occurrences
     .* - any number

<span style="background-color: #ffaaaa">ip  as-path access-list 1 deny ^100_</span>
 - Will match:
     -> 100 200          - yes
     -> 100                - yes
     -> 100 800 900    - yes
     -> 1008 500         - no

^1[2-5]8$
 - Will match:
     -> 128
     -> 138
     -> 148
     -> 158

5?
 - 5 will be there one time or will be missing

Match -> 100 200 300 or 100 300
     -> ^100_200_300$
     -> ^100_300$
          - or -
     -> ^100(_200)?_300$

Match -> ^100$, ^100_100$, and ^100_100_100$
     -> ^(100_)+
          - or -
     -> (100_)+$

<span style="background-color: #ffaaaa">sh ip bgp regexp _100_</span>

**BGP Community**
 - Standard Community
     -> 32-bit value
 - Extended Community
     -> 64-bit value
 - Can be sent with updates and can be used to change path attributes
 - Every router must have send-community set for all neighbors, otherwise the community values will be removed

![20141002_113545-1.jpeg](image/20141002_113545-1.jpeg)

Match a community and take action
     -> if 100:50, increase local preference to 500

Set community
 - In a route-map

Match community
 - Community list, called in a route-map

<span style="background-color: #ffaaaa">ip community-list { 1-99 | 100-500 } permit | deny <community value></span>
 - 1 - 99
     -> Standard ACL
     -> Simple numeric value
 - 100 - 500
     -> Extended ACL
     -> REGEXP can be used

R3(config)# <span style="background-color: #ffaaaa">access-list 1 permit 5.5.5.5</span>
<span style="background-color: #ffaaaa">route-map COMMUNITY</span>
 <span style="background-color: #ffaaaa">match ip add 1</span>
 <span style="background-color: #ffaaaa">set community 100:50</span>
<span style="background-color: #ffaaaa">route-map COMMUNITY permit 20</span>
<span style="background-color: #ffaaaa">router bgp 200</span>
 <span style="background-color: #ffaaaa">neighbor 13.0.0.1 route-map COMMUNITY out</span>
 <span style="background-color: #ffaaaa">neighbor 13.0.0.1 send-community</span>

<span style="background-color: #ffaaaa">clear ip bgp 13.0.0.1 out</span>
<span style="background-color: #ffaaaa">clear ip bgp 13.0.0.1 in</span>
<span style="background-color: #ffaaaa">clear ip bgp 13.0.0.1</span>

R1(config)# <span style="background-color: #ffaaaa">ip community-list 1 permite 100:50</span>
 <span style="background-color: #ffaaaa">route-map MATCH_COMM</span>
  <span style="background-color: #ffaaaa">match community 1</span>
  <span style="background-color: #ffaaaa">set local-preference 500</span>
<span style="background-color: #ffaaaa">route-map MATCH_COM permit 20</span>
<span style="background-color: #ffaaaa">router bgp 100</span>
 <span style="background-color: #ffaaaa">neighbor 13.0.0.3 route-map MATCH_COMM</span>

<span style="background-color: #ffaaaa">sh ip bgp 5.5.5.5</span>
 - Community: ______
     -> will display 32-bit number
     -> old format

<span style="background-color: #ffaaaa">ip bgp-community new-format</span>

<span style="background-color: #ffaaaa">sh ip bgp 5.5.5.5</span>
 - Community: 100:50
     -> new format

![20141002_113600-1.jpeg](image/20141002_113600-1.jpeg)

**Well Known Community Values**

no-export
 - Update will not be sent to any eBGP neighbor

no-advertise
 - Update will not be sent to any iBGP or eBGP neighbor

LOCAL-AS
 - Update will not be sent to another confederation

additive
 - Will add community to the existing list

R1(config)# <span style="background-color: #ffaaaa">route-map COMMUNITY</span>
 <span style="background-color: #ffaaaa">set community no-export</span>

<span style="background-color: #ffaaaa">set community 100:50 additive</span>

![20141002_113614-1.jpeg](image/20141002_113614-1.jpeg)

**Deleting Community Values**

R2(config)# <span style="background-color: #ffaaaa">ip community-list 1 permit 100:60</span>
<span style="background-color: #ffaaaa">route-map DELETE</span>
 <span style="background-color: #ffaaaa">set com-list 1 delete</span>
<span style="background-color: #ffaaaa">router bgp 200</span>
 <span style="background-color: #ffaaaa">neighbor 23.0.0.3 route-map DELETE</span>
 <span style="background-color: #ffaaaa">neighbor 23.0.0.3 send-community</span>

![20141002_113630-1.jpeg](image/20141002_113630-1.jpeg)

**BGP Remove Private-AS**
 - AS
     -> 2 bytes
     -> 1 - 65535
     -> 64512 - 65534
          -> Private AS numbers
 - Removes any AS in the private AS range before sending updates to a neighbor

R3(config)# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor 36.0.0.6 remove-private-as</span>

![20141002_113641-1.jpeg](image/20141002_113641-1.jpeg)

**BGP Default Routing**
 1. <span style="background-color: #ffaaaa">network 0.0.0.0 mask 0.0.0.0</span>
     -> Injects a default route to all neighbors
     -> Needs a default route to be present in the routing table
 2. <span style="background-color: #ffaaaa">neighbor <ip add> default-originate [route-map <name>]</span>
     -> Per neighbor
     -> Default route does not need to be present in the routing table

**BGP Dampening**
 - It is a procedure to suppress flapping routes
 - It uses a penalty system where on every flap, a penalty value 1000 is associated with the route
 - The moment the value is associated with the route, it starts decreasing on an exponential decay rate
 - If the route gains a penalty value of 2000, the route is suppressed
     -> Value called suppress limit
     -> Can be changed
 - Value has to decrease to 750 before the route is no longer suppressed
     -> Value called the reuse limit

suppress limit
     -> {P(0)}

reuse limit
     -> {P(t)}
          -> t in minutes

half-life
     -> The time in minutes the router will take to reduce the penalty to half of the suppress limit
     -> default is 15 minutes

![20141002_113651-1.jpeg](image/20141002_113651-1.jpeg)

Max suppress time
     -> 4 * half-life
          -> 60 minutes by default

Scenario -> Configure BGP Dampening on R1 so that if a route flaps 6 times, it is suppressed;  the penalty should reach 3000 after 40 minutes;  the route should be reusable after the penalty reaches 2000.   

     suppress limit -> 6000
     half-life -> 40 minutes
     reuse limit -> 2000
     max suppress time -> 160 minutes

<span style="background-color: #ffaaaa">router bgp 100</span>
 <span style="background-color: #ffaaaa">bgp dampening 40 2000 6000 160</span>

Scenario -> Dampen route 5.5.5.5 if it flaps 3 times, unsuppress it when the penalty reaches 1000, half-life is 20 minutes

R1(config)# <span style="background-color: #ffaaaa">access-list 1 permit 5.5.5.5</span>
 <span style="background-color: #ffaaaa">route-map DAMPENING</span>
  <span style="background-color: #ffaaaa">match ip add 1</span>
  <span style="background-color: #ffaaaa">set dampening 20 1000 3000 80</span>
 <span style="background-color: #ffaaaa">route-map DAMPENING permit 20</span>
<span style="background-color: #ffaaaa">router bgp 100</span>
 <span style="background-color: #ffaaaa">bgp dampening route-map DAMPENING</span>

![20141002_113702-1.jpeg](image/20141002_113702-1.jpeg)

<span style="background-color: #ffaaaa">sh ip bgp dampening parameters</span>
<span style="background-color: #ffaaaa">sh ip bgp dampening dampened-paths</span>
<span style="background-color: #ffaaaa">sh ip bgp dampening flap-statistics</span>

<span style="background-color: #ffaaaa">sh ip bgp</span>
 d> 5.5.5.5          -> dampened, currently up
 h> 5.5.5.5          -> history, another router suppressed, currently down

**BGP Timers**
 - Keepalive
     -> 60 seconds
 - Holddown
     -> 180 seconds

<span style="background-color: #ffaaaa">router bgp 100</span>
 <span style="background-color: #ffaaaa">bgp timer 30 90</span>

**Batch Updates**
 - BGP holds the new updates to be sent to a neighbor according to the following timer
     -> iBGP - 5 seconds
     -> eBGP - 30 seconds
 - Also called advertise-interval
 - If set to 0, updates are sent immediately

<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor <ip add> advertisement-interval <seconds></span>

**BGP Scan Time**
 - By default, BGP scans the BGP table for changes every 60 seconds

<span style="background-color: #ffaaaa">router bgp 100</span>
 <span style="background-color: #ffaaaa">bgp scan-time <seconds></span>

![20141002_113714-1.jpeg](image/20141002_113714-1.jpeg)

**BGP AS-Override / AllowAS-In**
 - If the link between R4 <-> R5 is goes down, R6 will not be able to reach R7

R1(config)# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor 13.0.0.2 as-override</span>
 <span style="background-color: #ffaaaa">neighbor 13.0.0.3 as-override</span>
     -> update to R3
          -> 6.6.6.6          100 100 i

     - or -

R2(config)# <span style="background-color: #ffaaaa">router bgp 200</span>
 <span style="background-color: #ffaaaa">neighbor 12.0.0.1 allowas-in</span>

R3(config)# <span style="background-color: #ffaaaa">router bgp 200</span>
 <span style="background-color: #ffaaaa">neighbor 13.0.0.1 allowas-in</span>

Allowas-in
 - Accept updates that contain our own AS in the path
 - Should be used as a temporary solution only while the link between R4 <-> R5 is down
