# Permission Control 


## address table 

id				unsigned int	10	default	no	primary	autoincrement	 unique ID
grp				unsigned short	5	0	no	 	 	The group ID - each address may belong to a group/set
ip				string	50	default	no	 	 	IP address, IPv4 or IPv6 format
mask			char	not specified	32	no	 	Network mask, a number from 0 to 128; It should be up to 32 if the IP is v4 and up to 128 if the IP is v6
port			unsigned short	5	0	no	 	 	Port number, 0 value meaning 'any' (wildcard)
proto			string	4	'any'	no	 	 	Transport protocol is either "any" or equal to transport protocol of request. Possible values that can be stored are "any", "udp", "tcp", "tls", and "sctp".
pattern			string	64	NULL	yes	 	 	A shell wildcard pattern to be used for matching string provided by the check address functions.
context_info	string	32	NULL	yes	 	 	Extra context information, not used by OpenSIPS, but simply exposed to the script level via scripting variables

Schema for dbtext 
```
id(int,auto) grp(int) ip(string) mask(int) port(int) proto(string) pattern(string,null) context_info(string,null)
```

## check address 

format 
check_address([partition:]group_id, ip, port, proto [, context_info [, pattern]])
exmaple
check_address("2", "$ip", "$p", "$(pr{s.tolower})", "$avp(ctx)", "callercompany")

1:2:106.51.78.22:32:0:udp:carrier:carrier
udp sip:106.51.78.22:39778

## debugging

**Issue1** ERROR:core:db_check_api: module db_postgres does not export db_use_table function
ERROR:permissions:init_address: load a database support module
ERROR:permissions:mod_init: failed to initialize the allow_address function
ERROR:core:init_mod: failed to initialize module permissions
**Solution** somewhere in the code is modparam("permissions", "db_url", "postgres:// .. ) therefore install postgress module and load it 
```
apt install opensips-postgres-module
```
```
loadmodule "db_postgres.so"
```
**Issue2** INFO:core:init_sock_keepalive: TCP keepalive enabled on socket 9
ERROR:mi_datagram:mi_init_datagram_server: bind: No such file or directory
CRITICAL:mi_datagram:pre_datagram_process: function mi_init_datagram_server returned with error!!!
ERROR:core:start_module_procs: pre-fork function failed for process "MI Datagram" in module mi_datagram
ERROR:core:main_loop: failed to fork module processes
**Solution** opens datagram ports , TBD
