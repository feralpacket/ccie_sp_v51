# Metro-Ethernet 3400 Switches - Class Notes

**ME 3400 Switches** (19 Sept 2014)- Metro Ethernet

 - All ports are shutdown by default

     -> Except uplink ports

 - Ports

     -> Downstream

          -> Going towards the client

          -> User Network Interface (UNI)

     -> Upstream

          -> Going towards  SP core

          -> Network Node Interface (NNI)

     -> Downstream ports are isolated by default

          -> Can't communicate with each other

     -> Downstream ports can communicate with upstream ports by default

UNI <--> NNI

UNI <-/-> UNI

On UNI ports, processing of these protocols is disabled

 - STP

 - CDP

 - LLDP

If a client wants STP / CDP / LLDP / etherchannel to be activated on downstream ports, the ports must be changed

 - Enhanced Network Interface (ENI)

No DTP

 - Trunk and access are the only options

Only dot1q is supported

No VTP support

Default interface configuration is "static access"

To change the port type

<span style="background-color: #ffaaaa">int fa0/1</span>

<span style="background-color: #ffaaaa"> port-type { uni | eni | nni }</span>

<span style="background-color: #ffaaaa">sh port-type</span>

All VLANs on UNI ports are "uni-isolated" type

 - To enable communication between UNI ports, change the VLAN type to "uni-community"

<span style="background-color: #ffaaaa">int range fa0/1 - 5</span>

<span style="background-color: #ffaaaa"> uni-vlan community</span>
