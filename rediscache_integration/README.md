# Redis Cache Integration in opensips 

Any gateway voip server will need to access DB and cache often to fetch paramaters and frequently used parameters to perform call routing .
Redis cache module in opensips lets us write to cache memory to perform quick read , write of key value pairs

## Install redis db 
To install 


## configure redis cache into opensips 

To set a value
```
	$var(my_hash) = "my_hash_name";
	$var(my_key) = "my_key_name";
	$var(my_value) = "my_key_value";
	cache_raw_query("redis","HSET $var(my_hash) $var(my_key) $var(my_value)");
```

To get a avalue
```
	cache_raw_query("redis","HGET $var(my_hash) $var(my_key)","$avp(result)");
	xlog("We have fetched $avp(result) \n");
```

## Debuging 

**Issue1** : ERROR:core:load_module: module 'cachedb_redis.so' not found in '/usr/lib/x86_64-linux-gnu/opensips/modules/'
CRITICAL:core:yyerror: parse error in config file /etc/opensips/opensips2.cfg, line 47, column 13-14: failed to load module cachedb_redis.so
ERROR:core:set_mod_param_regex: no module matching cachedb_redis found
CRITICAL:core:yyerror: parse error in config file /etc/opensips/opensips2.cfg, line 48, column 20-21: Parameter <cachedb_url> not found in module <cachedb_redis> - can't set
ERROR:core:set_mod_param_regex: no module matching cachedb_redis found
CRITICAL:core:yyerror: parse error in config file /etc/opensips/opensips2.cfg, line 49, column 21-22: Parameter <connect_timeout> not found in module <cachedb_redis> - can't set
ERROR:core:set_mod_param_regex: no module matching cachedb_redis found
CRITICAL:core:yyerror: parse error in config file /etc/opensips/opensips2.cfg, line 50, column 21-22: Parameter <query_timeout> not found in module <cachedb_redis> - can't set
**Solution** Clearly redis db module is eiter not installed , not comptible or path of modules is incorrect 
First list all instaled module 
```
apt list --installed | grep opensips 
opensips/trusty,now 2.2.7-1 amd64 [installed]
opensips-json-module/trusty,now 2.2.7-1 amd64 [installed]
opensips-postgres-module/trusty,now 2.2.7-1 amd64 [installed]
opensips-regex-module/trusty,now 2.2.7-1 amd64 [installed]
...
```
If redis doesnt feature then install redis moudle as given in section "onfigure redis cache into opensips" above 


Ref :
https://opensips.org/html/docs/modules/2.2.x/cachedb_redis.html


