# Quality of Service (QoS) - Class Notes

**Quality of Service (QoS)** (4 Sept 2014)

Lab: QoS 1

 - Managed unfairness

 - Problem with QoS is you will not see any results unless there is traffic congestion

     -> It's hard to generate a variety of different types of traffic

     -> During the lab, they are not expecting to you to generate traffic to verify the configuration

     -> They will only verify your configuration

Instructor comment, "Bandwidth and sex, there is never enough."

Bandwidth

     -> Traffic rate (width of the pipe)

Propagation Delay

     -> The time is takes to get from point A to point B (length of the pipe)

Serialization Delay

     -> The time it takes to send traffic from the interface to the media

     -> Delay = Amount of data / clock rate

          -> Clock rate of the interface

Jitter

     -> Variance in delay

**QoS Architecture**

 - Integrated QoS

     -> End-to-end QoS

 - Differential Service QoS

     -> Per-hop QoS

**Integrated Services QoS**

![20141015_133133-1.jpeg](image/20141015_133133-1.jpeg)

**Resource Reservation Protocol (RSVP)**

 - Path message set to destination and waits for a reservation message response before sending traffic

 - If any router disagrees with Path, it will be dropped and no traffic will be sent

**Diff Serv**

 - The first hop router will mark the important traffic and hope that other routers agrees with the markings

 - Mark the traffic and hope for the best

 - Per hop behavior (PHB)

**Differential Service**

 - Marking of packet

     -> Layer 3

          -> IP header

          -> IP Precedence / Differential Service Code Point (DSCP)

     -> Layer 2

          -> Frame header

          -> Class of Service (CoS)

               -> 802.1q tag

IP Header

 - 8 bit field called Type of Service (ToS)

![20141015_133148-1.jpeg](image/20141015_133148-1.jpeg)

**IP Precedence (IPP)**

 - Higher is better

     -> 0 - Routine

     -> 1 - Priority

          -> Data

     -> 2 - Immediate

          -> Video signaling

     -> 3 - Flash

          -> Voice signaling

     -> 4 - Flash Override

          -> Video stream

     -> 5 - Critical

          -> Voice stream

     -> 6 - Internetwork

          -> Management protocol

     -> 7 - Network

          -> Management protocol

![20141015_133159-1.jpeg](image/20141015_133159-1.jpeg)

**Priority Values**

     -> 0 - Default

     -> 1 - Assured Forwarding (AF)

     -> 2 - Assured Forwarding (AF)

     -> 3 - Assured Forwarding (AF)

     -> 4 - Assured Forwarding (AF)

     -> 5 - Expedited Forwarding (EF)

     -> 6 - Internetwork

     -> 7 - Network

**Drop Precedence**

 - Only works with AF

 - 2^2 = 4 Drop Precedence Values

     -> 0 0 = 0 -> Never used

     -> 0 1 = 1

     -> 1 0 = 2

     -> 1 1 = 3

AF X Y

 - X - Priority Value

     -> Higher is better

 - Y - Drop Precedence

     -> Lower is better

AF1     ->     AF11     AF12     AF13

AF2     ->     AF21     AF22     AF23

AF3     ->     AF31     AF32     AF33

AF4     ->     AF41     AF42     AF43

AF23     -> Priority 2                         -> 010

                 Drop Precendence 3         -> 11

                 Last bit                            -> 0

                 DSCP Value - 010110 = 22 in decimal

                    -> Don't need to know for the lab, but need to know for the written

AF X Y = ( 8X + 2Y )

AF23   = ( 8*2 + 2*3 ) = 22

**Modular QoS CLI (MQC)**

 - Old configuration was directly in global config

 - Class-map

     -> Matching procedure

     -> Classifying the data

 - Policy-map

     -> Defines the action

          -> Marking

          -> Queueing

          -> Shaping

          -> Policing

          -> Dropping

 - Service-policy

     -> Implement the policy-map

     -> Inside interface configuration

     -> In | out direction

Class-map

<span style="background-color: #ffaaaa">class-map [match-any | match-all] <name></span>

<span style="background-color: #ffaaaa"> match <condition></span>

<span style="background-color: #ffaaaa">match [ip] precedence <up to 4 comma separated values></span>

 - ip option

     -> If used, then only IPv4 packets are checked

     -> Otherwise, both IPv4 and IPv6 packets are checked

<span style="background-color: #ffaaaa">match [ip] dscp <up to 8 different vlaues></span>

     -> <span style="background-color: #ffaaaa">match dscp AF11 AF12</span>

<span style="background-color: #ffaaaa">match cos <up to 4 values></span>

<span style="background-color: #ffaaaa">match address-group <acl> [ip address]</span>

     -> IP addresses and port numbers

<span style="background-color: #ffaaaa">match source-address mac <mac address></span>

<span style="background-color: #ffaaaa">match destination-address mac <mac address></span>

<span style="background-color: #ffaaaa">match mpls experimental [topmost] <value></span>

Network Based Application Recognition (NBAR)

 - Performs deep packet inspection

<span style="background-color: #ffaaaa">match protocol <name></span>

<span style="background-color: #ffaaaa">match packet length min <value> max <value></span>

     - Only min or max needs to be specified, or both

<span style="background-color: #ffaaaa">match input-interface <int></span>

Voice Traffic

 - Real-time Transport Protocol (RTP)

     -> Port numbers

          -> 16384 - 32767

          -> Even port number

               -> Voice traffic

          -> Odd port number

               -> Voice signal

     -> Best way to match is to use NBAR

<span style="background-color: #ffaaaa">match protocol rtp audio</span>

     - or -

<span style="background-color: #ffaaaa">match ip rtp <start of range> <range></span>

     - start of range is a port number

<span style="background-color: #ffaaaa">match ip rtp 100 50</span>

     -> Ports 100 - 150

<span style="background-color: #ffaaaa">match ip rtp 16384 16384</span>

     -> Matches the entire RTP port range

<span style="background-color: #ffaaaa">match not <condition></span>

<span style="background-color: #ffaaaa">match class <class-map name></span>

     -> For nesting class-maps for advanced matching scenarios

Policy-map

<span style="background-color: #ffaaaa">policy-map <name></span>

<span style="background-color: #ffaaaa"> class <name></span>

<span style="background-color: #ffaaaa">  <action></span>

<span style="background-color: #ffaaaa"> class <name></span>

<span style="background-color: #ffaaaa">  <action></span>

<span style="background-color: #ffaaaa"> class class-default</span>

<span style="background-color: #ffaaaa">  <action></span>

     -> All traffic not matching other classes

**Class-map Actions**

 - Marking

 - Queueing

 - Shaping

 - Policing

 - Dropping

 - Random-detect

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> service policy { input | output } <name of policy-map></span>

<span style="background-color: #ffaaaa">sh policy-map interface <int></span>

**Classification and Marking**

![20141015_133215-1.jpeg](image/20141015_133215-1.jpeg)

Scenario -> R1 is the edge router.  Mark all incoming voice traffic with DSCP EF.  Mark all incoming http traffic with AF31.  All other traffic should be marked "default".

R1(config)# <span style="background-color: #ffaaaa">class-map CLASS1</span>

<span style="background-color: #ffaaaa"> match protocol rtp audio</span>

<span style="background-color: #ffaaaa">class-map CLASS2</span>

<span style="background-color: #ffaaaa"> match protocol http</span>

<span style="background-color: #ffaaaa">policy-map POLICY1</span>

<span style="background-color: #ffaaaa"> class CLASS1</span>

<span style="background-color: #ffaaaa">  set dscp ef</span>

<span style="background-color: #ffaaaa"> class CLASS2</span>

<span style="background-color: #ffaaaa">  set dscp AF31</span>

<span style="background-color: #ffaaaa"> class class-default</span>

<span style="background-color: #ffaaaa">  set dscp default</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> service-policy input POLICY1</span>

<span style="background-color: #ffaaaa">sh policy-map int s0/0</span>

**Queueing**

 - Congestion Management

![20141015_133225-1.jpeg](image/20141015_133225-1.jpeg)

e.g. - Clock rate 64000bps

     -> But if traffic rate is 96000bps

![20141015_133232-1.jpeg](image/20141015_133232-1.jpeg)

**Congestion**

 - If there HW queue is full and more traffic is waiting to be sent

**Software Queue**

 - Collection of pointers to the memory locations where the packets are located

 - FIFO queue

![20141015_133243-1.jpeg](image/20141015_133243-1.jpeg)

**Tail Drop**

 - What happens when the HW queue and Software queue is full

**To change the hardward queue-length**

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> tx-ring-limit <number></span>

**To change the software queue-length**

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> hard-queue <number> out</span>

**To display the HW Queue**

<span style="background-color: #ffaaaa">sh controllers s0/0</span>

 . . .

 . . . 

 . . .

tx-limit x(y)

     -> x - 0 -> default sofware queue

              1 -> advanced queueing

     -> y - queue-length

tx-limit 0(16)

     -> Default for most routers

tx-limit 0(128)

     -> IOU routers

          -> This could cause unexpected results if QoS was first tested in an IOU environment
