# Dynamic Multipoint VPN (DMVPN) - Class Notes

 - I never typed these up.

 - Added 7 April 2024

**Dynamic Multipoint VPN (DMVPN**

 - mGRE

  -> Multipoint GRE ( Generic Router Encapsulation )

 - HNRP

  -> Next Hop Resolution Protocol

 - IPSec

  -> IPSec ( optional )

 - Hub and spoke

<insert picture from page 366>

 - NBMA

  -> Nonbroadcast Multiaccess Network

    -> IP address on edge interface

 - Tunnel address

  -> GRE tunnel IP address

<insert picture from page 367>

**NHRP**

 - It is used to resolve tunnel IP addresses into NBMA IP addresses

 - All spokes will have IP addresses of the hub mapped statically

 - Hub router will have an NHRP cache of "tunnel to NMBA IP address" mappings for spokes

**DMVPN Phase 1**

 - Hub

R1(config)# **int tunnel 0**

**ip add [10.0.0.1](http://10.0.0.1) 255.255.255.0**

**tunnel source [180.1.1.1](http://180.1.1.1)**

**tunnel mode gre multipoint**

**ip nhrp network-id 500**

**ip nhrp map multicast dynamic**

  - Needed to allow multicast traffic

 - Spokes

R2(config)# **int tunnel 0**

**ip add [10.0.0.2](http://10.0.0.2) [255.255.255.0](http://255.255.255.0)**

**tunnel source [180.2.2.2](http://180.2.2.2)**

**tunnel mode gre multipoint**

**ip nhrp network-id 500**

**ip nhrp nhs [10.0.0.1](http://10.0.0.1)**

 - NHS ( Next Hop Server )

**ip nhrp map [10.0.0.1](http://10.0.0.1) [180.1.1.1](http://180.1.1.1)**

**ip nhrp map multicast 180.1.1.1**

 - All routers

  -> Routing

R1(config)# **router eigrp 1**

**network [10.0.0.0](http://10.0.0.0)**

**network [180.1.1.0](http://180.1.1.0)**

R1(config)# **int tunnel 0**

**no ip split-horizon eigrp 1**

**no ip next-hop-self eigrp 1**

**sh dmvpn**

**IP Security**

 -  Main mode

  -> ISAKMP tunnel

 - Quick mode

  -> IPSec tunnel

<insert picture from page 368>

 - Key exchange occurs within the ISAKMP tunnel

**Encryption and Authentication**

 - AH

  -> Authentication Header

 - ESP

  -> Encapsulation Security Payload

    -> Authentication

      -> MD5

      -> SHA1

    -> Encryption

      -> DES

      -> 3DES

      -> AES

**ISAKMP Parameters**

 - Encapsulation

 - Pre-shared key

 - Diffie-Hellman Group

**IPsec Transform Set**

 - Authentication

 - Encryption

 - Modes

  -> Tunnel mode

    -> Entire packet is encrypted

    -> New IP header created

  -> Transport mode

    -> Only IP payload is encrypted

    -> Source and destination IP address are unencrypted

**IPSec Profile**

 - Applied to mGRE tunnel

 - On all routers

R1(config)# **crypto isakmp policy 1**

**encryption aes**

**authentication pre-shared**

**group 14**

R1(config)# **crypto isakmp key CCIE@123 address [10.0.0.0](http://10.0.0.0) [255.255.255.0](http://255.255.255.0)**

 - Use [0.0.0.0](http://0.0.0.0) [0.0.0.0](http://0.0.0.0) for dynamic connections

R1(config# **crypto ipsec transform-set FERALPACKET**

**esp-aes esp-sha-hmac**

**mode tunnel**

 - or **mode transport**

R1(config)# crypto ipsec profile PROFILE1

set transform-set FERALPACKET

R1(config)# **int tunnel 0**

**tunnel protection ipsec profile PROFILE1**

**sh crypto isakmp sa**
