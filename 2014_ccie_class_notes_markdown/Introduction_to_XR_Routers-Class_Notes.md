# Introduction to XR Routers - Class Notes

**Introduction to XR Routers** (8 Sept 2014) 

What's different

 - IOS kernel is monolithic

     -> All processes share

(config)# int g0/0/0/1

 ipv4 address 10.0.0.1/24

 no shut

 root

 sh config

 commit

sh config failed

clear

sh ipv4 int bri

sh ipv6 int bri

Routing Protocols

 - Static

 - RIPv2

 - EIGRP

 - OSPF

 - IS-IS

 - BGP

**Static Routing**

(config)# router static

 address-family ipv4 unicast

  1.1.1.1/32 172.16.205.129

 root

 sh config

 commit

 end

sh route ipv4

sh route ipv6

(config)# router static

 vrf ABC

**RIPv2**

router rip

 int lo0

 int g0/0/0/0

 timers basic

 int g0/0/0/0

  authentication keychain ABC mode md5

 root / clear / commit

end

sh rip

**EIGRP**

router eigrp { <asn> | <name> }

 address-familty ipv4

  autonomous-system <asn>

     -> in named mode

  int lo0

  in g0/0/0/0

 root

 sh config

 commit

sh eigrp nei

(config)# router eigrp ABC

 address-family ipv4

  int g0/0/0/0

   hello-interval 10

   hold-time 30

 root

 sh config

 commit

(config)# ipv4 access-list ACL1

 permit ipv4 any any

**OSPF**

 

(config)# router ospf P1

 area 0

  int lo0

  int g0/0/0/0
