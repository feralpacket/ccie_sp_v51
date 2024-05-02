# OSPF - Class Notes - Part 2

`classnotes`

**Filtering by Distance**

router ospf 1

 distance 255 <source> <wildcard> <acl>

![20140930_113554-1.jpeg](image/20140930_113554-1.jpeg)

 **Filtering External Routes**

 Scenario -> R3 is redistributing the following routes from EIGRP

      - 10.0.0.0 /24

      - 10.0.1.0 /24

      - 10.0.2.0 /24

      - 10.0.3.0 /24

           -> Filer 10.0.0.0 and 10.0.1.0 when redistributing 

 R3(config)# router ospf 1

 redistribute eigrp 1 subnets

 summary-address 10.0.0.0 255.255.254.0 not-advertise

![20140930_113611-1.jpeg](image/20140930_113611-1.jpeg)

 **Virtual Links**

 - Used to connect discontiguous areas and broken area 0

 - A virtual link is a point-to-point link in area 0 

 ABR1 - ABR between backbone and transit area

 ABR2 - ABR between transit area and discontiguous area 

 ABR1 | ABR2 

router ospf 1

 area <transit area> virtual-link <router-id of other ABR>

 R1(config)# router ospf 1

 area 1 virtual-link 2.2.2.2

 R2(config)# rotuer ospf 1

 area 1 virtual-link 1.1.1.1

sh ip ospf interface

**Stub Areas**

 - Stub area

 - Totally stubby area

 - Not-so-stubby-area (NSSA)

 - Totally NSSA 

![20140930_115509-1.jpeg](image/20140930_115509-1.jpeg)

 Stub area

 - Does not allow type 5 routes into the area

 - All external routes (type 5) are filtered by ABR and replaced with one type 3 default route 

 Totally stubby area

 - Do not allow type 3 or type 5 routes into the area

 - Replaced with a type 3 default route 

![20140930_115520-1.jpeg](image/20140930_115520-1.jpeg)

 **Stub Area Configuration**

 - On all routers in area 1, including the ABR 

router ospf 1

 area 1 stub

sh ip ospf

 - lists areas and which ones are stubs 

**Totally Subby Area Configuration**

 - On all routers in the area except the ABR 

router ospf 1

 area 1 stub

 - On ABR 

router ospf 1

 area 1 stub no-summary

![20140930_120737-1.jpeg](image/20140930_120737-1.jpeg)

 NSSA

 - Stub area with redistribution possible

 - It's a stub area with an ASBR

 - External routes created inside the NSSA are type 7

      -> Because type 5 LSAs are not allowed in stub areas

 - When type 7 LSAs reach an ABR between the NSSA and area 0, the LSAs are translated to type 5 LSAs by a "translator ABR"

 - If multiple ABRs are present, there will be a translator election and the one with the highest router-id wins 

![20140930_120750-1.jpeg](image/20140930_120750-1.jpeg)

 If area 2 is filtering R6's address from area 0, then after translation from type 7 to type 5, area 0 routes would not be able to reach the external routers from the NSSA 

 type 5

 network 50.0.0.0

 originator-id 4.4.4.4

 forwarding address 6.6.6.6 -> 0.0.0.0

      -> forwarding address would need to be suppressed when translated from type 7 to type 5 

**NSSA Configuration**

 - On all routers in the area, including the ABR 

router ospf 1

 area 2 nssa

**Totally NSSA Configuration**

 - On all routers in the area, except the ABR 

router ospf 1

 area 2 nssa

 - On the ABR 

router ospf 1

 area 2 nssa no-summary

 To suppress forwarding address on translator ABR 

router ospf 1

 area 2 nssa translate type 7 supress-fa

![20140930_122555-1.jpeg](image/20140930_122555-1.jpeg)

no-redistribution

 - When no-redistribution is done, it will normally create type 5 and type 7 LSAs

 - By using the keyword "no-redistribution", the ABR is instructed not to generate type 7 LSAs

R4(config)# router ospf 1

 area 2 nssa no-redistribution

     -> Stub area - injects a default router type 3 LSA

     -> Totally stubby area - injects a default route type 3 LSA

     -> NSSA - no default route is injected

     -> Totally NSSA - default router type 3 LSA

To inject a default router in a NSSA

router ospf 1

 area 2 nssa default-information-originate

     -> This will create a type 7 default route be injected into the NSSA
