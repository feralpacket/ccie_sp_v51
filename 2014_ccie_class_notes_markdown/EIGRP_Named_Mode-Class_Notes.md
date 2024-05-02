# EIGRP Named Mode - Class Notes

 - I never typed these up.

 - Added 8 April 2024

**Named Mode**

 - IPv4

 - IPv6

 - VRF1

R1(config)# **router eigrp 100**

 - IPv4 only

R1(config)# **ipv6 router eigrp 100**

 - IPv6 only

R1(config)# **router eigrp <virtual-instance-name>**

**address-family <AFI> <SAFI> autonomous-system <asn>**

 - **system <asn>**

**network <IP address> [wildcard]**

**af-interface { default | <interface> }**

**summary-address <IP address> <subnet mask>**

**hello-interval <seconds>**

**hold-time <seconds>**

**hold-time <seconds>**

**authentication mode hmac-sha -256 <passprahse>**

**summary-metric <IP address>/<subnet mask> <bandwidth> <delay> <reliability> <load> <mtu>**

**exit**

**topology base**

**distribute-list . . .**

**distance . . .**

**sh run | se router**
