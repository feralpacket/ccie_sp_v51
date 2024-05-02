# QoS Shaping and Policing - Class Notes

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

<span style="background-color: #ffaaaa">policy-map <name></span>
<span style="background-color: #ffaaaa"> class <name></span>
<span style="background-color: #ffaaaa">  shape average <cir> [ b sub c ] [ b sub e ]</span>

e.g. - Shape all http traffic at the rate of 128kbps
     -> Use the burst size of 16000 bit / t sub c

<span style="background-color: #ffaaaa">class-map HTTP</span>
<span style="background-color: #ffaaaa"> match protocol http</span>
<span style="background-color: #ffaaaa">policy-map SHAPE</span>
<span style="background-color: #ffaaaa"> class HTTP</span>
<span style="background-color: #ffaaaa">  shape average 12800 1600</span>
<span style="background-color: #ffaaaa">int s0/0</span>
<span style="background-color: #ffaaaa"> service-policy output SHAPE</span>

**Dual Rate Shaping**
 - CIR
 - Peak Information Rate (PIR)
 - Allowsurst traffic at full rate (PIR) in every interval

![20141016_113554-1.jpeg](image/20141016_113554-1.jpeg)

![20141016_113602-1.jpeg](image/20141016_113602-1.jpeg)

<span style="background-color: #ffaaaa">policy-map <name></span>
<span style="background-color: #ffaaaa"> class <name></span>
<span style="background-color: #ffaaaa">  shape peak <cir> <b sub c> <b sub e></span>

Scenario -> 3 classes to be shaped at 64kbps, VOICE, HTTP, default

<span style="background-color: #ffaaaa">policy-map WRONG-ANSWER</span>
<span style="background-color: #ffaaaa"> class VOICE</span>
<span style="background-color: #ffaaaa">  shape average 64000</span>
<span style="background-color: #ffaaaa"> class HTTP</span>
<span style="background-color: #ffaaaa">  shape average 64000</span>
<span style="background-color: #ffaaaa"> class class-default</span>
<span style="background-color: #ffaaaa">  shape average 64000</span>

Use nested policy maps

<span style="background-color: #ffaaaa">policy-map CHILD</span>
<span style="background-color: #ffaaaa"> class VOICE</span>
<span style="background-color: #ffaaaa">  priority</span>
<span style="background-color: #ffaaaa"> class HTTP</span>
<span style="background-color: #ffaaaa">  bandwidth</span>
<span style="background-color: #ffaaaa"> class class-default</span>
<span style="background-color: #ffaaaa">policy-map PARENT</span>
<span style="background-color: #ffaaaa"> class class-default</span>
<span style="background-color: #ffaaaa">  shape average 64000</span>
<span style="background-color: #ffaaaa">  service-policy CHILD</span>

**Policing**
 - Punishing the traffic to enforce the service level agreement
     -> Traffic rate
          -> To control the incoming traffic rate
               -> Called metering
          -> 1 token = 1 byte
          -> b sub c = bucket size
               -> The burst size

![20141016_113611-1.jpeg](image/20141016_113611-1.jpeg)

The bucket is filled when the burst of packets arrive

The number of tokens filled in the bucket are calculated by a formula

![20141016_113618-1.jpeg](image/20141016_113618-1.jpeg)

The faster traffic is sent, the less traffic can be sent

The slower traffic is sent, the more traffic can be sent

![20141016_113629-1.jpeg](image/20141016_113629-1.jpeg)

![20141016_113637-1.jpeg](image/20141016_113637-1.jpeg)

Conforming rate
 - within the contract

Exceeding rate
 - beyond the contract

Violating rate
 - way beyond the contract

When a single bucket is used, it is called single rate, two bucket policing
 - Single rate = CIR
 - Colors = actions
     -> Conform
     -> Exceed

**Actions**
 - drop
 - transmit
 - set-dscp-transmit <dscp>
 - set-precedence-transmit <precedence>
 - set-frde-transmit
     -> Frame-Relay Discard Eligible (FRDE)

<span style="background-color: #ffaaaa">policy-map <name></span>
<span style="background-color: #ffaaaa"> class <name></span>
<span style="background-color: #ffaaaa">  police <cir> <b sub c> conform-action <action> exceed-action <action></span>

When two buckets are used, it is called single rate, three colors
 - Single rate = CIR
 - Colors = actions
     -> Conform
     -> Exceed
     -> Violate

![20141016_113648-1.jpeg](image/20141016_113648-1.jpeg)

![20141016_113656-1.jpeg](image/20141016_113656-1.jpeg)

18000 bytes
 - First 8000 bytes use b sub c tokens
     -> Conforming
 - Second 8000 bytes use b sub e tokens
     -> Exceeding
 - last 2000 bytes are dropped
     -> Violating

<span style="background-color: #ffaaaa">policy-map <name></span>
<span style="background-color: #ffaaaa"> class <name></span>
<span style="background-color: #ffaaaa">  police 64000 8000 8000 confirm-action transmit exceed-action set-dscp-transmit 0 violate-action drop</span>
     -> 64000 - bps
     -> 8000 - bytes
     -> 8000 - bytes

When two buckets and two rates are used, it is called dual rate, three color policing

Ratio = CIR * PIR

![20141016_113705-1.jpeg](image/20141016_113705-1.jpeg)

![20141016_113719-1.jpeg](image/20141016_113719-1.jpeg)

b sub c and b sub e buckets are filled
 - b sub c tokens are used to send the first 8000 bytes
 - For every b sub c token used, one token will be removed from the b sub e bucket
 - b sub e tokens are used to send the next 8000 bytes
 - The last 2000 bytes are violating

<span style="background-color: #ffaaaa">policy-map <name></span>
<span style="background-color: #ffaaaa"> class <name></span>
<span style="background-color: #ffaaaa">  police cir <value> bc <value> pir <value> be <value> conform-action transmit exceed-action set-dscp-transmit AF11 violate-action drop</span>
