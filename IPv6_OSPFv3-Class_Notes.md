# IPv6 OSPFv3 - Class Notes

**IPV6 OSPFv3** (28 Aug 2014)Lab:  IPv6 1 and 2

 - Multicast FF02::5 - all routers

 - Multicast FF02::6 - all DR routers

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 ospf 1 area 0</span>

<span style="background-color: #ffaaaa">ipv6 router ospf 1</span>

 <span style="background-color: #ffaaaa">ospf router-id 1.1.1.1</span>

**Summarization**

 - Inside routing mode

<span style="background-color: #ffaaaa">ipv6 router ospf 1</span>

 <span style="background-color: #ffaaaa">area 0 range <summary address/nm></span>

**Default Routing**

 - Inside routing mode

<span style="background-color: #ffaaaa">ipv6 router ospf 1</span>

 <span style="background-color: #ffaaaa">default-information originate [ always | route-map <name> ]</span>

**Filtering**

 - Only distribute-list is supported

 - Inter-area filtering is not supported

 - Filtering is done on the local router between the database and the routing table

<span style="background-color: #ffaaaa">ipv6 router ospf</span>

 <span style="background-color: #ffaaaa">distribute-list prefix <name> in</span>

**Authentication**

 - OSPFv3 supports IPsec authentication

     -> Authentication

          -> MD5

          -> SHA1 (Secure Hash Algorithm)

     -> Encryption

          -> DES

          ->3DES

          -> AES

<span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 ospf authentication ipsec spi 500 { md5 | sha1 } <password></span>

     -> For MD5, password is 32 characters long

     -> For SHA1, password is 40 characters long

 <span style="background-color: #ffaaaa">ipv6 ospf encryption ipsec spi 500 { des | 3des | aes }</span>

**New LSAs and Changes**

 - LSA 8

     -> Intra Area Prefixes

     -> All connected networks of all routers within the area

 - LSA 1

     -> Only lists the neighbors / routers in the area

 - LSA 9

     -> Link LSA

     -> It consists of the link-local address

     -> Scope is link-local
