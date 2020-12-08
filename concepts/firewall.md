### Firewall:
- [What is a firewall](https://www.youtube.com/watch?v=kDEX1HXybrU&ab_channel=PowerCertAnimatedVideos)
- [firewall and network security](https://www.youtube.com/watch?v=XEqnE_sDzSk&ab_channel=Dr.DanielSoper)
- on network and transport layer
- can implement NAT to hide internal network structure from outside world


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


### Other vulnerabilities
- CSRF (cross site request forgery) - wiki
- [Session hijacking or cookie stealing or man-in-the-middle attack](https://en.wikipedia.org/wiki/Session_hijacking#Prevention)
