# Record route in opensips 

Add record route to first message and follow the route in consequent messages .

First message is identifiesbale by not having to tag  , hence on meeting this condition do record_route ()
Else if loose_route or ACK with trsnasaction then relay 

## Features of this script 

detect too many hops
Check againt scanners , exit if found 
Handle Info and options 
record routing
Handle invite , add extra headers and relay 
Register forbidden
Hamdle reply for diffeent reply status codecs

## Debugging 

Issue 1 : ERROR:mi_datagram:mi_init_datagram_server: bind: No such file or directory
CRITICAL:mi_datagram:pre_datagram_process: function mi_init_datagram_server returned with error!!!
ERROR:core:start_module_procs: pre-fork function failed for process "MI Datagram" in module mi_datagram
ERROR:core:main_loop: failed to fork module processes
INFO:core:daemonize: pre-daemon process exiting with -1

Solution : Reload the modules and provide roght ip address to listen. also the headers for dbtext if being used should be following the schema speified for that version of opensips.