# QoS Class Based Weighted Fair Queueing (CBWFQ) - Class Notes

**QoS Class Based Weighted Fair Queueing (CBWFQ)** (4 Sept 2014)

Lab: QoS 1

 - Uses MQC

 - Number of queues is equal to the number of class-maps that are active

 - Max number of queues is 64

     -> queue-length = 64

 - If the queue is full, then by default tail drop is used

     -> Can use Random Early Detection (RED)

<span style="background-color: #ffaaaa">class-map VOICE</span>

<span style="background-color: #ffaaaa"> match ip rtp 16384 16384</span>

<span style="background-color: #ffaaaa">class-map FTP</span>

<span style="background-color: #ffaaaa"> match protocol ftp</span>

<span style="background-color: #ffaaaa">policy-map POLICY1</span>

<span style="background-color: #ffaaaa"> class VOICE</span>

<span style="background-color: #ffaaaa">  bandwidth { <value> | percent <value> }</span>

<span style="background-color: #ffaaaa">  bandwidth percent 60</span>

<span style="background-color: #ffaaaa"> class FTP</span>

<span style="background-color: #ffaaaa">  bandwidth percent 15</span>

<span style="background-color: #ffaaaa"> class class-default</span>

![20141015_154530-1.jpeg](image/20141015_154530-1.jpeg)

int s0/0

 bandwidth 2000

 service-policy output POLICY1

Bandwidth command hs to be used in the same format in all classes

 - Either all be <value> or all be percent <value>

 - Bandwidth is minimum guaranteed rate but can go above the configured rate if there is no congestion

     -> If incoming packet rate is lower than the outgoing interface clock rate
