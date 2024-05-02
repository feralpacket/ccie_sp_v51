# QoS Link Efficiency Tools - Class Notes

**QoS Link Efficiency Tools** (4 Sept 2014)

Lab: QoS 1

 - Header Compression

     -> TCP header

     -> UDP RTP header

![20141015_154659-1.jpeg](image/20141015_154659-1.jpeg)

Header compression works better on small packets

 - Such as voice packets

![20141015_154709-1.jpeg](image/20141015_154709-1.jpeg)

![20141015_154714-1.jpeg](image/20141015_154714-1.jpeg)

policy-map <name>

 class <name>

  compress header ip [ rtp | tcp ]
