# IPv6 EIGRPv6 - Class Notes

**IPv6 EIGRPv6** (28 Aug 2014)Lab:  IPv6 1 and 2

 - Multicast FF02::A

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 eigrp 100</span>

<span style="background-color: #ffaaaa">int lo0</span>

 <span style="background-color: #ffaaaa">ipv6 eigrp 100</span>

The EIGRP process is shutdown by default

<span style="background-color: #ffaaaa">ipv6 router eigrp 100</span>

 <span style="background-color: #ffaaaa">no shutdown</span>

The router-id is still a 32-bit address

 - If no IPv4 addresses are configured on the router, the router-id must be manually configured

<span style="background-color: #ffaaaa">ipv6 router eigrp 100</span>

 <span style="background-color: #ffaaaa">router-id 1.1.1.1</span>

<span style="background-color: #ffaaaa">sh ipv6 eigrp neighbor</span>

<span style="background-color: #ffaaaa">sh ipv6 eigrp topology</span>

<span style="background-color: #ffaaaa">sh ipv6 eigrp interface</span>

**Summarization**

<span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 summary-address eigrp 100 <address/nm></span>

**Authentication**

<span style="background-color: #ffaaaa">int s0/0</span>

 <span style="background-color: #ffaaaa">ipv6 authentication mode eigrp 100 md5</span>

 <span style="background-color: #ffaaaa">ipv6 authentication key-chain eigrp 100 ABC</span>

**Default Routing**

 - There is no dedicated default routing command

- Options

     -> Redistributing a static default router

     -> Using summarization with the ultimate summary ( :: )
