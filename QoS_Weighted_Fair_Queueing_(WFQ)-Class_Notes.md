# QoS Weighted Fair Queueing (WFQ) - Class Notes

**QoS Weighted Fair Queueing (WFQ)** (4 SEpt 2014)

Lab: QoS 1

 - Does not use MQC

 - Uses packet flows

     -> The collection of packets with the same header information

     -> Source / destination IP addresses

     -> Source / destination port numbers

     -> Protocol

     -> Precedence value

 - Every flow creates a software queue

 - Maximum number of queues is 4096

 - Flows are treated according to two things

     -> Precedence

     -> Packet length

 - Higher precedence and smaller packets are preferred over lower precedence and bigger packets

e.g. -

 - Flow1

     -> src IP - 10.0.0.1

     -> dst IP - 15.0.0.1

     -> src port - 4001

     -> dst port - 80

     -> precedence - 5

 - Flow2

     -> src IP - 10.0.0.1

     -> dst IP - 15.0.0.1

     -> src port - 5000

     -> dst port - 80

     -> precendence - 6

![20141015_154505-1.jpeg](image/20141015_154505-1.jpeg)

**WFQ Schedular Mechanism**

![20141015_154514-1.jpeg](image/20141015_154514-1.jpeg)

Sequence Number (SN) (Finish time) = old sequence number + ( packet length * weight )

 - Lower is better

Weight = 32384 / IPP + 1

Scheduler selects the packet with the lowest sequence number among flows

 - Within a flow, packets are FIFO

 - Between flows, the scheduler selects the next packet based on sequence numbers

**Congestive Discard Threshold (CDT)**

 - Drop mechanism for WFQ

 - If the queue is full and a new packet arrives for the queuethe the sequence number is calculated for the packet and the following happens

     -> If the SN of the packet is the highest, then the packet is dropped

     -> If there is another in any queue having a higher SN, then the packet with the higher SN is dropped

CDT value = queue length = 64 by default

Dynamic queues = 256 by default

RSVP queues = 0 by default

     -> Can be created to provide very low weight reserved queues for RSVP traffic

WFQ Packet Limit = 4096 packets

WFQ is the default for serial interfaces with clock rates lower than 2000kbps

 - For other interfaces, FIFO is the default

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> fair-queue [congestive-discard threshold [dynamic-queues [reservable-queues]]]</span>

<span style="background-color: #ffaaaa">hold-queue <number> out</span>

     -> Max number of packets that can be held in memory

<span style="background-color: #ffaaaa">sh queueing fair</span>

<span style="background-color: #ffaaaa">sh int fa0/0</span>
