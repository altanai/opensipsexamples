# Speed Dial 

uses prestore records of pairs of short numbers (2 digits) and SIP addresses into a table . As user dial the 2 digit number opensip connects the call to the SIP address associated with that 2 digit speeddial number.

opensips version 2.2.7

Main module for usecase 
-speeddial
-dbtext

Using dbtext , create a table for speed dial mappings 

id(int,auto) username(str) domain(str) sd_username(str) sd_domain(str) new_uri(str) fname(str) lname(str) description(str)
1:11:10.130.74.149:1111111111:10.130.74.149:'user1':'domain1':'sip:user1@domain1':'u1':'d1':'some description 1'
2:22:10.130.74.80:2222222222:0.130.74.80:'user2':'domain2':'sip:user2@domain2':'u2':'d2':'some description 2'

## debug 

**Issue 1** : too many hops 
```
SIP/2.0 483 Too Many Hops
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.553c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.453c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.353c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.253c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.153c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.053c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.f43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.e43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.d43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.c43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.b43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.a43c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.943c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.843c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.743c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.643c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.543c8513.0
Via: SIP/2.0/UDP y.y.y.y:5060;received=x.x.x.x;branch=z9hG4bK2d1c.033c8513.0
Via: SIP/2.0/UDP 192.168.1.120:2214;received=106.51.78.22;branch=z9hG4bK-d8754z-67e1844e3d46cc01-1---d8754z-;rport=8525
To: <sip:22@x.x.x.x>;tag=8aaf815a529abe00c20a394b34fbf75a.9382
From: <sip:8887779996@x.x.x.x>;tag=a650514f
Call-ID: YmNlY2MwMjA5YjVlNWJjYTk0YTZkZGUxYWE0ZDJlNmQ
CSeq: 1 INVITE
Server: OpenSIPS (2.2.7 (x86_64/linux))
Content-Length: 0
```
**Solution** : check the via and contact headers if they contain appropriate address and modify the listen address accodingly
```
opensips -f /etc/opensips/opensips.cfg -E 
Listening on 
             udp: ip_addr [ip_addr]:5060
Aliases: 
             udp: publicip_addr:5060
```

Ref : 
Speed Dial module for 2.2 - https://opensips.org/html/docs/modules/2.2.x/speeddial.html
Speed_dial db table for 2.4 - https://www.opensips.org/Documentation/Install-DBSchema-2-4#AEN9116
