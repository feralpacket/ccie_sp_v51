# IPv6 Data Plane Filtering - Class Notes

**IPv6 Data Plane Filtering** (28 Aug 2014)Lab:  IPv6 1 and 2

 - Only extended ASLs are supported

 - Only named ACLs are supported

<span style="background-color: #ffaaaa">ipv6 access-list <name></span>

 <span style="background-color: #ffaaaa">permit | deny <protocol> <source> <destination> [log]</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

 <span style="background-color: #ffaaaa">ipv6 traffic-filter <name> in | out</span>
