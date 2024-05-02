# STP Fast Convergence Features - Class Notes

**STP Fast Convergence Features**

**1.  Portfast**
 - Bypasses the listening and learning states on edge ports
 - Used for end devices
 - Global configuration
     -> Applied to all non-trunking ports

SW1(config)#  <span style="background-color: #ffaaaa">spanning-tree portfast default</span>

 - Interface configuration

SW1(config)# <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> spanning-tree portfast</span>

 - On trunk port
     -> Should be used on trunk ports connected to
          -> Routers
          -> Servers

SW1(config)# <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> spanning-tree portfast trunk</span>

**2.  Uplinkfast**
 - Bypasses listening and learning states
     -> Enables blocked port immediately
 - Used for direct link failurees
 - Sends dummy packets for all connected end devices
     -> Destined for STP multicast address
 - Max-update-rate
     -> Default is 150 packets per second
 - Global configuration

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree uplinkfast [max-update-rate <pps>]</span>

**3.  Backbonefast**
 - To avoid max-age-timer incase of indirect link failure
 - Sends Root Link Query (RLQ) out blocking port
     -> Sent as soon as an inferior BPDU is received
 - When RLQ acknowledgement is received, listening / learning procedure starts
 - Global configuration

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree backbonefast</span>

<span style="background-color: #ffaaaa">show spanning-tree sum</span>
 - Will list all of the STP features that are enabled

<span style="background-color: #ffaaaa">show spanning-tree int <int> detail</span>
 - Detailed information
 - Lists the number of BPDUs that are sent and received
