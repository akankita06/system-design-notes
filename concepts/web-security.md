### Authentication and authorization
- Authentication - you are who you say you are
- Authorization - you have permissions to access the resources

&nbsp;

### OAuth (authorization), OpenIDConnect (authentication + authorization)
- [OAuth 2.0](https://www.youtube.com/watch?v=CPbvxxslDTU&ab_channel=InterSystemsLearningServices)
- [OAuth and OpenIDConnect Simplified](https://www.youtube.com/watch?v=996OiexHze0&feature=youtu.be)
- [Oauth2.0](https://www.youtube.com/watch?v=CPbvxxslDTU&ab_channel=InterSystemsLearningServices)
- [OpenID Connect + Oauth in plain English](https://youtu.be/996OiexHze0)


- definition
- oauth
- openID
- openID Connect
- single sign-on

&nbsp;

### Authentication vs authorization:
- authentication - you are who you say you are
- authorization - you have access permissions for the given resource
  - OpenID connect - openID + oauth
  - Single sign on / okta

&nbsp;

### secure web application
- strong pwds
- strong encryption
- secure comm.
- password storage
- password recovery
- 2-factor authentication
- man-in-the middle attacks

client vs server vs implemented in both
- [firewall implementation](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/firewalls/)

&nbsp;

### HTTP, HTTPS, SSH, TLS, SSL
- [explanation](https://www.youtube.com/watch?v=hExRDVZHhig&ab_channel=PowerCertAnimatedVideos)
- [explanation 2](https://www.youtube.com/watch?v=po3zYOe00O4&ab_channel=JackkTutorials)
- [HTTPS vs SSH](https://www.ssl2buy.com/wiki/ssh-vs-ssl-tls)

**HTTP** 
- data in the packets travels without encryption over internet. Anybody can see that data.
- works on application layer (layer 7) of OSI model
- TCP/IP based protocol
- stateless protocol (info not retained across multiple requests)
    - to attain state info → http cookies, server side sessions, variables, url rewriting
- ℹ️HTTP's new version - HTTP/3 might be on UDP

**HTTPS** 
- data in the packets in encrypted. Mechanisms / protocols for encryption - TLS or SSL (TLS is newer version of SSL)

**SSL**
- authenticates client, server, and encrypts the data exchanged

**TLS**
- [handshake breakdown](https://www.youtube.com/watch?v=cuR05y_2Gxc&ab_channel=F5DevCentral)

&nbsp;

### JWT
- [https://www.youtube.com/watch?v=soGRyl9ztjI&ab_channel=JavaBrains](https://www.youtube.com/watch?v=soGRyl9ztjI&ab_channel=JavaBrains)
- [https://www.youtube.com/watch?v=_XbXkVdoG_0&ab_channel=JavaBrains](https://www.youtube.com/watch?v=_XbXkVdoG_0&ab_channel=JavaBrains)
- not for sending sensitive information or security
- it is just to verify who the user is and get their information - not to authenticate the users.

&nbsp;

### 2FA (or multifactor authentication)
- just by password - easy to get the password through mechanisms like phishing, key logger ,etc .
- [what is 2FA](https://www.youtube.com/watch?v=0mvCeNsTa1g&ab_channel=DuoSecurity)
- [Computerphile](https://www.youtube.com/watch?v=ZXFYT-BG2So&ab_channel=Computerphile)
&nbsp;

**Factors:**
- what you know (passwords)
- what you are (biometrics)
- what you have (phone, bank card, yubikey, etc.)
&nbsp;

- multifactor - more than 1 of the above factors
- for authentication of the client / user only 
- for server authentication - HTTPS - certificates 
- [https://www.youtube.com/watch?v=hGRii5f_uSc&ab_channel=TomScott](https://www.youtube.com/watch?v=hGRii5f_uSc&ab_channel=TomScott)
- [how to build it](https://www.gosquared.com/blog/building-two-factor-authentication)
- invalidate the code once it has been used so it cannot be used again
- [how a yubikey works](https://www.youtube.com/watch?v=MffUKM6HnYY&ab_channel=WIREDUK)
- yubikey always sends the data to the exact website where it is supposed to + you don't need to enter any password -   and so prevents phishing

&nbsp;

### VPN
- privacy (location or IP not disclosed)
- secure (data encrypted)
- anonymous  (they don't know who you are, change your location)
- [what is VPN](https://www.youtube.com/watch?v=_wQTRMBAvzg&ab_channel=vpnMentor)
- [VPN explained](https://www.youtube.com/watch?v=xGjGQ24cXAY&ab_channel=AndroidAuthority)

&nbsp;

### DDoS and DNS
- [https://www.youtube.com/watch?v=mpQZVYPuDGU&ab_channel=PowerCertAnimatedVideos](https://www.youtube.com/watch?v=mpQZVYPuDGU&ab_channel=PowerCertAnimatedVideos)
- [https://www.youtube.com/watch?v=ilhGh9CEIwM&ab_channel=PowerCertAnimatedVideos](https://www.youtube.com/watch?v=ilhGh9CEIwM&ab_channel=PowerCertAnimatedVideos)

&nbsp;

### Firewall:
- [What is a firewall](https://www.youtube.com/watch?v=kDEX1HXybrU&ab_channel=PowerCertAnimatedVideos)
- [firewall and network security](https://www.youtube.com/watch?v=XEqnE_sDzSk&ab_channel=Dr.DanielSoper)
- on network and transport layer
- can implement NAT to hide internal network structure from outside world

&nbsp;

#### Types:

- packet filtering gateways - examine source IP, destination IP, protocol for packet
- stateful inspection firewalls
    - considers state or context of the packets
    - "remembers" the network activities of the host
    - to identify hosts that are a threat
    
- application proxy gateways
    - runs pseudo applications which mimic the proper behaviour of the real applications
    - mimics packets travelling between applications inside n/w boundary and application users outuse n/w boundary
    - fitlers out unacceptable protocol commands , or other malformed commands
    
- circuit level gateways
    - firewall that enables one network to become a virtual extension of another network
    - examine whether packets are sent to or received from a target network
    - to implement VPNs
    
- others

&nbsp;

### Other vulnerabilities
- CSRF (cross site request forgery) - wiki
- [Session hijacking or cookie stealing or man-in-the-middle attack](https://en.wikipedia.org/wiki/Session_hijacking#Prevention)

&nbsp;

### Encryption algos
- HAshing algos like SHA1 ,etc. are not exactly  encryption algos since htey cannot be decrypted
- they are more for verification
- as they are 1 directional, you can't get the message back if you give the hash


