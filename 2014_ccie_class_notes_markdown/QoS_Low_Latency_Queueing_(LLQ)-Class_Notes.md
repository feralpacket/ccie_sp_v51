# QoS Low Latency Queueing (LLQ) - Class Notes

**QoS Low Latency Queueing (LLQ)** (4 Sept 2014)Lab: QoS 1

![20141015_154540-1.jpeg](image/20141015_154540-1.jpeg)

<span style="background-color: #ffaaaa">polciy-map POLICY1</span>

<span style="background-color: #ffaaaa"> class VOICE</span>

<span style="background-color: #ffaaaa">  priority percent 60</span>

<span style="background-color: #ffaaaa"> class FTP</span>

<span style="background-color: #ffaaaa">  bandwidth percent 15</span>

**Priority**

 - Bandwidth is the minimum guaranteed, but it is also the maximum limit

     -> Above this limit, traffic will be dropped

Between priority and bandwidth, the format can be mixed

<span style="background-color: #ffaaaa">class VOICE</span>

<span style="background-color: #ffaaaa"> priority percent 60</span>

<span style="background-color: #ffaaaa">class FTP</span>

<span style="background-color: #ffaaaa"> bandwidth 300</span>

**Remaining Percent**

 - e.g. - Voice -> 60% of bandwidth

              FTP -> 30% of the remaining bandwidth

 - Remaining bandwidth = Actual BW - (any priority bandwidth + class-default bandwidth)

 - 200 - ( 1200 + 500 ) = 300 kbps

<span style="background-color: #ffaaaa">policy-map POLICY1</span>

<span style="background-color: #ffaaaa"> class VOICE</span>

<span style="background-color: #ffaaaa">  priority percent 60</span>

<span style="background-color: #ffaaaa"> class FTP</span>

<span style="background-color: #ffaaaa">  bandwidth remaining-percent 30</span>
