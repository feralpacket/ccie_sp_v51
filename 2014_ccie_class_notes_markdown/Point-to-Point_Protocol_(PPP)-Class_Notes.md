# Point-to-Point Protocol (PPP) - Class Notes

**Point-to-Point Protocol (PPP)**

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

On the exam, they will always ask for authentication

**Authentication**

 - Password Authentication Protocol (PAP)

     -> Plain text

     -> One way or two way

     -> One time authentication

 - Challenge Handshake Authentication Protocol (CHAP)

     -> Encrypted

     -> Periodic authentication

![20141222_131510-1.jpeg](image/20141222_131510-1.jpeg)

**PPP Authentication**

 - Server / Client authentication

     -> Server (authenticator) authenticates by comparing received credentials with the security database

     -> Client (authenticated) sends credentials to be authenticated

![20141222_131514-1.jpeg](image/20141222_131514-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">username R2 password CISCO</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp authentication pap</span>

R2(config)# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp pap sent-username R2 password CISCO</span>

**CHAP**

R1(config)# <span style="background-color: #ffaaaa">username R2 password CISCO</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation</span>

<span style="background-color: #ffaaaa"> ppp authentication chap</span>

R2(config)# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation</span>

<span style="background-color: #ffaaaa"> ppp chap hostname R2</span>

     -> sent as username

 <span style="background-color: #ffaaaa">ppp chap password CISCO</span>

 - If the ppp chap hostname is not configured, the router hostname will be sent as the username

**Mutual Authentication**

R1(config)# <span style="background-color: #ffaaaa">username R1 password CISCO</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp authentication chap</span>

R2(config)# <span style="background-color: #ffaaaa">username R2 password CISCO</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp authentication chap</span>

**Multi Link Point-to-Point Protocol (MLPPP)**

![20141222_131523-1.jpeg](image/20141222_131523-1.jpeg)

R1(config)# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp multilink</span>

<span style="background-color: #ffaaaa"> ppp multilink group 1</span>

<span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> ppp multilink</span>

<span style="background-color: #ffaaaa"> ppp multilink group 1</span>

<span style="background-color: #ffaaaa">int multilink1</span>

<span style="background-color: #ffaaaa"> ip address 10.0.0.1 255.255.255.0</span>

<span style="background-color: #ffaaaa"> ppp multilink</span>

<span style="background-color: #ffaaaa"> ppp multilink group 1</span>

In IOS 12.4, the multilink interface number has to match the group number

**Load-balancing**

 - Frame level

 - Divided equally

 - Should be used on high end routers

<span style="background-color: #ffaaaa">show ppp multilink</span>
<span style="background-color: #ffaaaa">show int multilink1</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
**Verification**

<span style="background-color: #ffaaaa">sh ppp int <int></span>
<span style="background-color: #ffaaaa">sh ppp all</span>
<span style="background-color: #ffaaaa">sh int <int></span>
<span style="background-color: #ffaaaa">ping <ip address></span>
