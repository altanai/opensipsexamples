# Opensips acting as REGISTRAR

opensips version 2.2.7 

Main modules used in Registraer usecases 
-uri 
-registrar
-usrloc

A register request goes through record_route , save location processes .

## REGISTER handling

provided domain altanai.com mapping to the opensips as registrar
```
U x.x.x.x:5060 -> x.x.x.x:5060
REGISTER sip:altanai.com SIP/2.0.
Via: SIP/2.0/UDP x.x.x.x:5060;branch=z9hG4bKd47a.b6cf701.1.
Via: SIP/2.0/UDP x.x.x.x:51078;received=x.x.x.x;branch=z9hG4bK-d8754z-9b34b37884085f16-1---d8754z-;rport=24258.
Max-Forwards: 69.
Contact: <sip:8887779996@x.x.x.x:51078;rinstance=36c128012aea35fe;transport=udp>.
To: <sip:8887779996@altanai.com>.
From: <sip:8887779996@altanai.com>;tag=ef11122e.
Call-ID: Y2FlNWFmZDc0MzJhMjdhYTFhZjIxN2IwNjdhNTA3NTc.
CSeq: 1 REGISTER.
Expires: 3600.
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, SUBSCRIBE, INFO.
User-Agent: Bria 3 release 3.5.5 stamp 71243.
Content-Length: 0. 
```
Incoming REGISTER request handled as 
```
 SIP request methods - REGISTER 
from udp sip:x.x.x.x:24258 
 [22867] DBG:core:parse_headers: flags=8
 [22867] DBG:core:_parse_to: end of header reached, state=10
 [22867] DBG:core:_parse_to: display={}, ruri={sip:8887779996@altanai.com}
 [22867] DBG:core:get_hdr_field: <To> [30]; uri=[sip:8887779996@altanai.com] 
 [22867] DBG:core:get_hdr_field: to body [<sip:8887779996@altanai.com>
for sip:8887779996@altanai.com 
 [22867] DBG:maxfwd:is_maxfwd_present: value = 70 
 [22867] DBG:uri:has_totag: no totag
 [22867] DBG:core:parse_headers: flags=78
 [22867] DBG:core:get_hdr_field: cseq <CSeq>: <1> <REGISTER>
 [22867] DBG:tm:t_lookup_request: start searching: hash=42829, isACK=0
 [22867] DBG:tm:matching_3261: RFC3261 transaction matched, tid=-d8754z-9b34b37884085f16-1---d8754z-
 [22867] DBG:tm:t_lookup_request: REF_UNSAFE:[0x7ff2f8664c78] after is 1
 [22867] DBG:tm:t_lookup_request: transaction found (T=0x7ff2f8664c78)
 [22867] DBG:tm:t_retransmit_reply: nothing to retransmit
 [22867] DBG:tm:t_check_trans: UNREF_UNSAFE: [0x7ff2f8664c78] after is 0
 [22867] DBG:core:destroy_avp_list: destroying list (nil)
 [22867] DBG:core:receive_msg: cleaning up
 [22871] DBG:tm:timer_routine: timer routine:0,tl=0x7ff2f8664ec8 next=(nil), timeout=114
 [22871] DBG:tm:final_response_handler: Cancel sent out, sending 408 (0x7ff2f8664c78)
 [22871] DBG:tm:t_should_relay_response: T_code=0, new_code=408
 [22871] DBG:tm:t_pick_branch: picked branch 0, code 408 (prio=800)
 [22871] DBG:tm:is_3263_failure: dns-failover test: branch=0, last_recv=408, flags=0
 [22871] DBG:tm:t_should_relay_response: trying DNS-based failover
 [22871] DBG:tm:do_dns_failover: new destination available
 [22871] DBG:tm:set_timer: relative timeout is 500000
 [22871] DBG:tm:insert_timer_unsafe: [4]: 0x7ff2f8665098 (115100000)
 [22871] DBG:tm:insert_timer_unsafe: [0]: 0x7ff2f86650c8 (119)
 [22871] DBG:tm:relay_reply: branch=0, save=1, relay=-1
 [22871] DBG:tm:final_response_handler: done
 [22871] DBG:tm:utimer_routine: timer routine:4,tl=0x7ff2f8665098 next=(nil), timeout=115100000
 [22871] DBG:tm:retransmission_handler: retransmission_handler : request resending (t=0x7ff2f8664c78, REGISTER  ... )
 [22871] DBG:tm:set_timer: relative timeout is 1000000
 [22871] DBG:tm:insert_timer_unsafe: [5]: 0x7ff2f8665098 (116100000)
 [22871] DBG:tm:retransmission_handler: retransmission_handler : done
 [22871] DBG:tm:utimer_routine: timer routine:5,tl=0x7ff2f8665098 next=(nil), timeout=116100000
 [22871] DBG:tm:retransmission_handler: retransmission_handler : request resending (t=0x7ff2f8664c78, REGISTER  ... )
 [22871] DBG:tm:set_timer: relative timeout is 2000000
 [22871] DBG:tm:insert_timer_unsafe: [6]: 0x7ff2f8665098 (118100000)
 [22871] DBG:tm:retransmission_handler: retransmission_handler : done
 [22868] DBG:core:parse_msg: SIP Request:
 [22868] DBG:core:parse_msg:  method:  <REGISTER>
 [22868] DBG:core:parse_msg:  uri:     <sip:altanai.com>
 [22868] DBG:core:parse_msg:  version: <SIP/2.0>
 [22868] DBG:core:parse_headers: flags=2
 [22868] DBG:core:parse_via_param: found param type 232, <branch> = <z9hG4bK-d8754z-9b34b37884085f16-1---d8754z->; state=6
 [22868] DBG:core:parse_via_param: found param type 235, <rport> = <n/a>; state=17
 [22868] DBG:core:parse_via: end of header reached, state=5
 [22868] DBG:core:parse_headers: via found, flags=2
 [22868] DBG:core:parse_headers: this is the first via
 [22868] DBG:core:receive_msg: After parse_msg...
 [22868] DBG:core:receive_msg: preparing to run routing scripts...
```
