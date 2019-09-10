# Opensips as SBC (Session Border Controller )

As an SBC opensips is required to carry out the following tasks 
1. Detect too many Hops
2. Support Auth
3. Should have scanner to detect malicious hacks 
4. should support branching 
5. perission to check_address on messages 

## Inbound SBC 

carrier / PSTN / external world -> opensips outbound SBC -> custoer premises 

Routing external calls inwards towards customer premises 

stripping + symbol added before numbers 
```
if($rU =~ "^\+.*" )
    $rU=$(rU{s.substr,1,0});
```

Load permission to make sure only allowed ips are calling inbound 
```
check_address("1500", "$si", "$sp", "$(pr{s.tolower})", "$avp(707)", "customer")
```

## Outbound SBC 

custoer premises -> opensips outbound SBC -> externa world / carrier / PSTN 

Do not allow emergency services like 911 , add "service unavailable 503" reply when called 
```
if ($fU =~ '^(\+)?(1)?911$') {
    xlog("L_INFO", ">>> OUTBOUND CALL $ci callerid is 911 ($fU) rejecting call\n");
    sl_send_reply("503", "Service Unavailable");
    exit;
}
```

make and manage using branches 
```
t_on_failure("OUTBOUND"); # failure route
t_on_branch("OUTBOUND");  # branch route
t_on_reply("OUTBOUND");   # reply route

# do branches
serialize_branches(2);
if(!next_branches()) {
    sl_send_reply("503", "No branches");
    exit;
}
```

As an SBC handle interfacing with multiple gateways , pass all destination gateways as str either in special header or pick up from backend and parse to create seprate uri which whcih will be tried successively 
```
$var(x) = "sip:1111111111@127.0.0.1:5067;timeout=90;h=111^sip:2222222222@127.0.0.1:5077;timeout=90;h=222^";
...
while ($var(gw_index) < 100) {
	$var(uri) = $(var(x){s.select,$var(gw_index),^});
    // iterate
    $var(itr) = 0;
    while($var(itr) < $var(ruri_count)) {
        append_branch();
        $var(itr) = $var(itr) + 1;
    }
    $var(gw_index) = $var(gw_index) + 1;
}
...
t_relay();
```
sample outout when 2 gateways are added in outbound opensip SBC
```
------------INVITE ru sip:23456789@x.x.x.x
>>> OUTBOUND CALL 99140NDY4NTFmMDg5NzAyMzNmNmIyMWRhYjBjNGUyMDQ3YTA - Gateways sip:1111111111@127.0.0.1:5067;timeout=90;h=111^sip:2222222222@127.0.0.1:5077;timeout=90;h=222^
>>> OUTBOUND CALL 99140NDY4NTFmMDg5NzAyMzNmNmIyMWRhYjBjNGUyMDQ3YTA - Added branch URI sip:1111111111@127.0.0.1:5067 - Q 1
>>> OUTBOUND CALL 99140NDY4NTFmMDg5NzAyMzNmNmIyMWRhYjBjNGUyMDQ3YTA - Added branch URI sip:2222222222@127.0.0.1:5077 - Q 0.9
>>> OUTBOUND CALL 99140NDY4NTFmMDg5NzAyMzNmNmIyMWRhYjBjNGUyMDQ3YTA - Found 2 Gateways
Trying first sip:1111111111@127.0.0.1:5067
```

Remove all extra X headers when sending to carriers if not, packets can be fragmented or some hardware implementing SIP like a #*!@% may cause issues.
```
remove_hf("^X-*", "r");
```



## Debugging 

**Issue1** : fork is deprecated, use debug_mode
**Solution** : Core script parameter fork  of opensips is Removed in OpenSIPS 2.2 and Replaced by debug_mode parameter.
Enabling the debug_mode option is a fast way to debug and automatically force:
staying in foreground (do not detach from console)
set logging level to 4 (debug)
set logging to standard error
enable core dumping
set UDP worker processes to 2
set TCP worker processes to 2
Default value is false/0 (disabled).

**Issue2** : 'debug' is deprecated, use 'log_level' instead
**Solution** : loglevel Set the logging level (how verbose OpenSIPS should be) . Actual values are:
-3 - Alert level
-2 - Critical level
-1 - Error level
1 - Warning level
2 - Notice level
3 - Info level
4 - Debug level
The 'log_level' parameter is usually used in concordance with 'log_stderror' parameter.

**Issue3** : ERROR:core:main: no transport protocol loaded
**Solution**  Load either / both tcp , udp protocols 
```
loadmodule "proto_udp.so"
loadmodule "proto_tcp.so"
```

**Issue4**  unknown command <append_hf>, missing loadmodule? or : unknown command <is_method>, missing loadmodule?
**Solution** module sipmsgops provides SIP based operations over the messages such as 
append_to_reply(txt)
append_hf(txt)
append_hf(txt, hdr)
insert_hf(txt)
insert_hf(txt, hdr)
append_urihf(prefix, suffix)
is_present_hf(hf_name)
append_time()
is_method(name)
remove_hf(hname [,flags])
has_body(), has_body(mime)
is_audio_on_hold()
is_privacy(privacy_type