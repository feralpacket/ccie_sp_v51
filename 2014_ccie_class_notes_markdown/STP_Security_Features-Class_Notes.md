# STP Security Features - Class Notes

**STP Security Features**

**BPDU Guard**
 - Checks for any incoming BPDUs
 - Will block any port that receives a BPDU
     -> The port is err-disabled
 - Global configuration
     -> Only affects ports configured for portfast

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree portfast bpduguard default</span>

 - Interface configuration
     -> Portfast not required
     -> “no” command used to disable BPDU Guard on the interface if it is configured globally

SW1(config)# <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> spanning-tree bpduguard enable</span>

**BPDU Filter**
  - Global configuration
     -> Stops sending BPDUs out all interfaces configured for portfast
     -> If BPDU is received
          -> Disables portfast
          -> Starts listening / learning procedure

SW1(config)# <span style="background-color: #ffaaaa">spanning-tree portfast bpdufilter default</span>

 - Interface configuration
     -> Stops sending BPDUs on the interface
     -> Ignores received BPDUs

SW1(config)# <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> spanning-tree bpdufilter enable</span>

If BPDU Guard and BPDU Filter are configured on the same interface
 - Stops sending BPDUs out the interface
 - The interface is err-disabled if a BPDU is received

**Root Guard**
 - Interface configuration
     -> Exams incoming BPDUs
     -> If a superior BPDU is received, the port is err-disabled
     -> Root inconsistant mode
     -> Recommended for edge switches
          -> Can be configured on the root switch

SW1(config)# <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> spanning-tree guard root</span>

**Loop Guard**
 - On a non-designated port (blocking)
     -> If BPDUs are no longer received, the port is err-disabled

**Unidirectional Link Detection (UDLD)**
 - Can be configured on any link between two ports
 - Has hello mechanism to check tx & rx
     -> Sent every 15 seconds
     -> If 3 consecutive hellos are missed, an action is taken
          -> udld mode normal
               -> Syslog message
               -> SNMP trap sent (if configured)
          -> udld mode aggressive
               -> Starts sending hellos every second for 8 seconds
               -> If no hellos are received, the port is err-disabled
 - Global configuration

SW1(config)# <span style="background-color: #ffaaaa">udld mode { aggressive | normal }</span>

 - Interface configuration

SW1(config)#  <span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> udld mode { aggressive | normal }</span>
