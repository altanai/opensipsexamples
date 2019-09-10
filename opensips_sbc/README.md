## Opensips as SBC (Session Border Controller )

As an SBC opensips is required to carry out the following tasks 
1. Detect too many Hops
2. Support Auth


Do not allow emergency services like 911 , add "service unavailable 503" reply when called 
```
if ($fU =~ '^(\+)?(1)?911$') {
    xlog("L_INFO", ">>> OUTBOUND CALL $ci callerid is 911 ($fU) rejecting call\n");
    sl_send_reply("503", "Service Unavailable");
    exit;
}
```

As an SBC handle interfacing with multiple gateways , pass all destination gateways as str either in special header or pick up from backend and parse to create seprate uri which whcih will be tried successively 


make and manage using branches 

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
**Solution** 