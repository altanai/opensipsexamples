# Opensips 

"multi-functional, multi-purpose signaling SIP server used by carriers, telecoms or ITSPs for solutions like Class4/5 Residential Platforms, Trunking / Wholesale, Enterprise / Virtual PBX Solutions, Session Border Controllers, Application Servers, Front-End Load Balancers, IMS Platforms, Call Centers, and many others" - opensips.org

Opensips introduction , Installation and menuconfig - https://telecom.altanai.com/2018/06/06/opensips/

### Types of routes 
1. route
2. branch_route
3. failure_route
4. onreply_route
5. error_route
6. local_route
7. startup_route
8. timer_route
9. event_route

This repo conains examples of opensips configuration files.
An opensips.cfg contains parameters that control the OpenSIPS core and modules. Also congains call routing logic.

## Configuration 

To test validation of cfg file run run -C which returns 0 if ok and -1 if faulty
```
opensips -C opensips.cfg
```

After instalaltion check the version of opensips
```sh
opensips -V
version: opensips 1.11.9-tls (x86_64/linux)
flags: STATS: On, USE_IPV6, USE_TCP, USE_TLS, DISABLE_NAGLE, USE_MCAST, SHM_MEM, SHM_MMAP, PKG_MALLOC, F_MALLOC, FAST_LOCK-ADAPTIVE_WAIT
ADAPTIVE_WAIT_LOOPS=1024, MAX_RECV_BUFFER_SIZE 262144, MAX_LISTEN 16, MAX_URI_SIZE 1024, BUF_SIZE 65535
poll method support: poll, epoll_lt, epoll_et, sigio_rt, select.
git revision: 5f61644
main.c compiled on 20:29:43 Jun 29 2018 with gcc 4.8
```

### To run 
```sh
opensips -E opensips.cfg -E
```
note : match the opensips version with module version to avoid usuage of depricate functiosn or params . Infact opensips itsefl throws a warhning when incompatible version of core and modules are loaded together such as 
```
Jul  5 08:53:49 [13720] ERROR:core:version_control: module version mismatch for signaling; core: opensips 2.2.7 (x86_64/linux); module: opensips 3.0.0-beta (x86_64/linux)
```
or 
```sh
Jul  5 10:41:01 [17058] ERROR:core:version_control: module compile flags mismatch for signaling  
core: STATS: On, DISABLE_NAGLE, USE_MCAST, SHM_MMAP, PKG_MALLOC, F_MALLOC, FAST_LOCK-ADAPTIVE_WAIT 
module: STATS: On, EXTRA_DEBUG, DISABLE_NAGLE, USE_MCAST, NO_DEBUG, NO_LOG, SHM_MMAP, PKG_MALLOC, HP_MALLOC, DBG_MALLOC, FAST_LOCK-FUTEX-NOSMP, USE_PTHREAD_MUTEX, USE_POSIX_SEM, USE_SYSV_SEM, DBG_LOCK
```
note : every change in cfg requires server restart 

## opensips commands 

using version 2.2.7 
Usage: opensips -l address [-l address ...] [options]
Options:
-f file      Configuration file (default /etc/opensips/opensips.cfg)
-c           Check configuration file for errors
-C           Similar to '-c' but in addition checks the flags of exported functions from included route blocks
-l address   Listen on the specified address/interface (multiple -l mean listening on more addresses).  
			 The address format is [proto:]addr[:port], where proto=udp|tcp and addr= host|ip_address|interface_name. 
			 E.g: -l locahost, -l udp:127.0.0.1:5080, -l eth0:5062 
			 The default behavior is to listen on all the interfaces.
-n processes Number of worker processes to fork per UDP interface(default: 8)
-r           Use dns to check if is necessary to add a "received=" field to a via
-R           Same as `-r` but use reverse dns; (to use both use `-rR`)
-v           Turn on "via:" host checking when forwarding replies
-d           Debugging mode (multiple -d increase the level)
-D           Run in debug mode
-F           Daemon mode, but leave main process foreground
-E           Log to stderr
-N processes Number of TCP worker processes (default: equal to `-n`)
-W method    poll method
-V           Version number
-h           This help message
-b nr        Maximum receive buffer size which will not be exceeded by auto-probing procedure even if  OS allows
-m nr        Size of shared memory allocated in Megabytes
-M nr        Size of pkg memory allocated in Megabytes
-w dir       Change the working directory to "dir" (default "/")
-t dir       Chroot to "dir"
-u uid       Change uid 
-g gid       Change gid 
-P file      Create a pid file
-G file      Create a pgid file

## opensip routes

Request route
Branch route
Failure route
Reply route
Local route
Start up route
Timer route
Event route
Error route

In addition to main route opensips can have multiple sub routes with names such as 
route[name ] {
...
}

## opensipsctl

command line utility for opensips

Existing commands:

 -- command 'start|stop|restart|trap'

 trap ............................... trap with gdb OpenSIPS processes
 restart ............................ restart OpenSIPS
 start .............................. start OpenSIPS
 stop ............................... stop OpenSIPS

 -- command 'acl' - manage access control lists (acl)

 acl show [<username>] .............. show user membership
 acl grant <username> <group> ....... grant user membership (*)
 acl revoke <username> [<group>] .... grant user membership(s) (*)

 -- command 'cr' - manage carrierroute tables

 cr show ....................................................... show tables
 cr reload ..................................................... reload tables
 cr dump ....................................................... show in memory tables
 cr addrt <routing_tree_id> <routing_tree> ..................... add a tree
 cr rmrt  <routing_tree> ....................................... rm a tree
 cr addcarrier <carrier> <scan_prefix> <domain> <rewrite_host> ................
               <prob> <strip> <rewrite_prefix> <rewrite_suffix> ...............
               <flags> <mask> <comment> .........................add a carrier
               (prob, strip, rewrite_prefix, rewrite_suffix,...................
                flags, mask and comment are optional arguments) ...............
 cr rmcarrier  <carrier> <scan_prefix> <domain> ................ rm a carrier

 -- command 'rpid' - manage Remote-Party-ID (RPID)

 rpid add <username> <rpid> ......... add rpid for a user (*)
 rpid rm <username> ................. set rpid to NULL for a user (*)
 rpid show <username> ............... show rpid of a user

 -- command 'add|passwd|rm' - manage subscribers

 add <username> <password> .......... add a new subscriber (*)
 passwd <username> <passwd> ......... change user's password (*)
 rm <username> ...................... delete a user (*)

 -- command 'add|dump|reload|rm|show' - manage address

 address show ...................... show db content
 address dump ...................... show cache content
 address reload .................... reload db table into cache
 address add <grp> <ip> <mask> <port> <proto> [<context_info>] [<pattern>]
             ....................... add a new entry
	     ....................... (from_pattern and tag are optional arguments)
 address rm <grp> <ip> <mask> <port> ............... remove all entries 
	     ....................... for the given grp ip mask port

 -- command 'dr' - manage dynamic routing

   * Examples: dr addgw '1' 10 '192.168.2.2' 0 '' 'GW001' 0 'first_gw'
   *           dr addgw '2' 20 '192.168.2.3' 0 '' 'GW002' 0 'second_gw'
   *           dr rmgw 2
   *           dr addgrp 'alice' 'example.com' 10 'first group'
   *           dr rmgrp 1
   *           dr addcr 'cr_1' '10' 0 'CARRIER_1' 'first_carrier'
   *           dr rmcr 1
   *           dr addrule '10,20' '+1' '20040101T083000' 0 0 '1,2' 'NA_RULE' 'NA routing'
   *           dr rmgrule 1
 dr show ............................ show dr tables
 dr addgw <gwid> <type> <address> <strip> <pri_prefix>
          <attrs> <probe_mode> <description>
    ................................. add gateway
 dr rmgw <id> ....................... delete gateway
 dr addgrp <username> <domain> <groupid> <description>
    ................................. add gateway group
 dr rmgrp <id> ...................... delete gateway group
 dr addcr <carrierid> <gwlist> <flags> <attrs> <description>
          ........................... add carrier
 dr rmcr <id> ....................... delete carrier
 dr addrule <groupid> <prefix> <timerec> <priority> <routeid>
            <gwlist> <attrs> <description>
    ................................. add rule
 dr rmrule <ruleid> ................. delete rule
 dr reload .......................... reload dr tables
 dr gw_status ....................... show gateway status
 dr carrier_status .................. show carrier status

 -- command 'dispatcher' - manage dispatcher

   * Examples:  dispatcher addgw 1 sip:1.2.3.1:5050 '' 0 50 'og1' 'Outbound Gateway1'
   *            dispatcher addgw 2 sip:1.2.3.4:5050 '' 0 50 'og2' 'Outbound Gateway2'
   *            dispatcher rmgw 4
 dispatcher show ..................... show dispatcher gateways
 dispatcher reload ................... reload dispatcher gateways
 dispatcher dump ..................... show in memory dispatcher gateways
 dispatcher addgw <setid> <destination> <socket> <state> <weight> <attrs> [description]
            .......................... add gateway
 dispatcher rmgw <id> ................ delete gateway

 -- command 'registrant' - manage registrants

   * Examples:  registrant add sip:opensips.org '' sip:user@opensips.org '' user password sip:user@localhost '' 3600 ''
 registrant show ......................... show registrant table
 registrant dump ......................... show registrant status
 registrant add <registrar> <proxy> <aor> <third_party_registrant>
                <username> <password> <binding_URI> <binding_params>
                <expiry> <forced_socket> . add a registrant
 registrant rm ........................... removes the entire registrant table
 registrant rmaor <id> ................... removes the gived aor id

 -- command 'cisco_restart' - restart CISCO phone (NOTIFY)

 cisco_restart <uri> ................ restart phone configured for <uri>

 -- command 'online' - dump online users from memory

 online ............................. display online users

 -- command 'monitor' - show internal status

 monitor ............................ show server's internal status

 -- command 'ping' - ping a SIP URI (OPTIONS)

 ping <uri> ......................... ping <uri> with SIP OPTIONS

 -- command 'ul|alias' - manage user location or aliases

 ul show [<username>]................ show in-RAM online users
 ul show --brief..................... show in-RAM online users in short format
 ul rm <username> [<contact URI>].... delete user's usrloc entries
 ul add <username> <uri> ............ introduce a permanent usrloc entry
 ul add <username> <uri> <expires> .. introduce a temporary usrloc entry

 -- command 'fifo'

 fifo ............................... send raw FIFO command


Ref :

Opensips Module - https://telecom.altanai.com/2018/07/19/opensips-modules/
Opensips as SIP gateway - https://telecom.altanai.com/2018/01/16/opensips-modules-2/
