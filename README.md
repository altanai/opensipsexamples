## Opensips 

"multi-functional, multi-purpose signaling SIP server used by carriers, telecoms or ITSPs for solutions like Class4/5 Residential Platforms, Trunking / Wholesale, Enterprise / Virtual PBX Solutions, Session Border Controllers, Application Servers, Front-End Load Balancers, IMS Platforms, Call Centers, and many others" - opensips.org

Opensips introduction , Installation and menuconfig - https://telecom.altanai.com/2018/06/06/opensips/

This repo conains examples of opensips configuration files.
An opensips.cfg contains parameters that control the OpenSIPS core and modules. Also congains call routing logic.

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
To run 
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

Opensips Module - https://telecom.altanai.com/2018/07/19/opensips-modules/
Opensips as SIP gateway - https://telecom.altanai.com/2018/01/16/opensips-modules-2/
