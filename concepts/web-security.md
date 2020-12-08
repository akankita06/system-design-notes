### Authentication and authorization
- Authentication - you are who you say you are
- Authorization - you have permissions to access the resources

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

### Authentication vs authorization:
- authentication - you are who you say you are
- authorization - you have access permissions for the given resource
  - OpenID connect - openID + oauth
  - Single sign on / okta

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

