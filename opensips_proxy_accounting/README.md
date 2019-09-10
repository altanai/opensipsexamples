## Opensips Proxy Usecase 

Version  , opensips -V
```sh
version: opensips 2.2.7 (x86_64/linux)
flags: STATS: On, DISABLE_NAGLE, USE_MCAST, SHM_MMAP, PKG_MALLOC, F_MALLOC, FAST_LOCK-ADAPTIVE_WAIT
ADAPTIVE_WAIT_LOOPS=1024, MAX_RECV_BUFFER_SIZE 262144, MAX_LISTEN 16, MAX_URI_SIZE 1024, BUF_SIZE 65535
poll method support: poll, epoll_lt, epoll_et, sigio_rt, select.
main.c compiled on  with gcc 4.8
```

Main modules 
-signalling
-sl
-tm
- udpproto

## INVITE handling

Receiving INVITE from caller and promptly replying with 100 trying  

```
U <from_ip>:4539 -> <opensips_proxy_ip>:5060
INVITE sip:2222222222@<outbound_server_ip>;transport=udp SIP/2.0.
Via: SIP/2.0/UDP <ofrom_pvtip>:58088;branch=z9hG4bK-d8754z-1cf67b12fff3ba66-1---d8754z-;rport.
Max-Forwards: 70.
Contact: <sip:8887779996@<ofrom_pvtip>:58088;transport=udp>.
To: <sip:2222222222@<outbound_server_ip>>.
From: <sip:8887779996@<opensips_proxy_pubip>>;tag=fd542335.
Call-ID: Y2UzN2VmYTFhMzczNDk3ZGE1NzM0YjI4NzUzYzQyZTA.
CSeq: 1 INVITE.
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, SUBSCRIBE, INFO.
Content-Type: application/sdp.
Supported: replaces.
User-Agent: Bria 3 release 3.5.5 stamp 71243.
Content-Length: 286.
.
v=0.
o=- 1562584100532486 1 IN IP4 <ofrom_pvtip>.
s=Bria 3 release 3.5.5 stamp 71243.
c=IN IP4 <ofrom_pvtip>.
t=0 0.
m=audio 62710 RTP/AVP 9 0 18 98 101.
a=rtpmap:18 G729/8000.
a=fmtp:18 annexb=yes.
a=rtpmap:98 ILBC/8000.
a=rtpmap:101 telephone-event/8000.
a=fmtp:101 0-15.
a=sendrecv.


U <opensips_proxy_ip>:5060 -> <from_ip>:4539
SIP/2.0 100 Giving a try.
Via: SIP/2.0/UDP <ofrom_pvtip>:58088;received=<from_ip>;branch=z9hG4bK-d8754z-1cf67b12fff3ba66-1---d8754z-;rport=4539.
To: <sip:2222222222@<outbound_server_ip>>.
From: <sip:8887779996@<opensips_proxy_pubip>>;tag=fd542335.
Call-ID: Y2UzN2VmYTFhMzczNDk3ZGE1NzM0YjI4NzUzYzQyZTA.
CSeq: 1 INVITE.
Server: OpenSIPS (2.2.7 (x86_64/linux)).
Content-Length: 0.
.
```

```sh
 [26498] DBG:core:parse_msg: SIP Request:
 [26498] DBG:core:parse_msg:  method:  <INVITE>
 [26498] DBG:core:parse_msg:  uri:     <sip:2222222222@<outbound_server_ip>;transport=udp>
 [26498] DBG:core:parse_msg:  version: <SIP/2.0>
 [26498] DBG:core:parse_headers: flags=2
 [26498] DBG:core:parse_via_param: found param type 232, <branch> = <z9hG4bK-d8754z-1cf67b12fff3ba66-1---d8754z->; state=6
 [26498] DBG:core:parse_via_param: found param type 235, <rport> = <n/a>; state=17
 [26498] DBG:core:parse_via: end of header reached, state=5
 [26498] DBG:core:parse_headers: via found, flags=2
 [26498] DBG:core:parse_headers: this is the first via
 [26498] DBG:core:receive_msg: After parse_msg...
 [26498] DBG:core:receive_msg: preparing to run routing scripts...
 SIP request methods - INVITE 
from udp sip:<from_ip>:4539 
 [26498] DBG:core:parse_headers: flags=8
 [26498] DBG:core:_parse_to: end of header reached, state=10
 [26498] DBG:core:_parse_to: display={}, ruri={sip:2222222222@<outbound_server_ip>}
 [26498] DBG:core:get_hdr_field: <To> [32]; uri=[sip:2222222222@<outbound_server_ip>] 
 [26498] DBG:core:get_hdr_field: to body [<sip:2222222222@<outbound_server_ip>>
for sip:2222222222@<outbound_server_ip> 
 [26498] DBG:maxfwd:is_maxfwd_present: value = 70 
 [26498] DBG:uri:has_totag: no totag
 [26498] DBG:core:parse_headers: flags=78
 [26498] DBG:core:get_hdr_field: cseq <CSeq>: <1> <INVITE>
 [26498] DBG:tm:t_lookup_request: start searching: hash=60272, isACK=0
 [26498] DBG:tm:matching_3261: RFC3261 transaction matching failed
 [26498] DBG:tm:t_lookup_request: no transaction found
```

and then forward the INVITE to specific server IP as called sipuri was 2222222222@<outbound_server_ip>

```
U <opensips_proxy_ip>:5060 -> <outbound_server_ip>:5060
INVITE sip:2222222222@<outbound_server_ip>;transport=udp SIP/2.0.
Record-Route: <sip:<opensips_proxy_ip>;lr>.
Via: SIP/2.0/UDP <opensips_proxy_ip>:5060;branch=z9hG4bK07be.cf297d74.0.
Via: SIP/2.0/UDP <ofrom_pvtip>:58088;received=<from_ip>;branch=z9hG4bK-d8754z-1cf67b12fff3ba66-1---d8754z-;rport=4539.
Max-Forwards: 69.
Contact: <sip:8887779996@<ofrom_pvtip>:58088;transport=udp>.
To: <sip:2222222222@<outbound_server_ip>>.
From: <sip:8887779996@<opensips_proxy_pubip>>;tag=fd542335.
Call-ID: Y2UzN2VmYTFhMzczNDk3ZGE1NzM0YjI4NzUzYzQyZTA.
CSeq: 1 INVITE.
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, SUBSCRIBE, INFO.
Content-Type: application/sdp.
Supported: replaces.
User-Agent: Bria 3 release 3.5.5 stamp 71243.
Content-Length: 286.
P-hint: outbound.
.
v=0.
o=- 1562584100532486 1 IN IP4 <ofrom_pvtip>.
s=Bria 3 release 3.5.5 stamp 71243.
c=IN IP4 <ofrom_pvtip>.
t=0 0.
m=audio 62710 RTP/AVP 9 0 18 98 101.
a=rtpmap:18 G729/8000.
a=fmtp:18 annexb=yes.
a=rtpmap:98 ILBC/8000.
a=rtpmap:101 telephone-event/8000.
a=fmtp:101 0-15.
a=sendrecv.
```

```sh
 For non-REGISTER requests check if the source or destination is local 
  [26498] DBG:core:parse_to_param: tag=fd542335
 [26498] DBG:core:_parse_to: end of header reached, state=29
 [26498] DBG:core:_parse_to: display={}, ruri={sip:8887779996@<opensips_proxy_pubip>}
 caller URI sip:8887779996@<opensips_proxy_pubip> 
 callee URI sip:2222222222@<outbound_server_ip>;transport=udp 
 [26498] WARNING:core:comp_scriptvar: invalid EQUAL operation: left is VARIABLE_ELEMENT/STRING_VAL, right is MYSELF/NO_VAL
 [26498] WARNING:core:do_action: error in expression at /etc/opensips/opensips.cfg:166
 [26498] DBG:core:grep_sock_info: checking if host==us: 13==13 &&  [<outbound_server_ip>] == [<opensips_proxy_ip>]
 [26498] DBG:core:grep_sock_info: checking if port 5060 matches port 5060
 [26498] DBG:core:check_self: host != me
 Neither from URI nor called URI is local/myself to opensips Server 
 [26498] DBG:core:parse_headers: flags=200
 [26498] DBG:core:get_hdr_field: content_length=286
 [26498] DBG:core:get_hdr_field: found end of header
 [26498] DBG:rr:find_first_route: No Route headers found
 [26498] DBG:rr:loose_route: There is no Route HF
 [26498] DBG:core:parse_headers: flags=78
 [26498] DBG:core:grep_sock_info: checking if host==us: 13==13 &&  [<outbound_server_ip>] == [<opensips_proxy_ip>]
 [26498] DBG:core:grep_sock_info: checking if port 5060 matches port 5060
 [26498] DBG:core:check_self: host != me
 Request URI is different , possibly outbound , append header 
  [26498] DBG:core:parse_headers: flags=ffffffffffffffff
 [26498] DBG:tm:t_newtran: transaction on entrance=(nil)
 [26498] DBG:core:parse_headers: flags=ffffffffffffffff
 [26498] DBG:core:parse_headers: flags=78
 [26498] DBG:tm:t_lookup_request: start searching: hash=60272, isACK=0
 [26498] DBG:tm:matching_3261: RFC3261 transaction matching failed
 [26498] DBG:tm:t_lookup_request: no transaction found
 [26498] DBG:core:parse_headers: flags=ffffffffffffffff
 [26498] DBG:core:_shm_resize: resize(0) called
 [26498] DBG:tm:_reply_light: reply sent out. buf=0x7f0197b7e020: SIP/2.0 1..., shmem=0x7f0195df4330: SIP/2.0 1
 [26498] DBG:tm:_reply_light: finished
Y2UzN2VmYTFhMzczNDk3ZGE1NzM0YjI4NzUzYzQyZTA new branch at sip:2222222222@<outbound_server_ip>;transport=udp
```
## Accounting 

The ACC module is used to account transaction information to different backends such as syslog, SQL, AAA.
do_accounting(type, flags, table) script function marks a transaction for accounting  and accounting will be done later when the transaction or dialog will be completed.

The fixed minimal accounting information is:

- Request Method name
- From header TAG parameter
- To header TAG parameter
- Call-Id
- 3-digit Status code from final reply
- Reason phrase from final reply
- Timestamp when transaction was completed

session/dialog accounting can automatically correlate INVITEs with BYEs for generating proper CDRs, for example for billing purposes.

If a UA fails in the middle of a conversation, a proxy will never find out about it. In general, a better practice is to account from an end-device (such as PSTN gateway), which best knows about call status (including media status and PSTN status in case of the gateway).

### do_accounting params 

**type** 
- log - syslog accounting;
- db - database accounting;
- aaa - aaa specific accounting;
- evi - Event Interface accounting;

**flags** 
- cdr - enables dialog-level accounting. OpenSIPS will internally detect dialog termination (generation/receipt of a BYE request), and store the CDR as soon as the BYE request is replied to. By enabling the "cdr" flag, the following additional fields will be populated: duration, ms_duration, setuptime, created. (requires dialog module support)
- missed - log missed calls; take care that this flag will be deactivated after the first missed call; you will have to reactivate it in the failure_route if you want to account each destination that did not respond to the call;
- failed - flag which indicates if the transaction should also be accounted in case of failure (status>=300);
- table - table where to do the accounting; it replaces old table_avp parameter;

Example : enable cdr and missed calls accounting in the database and to syslog; 
db accounting shall be done in "my_acc" table in INVITE
do normal accounting via aaa on BYE
```
if (!has_totag()) {
	if (is_method("INVITE")) {
		do_accounting("db|log", "cdr|missed", "my_acc");
	}
}
...
if (is_method("BYE")) {
	do_accounting("aaa");
}
```
