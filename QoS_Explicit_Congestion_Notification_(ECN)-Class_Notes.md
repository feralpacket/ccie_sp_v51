# QoS Explicit Congestion Notification (ECN) - Class Notes

**QoS Explicit Congestion Notification (ECN)** (4 Sept 2014)Lab: QoS 1

![20141015_154633-1.jpeg](image/20141015_154633-1.jpeg)

![20141015_154651-1.jpeg](image/20141015_154651-1.jpeg)

**Congestion Experienced (CE)**

 - Default value of 0

Upon receiving a TCP packet with CE bit set to 1, the TCP congestion window size (CWND) is reduced in half

<span style="background-color: #ffaaaa">policy-map <name></span>

<span style="background-color: #ffaaaa"> class <class></span>

<span style="background-color: #ffaaaa">  random-detect</span>

<span style="background-color: #ffaaaa">  random-detect dscp-based</span>

<span style="background-color: #ffaaaa">  random-detect ecn</span>
