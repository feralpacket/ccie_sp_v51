# Bidirectional Forwarding Detection (BFD) - Class Notes

**Bidirectional Forwarding Detection (BFD)** (19 Sept 2014) - It can check the connection status between two layer 3 devices in the periods of milliseconds and inform the upper layer routing protocol to take action

 - Supported protocols

     -> OSPF

     -> IS-IS

     -> EIGRP

     -> BGP

**Configuration**

 - Keepalive parameters

     -> Sending / receiving frequency

     -> Multiplier

 - Associating it with routing protocols

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> bfd interval <ms> min-rc <ms> multiplier <number></span>

EIGRP

<span style="background-color: #ffaaaa">router eigrp 1</span>

<span style="background-color: #ffaaaa"> bfd { interface <int> | all-interfaces }</span>

OSPF

<span style="background-color: #ffaaaa">router ospf 1</span>

<span style="background-color: #ffaaaa"> bfd all-interfaces</span>

     - or -

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip ospf bfd [disabled]</span>

IS-IS

<span style="background-color: #ffaaaa">router isis ABC</span>

<span style="background-color: #ffaaaa"> bdf all-interfaces</span>

     - or -

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> isis bfd [disable]</span>

BGP

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> neighbor <ip add> fall-over bfd</span>

<span style="background-color: #ffaaaa">sh bfd neighbors</span>

<span style="background-color: #ffaaaa">sh bfd neighbors detail</span>

BFD only works if both sides are configured
