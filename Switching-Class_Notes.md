# Switching - Class Notes

**Layer 2** (18 Aug 2014)

Lab:  STP

![ba572452-3433-4bea-b66c-4cd3adfff6b1.png](image/ba572452-3433-4bea-b66c-4cd3adfff6b1.png)

ACL --> Frame allowed / denied

QoS --> Priority frame marking

CAM --> next-hop information -> exit interface 

   |

   |--> All three operations are performed at the same time using dedicated co-processors (ASICs)

Terney Content Addressable Memory

 - 3 values (VMR)

     - Value

     - Mask

     - Result

If destination is starting with 10.x.x.x, then deny

 - 10.x.x.x               --> Value

 - 0.255.255.255     --> Mask

 - Deny                   --> Result

show mac address-table

 --> Vlan

 --> MAC address

 --> Type (how learned)

 --> Ports (exit interface)

**Layer 3**

![48c44882-113b-45c3-8f66-83cb854b544f.png](image/48c44882-113b-45c3-8f66-83cb854b544f.png)

FIB --> Forwarding Information Base

PR --> Packet Rewrite

CEF --> Cisco Express Forwarding

PR

 - rewrite TTL (IP headers)

 - MAC source / destination

 - IP checksum

 - ethernet CRC

show ip cef

 -> Prefix

 -> Next-hop

     -> no route

     -> receive

     -> drop

 -> Interface

Switching is all about interfaces

Interface Modes

 -> Access - connected to end devices

 -> Trunk - connected to other switches

 -> Tunnel - Q in Q tunnel

<span style="background-color: #ffaaaa">int fa0/1</span>

 <span style="background-color: #ffaaaa">switchport mode { access | trunk }</span>

Tunnel mode is configured in access mode

<span style="background-color: #ffaaaa">int range fa0/1 - 5</span>

<span style="background-color: #ffaaaa">int range fa0/1, fa0/5 - 8 , fa0/15</span>

<span style="background-color: #ffaaaa">define interface ABC fa0/1, fa0/5 - 8</span>

i<span style="background-color: #ffaaaa">nt range macro ABC</span>

**Trunking**

 -> ISL - Inter-Switch Link

 -> 802.1Q - IEEE standard

ISL

 -> not in the R&S v5 lab

 -> Cisco proprietary

 -> 30 bytles additionally added to the frame

 --------------------

 | 26 | frame | 4 |

 ---------------------

 -> only 15 bits used to identify the VLAN

 -> no untagged VLAN

802.1Q

 -> 4 bytes added to the frame

   6      6      4      up to 1500 bytes  4

-------------------------------------------------

| DA | SA | .1q | type | data.........| FCS |

-------------------------------------------------

                   ^

 -> 12 bits

     -> 2^12 VLANs --> 4096

          0 to 4095

               -> Vlan 0 (hidden system VLAN)

               -> Vlan 4095 (hidden system VLAN)

               -> Vlans 1 - 4094 usable

Normal frame

 -> 1518 bytes

802.1Q frame

 -> 1522 bytes

Vlan 1

 -> Management VLAN

 -> Default native VLAN

     -> untagged VLAN

 -> used for:

     -> CDP packets

     -> VTP packets

     -> STP packets

 -> Shutting down VLAN 1 only affects data traffic

 -> All switching protocols use VLAN 1

VLANs 1002 - 1005

 -> Tolken Ring VLANs

ISL

 -> VLANs 1 - 1001

Reduced MAC Address feature

 - One VLAN -> priority number + Base MAC address

 - Configured with:

     -> SW1(config)# <span style="background-color: #ffaaaa">spanning-tree extended system-id</span>

     -> Default configuration

     -> Required to create more than 1001 VLANs

SW1

 - priority number: 32768

 - Base MAC address:  0001.0002.0003

 - VLAN 10:  32768.0001.0002.0003

 - VLAN 11:  32769.0001.0002.0003

Spanning Tree Protocol (801.2D)

 -> used to prevent loops while maintaining redundancy
