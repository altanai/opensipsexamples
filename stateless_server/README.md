# Proxy server 

stateless proxy server . Here stateless signifies that it does not maintain the dialog state information .

## Debugging 

**Issue1:** ERROR:mi_datagram:mi_init_datagram_server: bind: No such file or directory
CRITICAL:mi_datagram:pre_datagram_process: function mi_init_datagram_server returned with error!!!
ERROR:core:start_module_procs: pre-fork function failed for process "MI Datagram" in module mi_datagram
ERROR:core:main_loop: failed to fork module processes

**Solution** check the location of opensips.cfg file . If necessary give the path opensip command like 
```sh
opensips -f /etc/opensips/opensips.cfg -E
```