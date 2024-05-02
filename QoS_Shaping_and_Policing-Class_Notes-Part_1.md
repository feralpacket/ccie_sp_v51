# QoS Shaping and Policing - Class Notes - Part 1

- Note: Split on 7 April 2024, because the original export was 32 MB in size. Github file size limit is 25 MB.

**QoS Shaping and Policing** (5 Sept 2014)

 - Shaping

      -> Delaying the traffic to conform with service level agreement

      -> Outgoing

 - Policing

      -> Punishing the traffic to enforce the service level agreement

      -> Incoming
![20141016_113426-1.jpeg](image/20141016_113426-1.jpeg)

 A router cannot send traffic at lower rates then the clock rate 

 Contract rate

 - Committed information rate (CIR)

      -> e.g. - 64 kbps

 Actual rate

 - Clock rate

      -> e.g. - 128 kbps

![20141016_113433-1.jpeg](image/20141016_113433-1.jpeg)

 Problem

 - 500ms of delay is added to all network traffic

![20141016_113447-1.jpeg](image/20141016_113447-1.jpeg)

 In every t sub c, only half of the time traffic is sent as a burst 

 b sub c = t sub c * CIR 

 128000 bps

 - In one t sub c = 12800 bits

              b sub c = 6400 bits

![20141016_113500-1.jpeg](image/20141016_113500-1.jpeg)

 b sub c bucket

 - When the bucket is empty, no traffic can be sent

 - Bucket will be refilled at the start of every t sub c (interval)2 

![20141016_113508-1.jpeg](image/20141016_113508-1.jpeg)

If the bucket is not empty at the start of a t sub c, the excess tokens overflow into the b sub e bucket

![20141016_113518-1.jpeg](image/20141016_113518-1.jpeg)

If CIR is configured but b sub c and b sub e are not configured, the default values are used

If the contract rate is less than 320kbps, then the default b sub c is 8000 bps

![20141016_113533-1.jpeg](image/20141016_113533-1.jpeg)

If the contract rage is more than or equal to 320kbps, then the default t sub c is 25ms

![20141016_113538-1.jpeg](image/20141016_113538-1.jpeg)

policy-map <name>

 class <name>

  shape average <cir> [ b sub c ] [ b sub e ]

e.g. - Shape all http traffic at the rate of 128kbps

     -> Use the burst size of 16000 bit / t sub c

class-map HTTP

 match protocol http

policy-map SHAPE

 class HTTP

  shape average 12800 1600

int s0/0

 service-policy output SHAPE

**Dual Rate Shaping**

 - CIR

 - Peak Information Rate (PIR)

 - Allowsurst traffic at full rate (PIR) in every interval

![20141016_113554-1.jpeg](image/20141016_113554-1.jpeg)

![20141016_113602-1.jpeg](image/20141016_113602-1.jpeg)

policy-map <name>

 class <name>

  shape peak <cir> <b sub c> <b sub e>

Scenario -> 3 classes to be shaped at 64kbps, VOICE, HTTP, default

policy-map WRONG-ANSWER

 class VOICE

  shape average 64000

 class HTTP

  shape average 64000

 class class-default

  shape average 64000

Use nested policy maps

policy-map CHILD

 class VOICE

  priority

 class HTTP

  bandwidth

 class class-default

policy-map PARENT

 class class-default

  shape average 64000

  service-policy CHILD
