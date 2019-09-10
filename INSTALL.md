#opensips Installation 

Opensips introduction , Installation and menuconfig - https://telecom.altanai.com/2018/06/06/opensips/

source list 
```
deb http://apt.opensips.org trusty 2.2-releases
```

On ubuntu 
```
apt-get install opensips
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  docutils-common libberkeleydb-perl libfrontier-rpc-perl liblcms2-2
  libmemcached10 libmicrohttpd10 libnet-ip-perl libodbc1 libpaper-utils
  libpaper1 libwebp5 libwebpmux1 libxml-parser-perl python3-bcdoc
  python3-botocore python3-dateutil python3-docutils python3-jmespath
  python3-pil python3-ply python3-pygments python3-roman python3-rsa
Use 'apt-get autoremove' to remove them.
Suggested packages:
  opensips-b2bua-module opensips-berkeley-module opensips-carrierroute-module
  opensips-compression-module opensips-console opensips-cpl-module
  opensips-dbhttp-module opensips-dialplan-module opensips-emergency-module
  opensips-geoip-module opensips-http-modules opensips-identity-module
  opensips-jabber-module opensips-json-module opensips-ldap-modules
  opensips-lua-module opensips-memcached-module opensips-mysql-module
  opensips-mongodb-module opensips-perl-modules opensips-postgres-module
  opensips-presence-modules opensips-rabbitmq-module opensips-radius-modules
  opensips-redis-module opensips-regex-module opensips-restclient-module
  opensips-sctp-module opensips-snmpstats-module opensips-sqlite-module
  opensips-tls-module opensips-wss-module opensips-tlsmgm-module
  opensips-unixodbc-module opensips-xmlrpc-module opensips-xmpp-module
```

List apt opensips packages 
```
>apt list | grep opensips

opensips/trusty,now 2.2.7-1 amd64 [residual-config]
opensips-b2bua-module/trusty 2.2.7-1 amd64
opensips-berkeley-bin/trusty 2.2.7-1 amd64
opensips-berkeley-module/trusty 2.2.7-1 amd64
opensips-carrierroute-module/trusty 2.2.7-1 amd64
opensips-compression-module/trusty 2.2.7-1 amd64
opensips-console/trusty 2.2.7-1 amd64
opensips-cpl-module/trusty 2.2.7-1 amd64
opensips-dbg/trusty 2.2.7-1 amd64
opensips-dbhttp-module/trusty 2.2.7-1 amd64
opensips-dialplan-module/trusty 2.2.7-1 amd64
opensips-emergency-module/trusty 2.2.7-1 amd64
opensips-geoip-module/trusty 2.2.7-1 amd64
opensips-http-modules/trusty 2.2.7-1 amd64
opensips-identity-module/trusty 2.2.7-1 amd64
opensips-jabber-module/trusty 2.2.7-1 amd64
opensips-json-module/trusty 2.2.7-1 amd64
opensips-ldap-modules/trusty 2.2.7-1 amd64
opensips-lua-module/trusty 2.2.7-1 amd64
opensips-memcached-module/trusty 2.2.7-1 amd64
opensips-mysql-module/trusty 2.2.7-1 amd64
opensips-perl-modules/trusty 2.2.7-1 amd64
opensips-postgres-module/trusty 2.2.7-1 amd64
opensips-presence-modules/trusty 2.2.7-1 amd64
opensips-rabbitmq-module/trusty 2.2.7-1 amd64
opensips-radius-modules/trusty 2.2.7-1 amd64
opensips-redis-module/trusty 2.2.7-1 amd64
opensips-regex-module/trusty 2.2.7-1 amd64
opensips-restclient-module/trusty 2.2.7-1 amd64
opensips-sctp-module/trusty 2.2.7-1 amd64
opensips-snmpstats-module/trusty 2.2.7-1 amd64
opensips-sqlite-module/trusty 2.2.7-1 amd64
opensips-tls-module/trusty 2.2.7-1 amd64
opensips-tlsmgm-module/trusty 2.2.7-1 amd64
opensips-unixodbc-module/trusty 2.2.7-1 amd64
opensips-wss-module/trusty 2.2.7-1 amd64
opensips-xmlrpc-module/trusty 2.2.7-1 amd64
opensips-xmpp-module/trusty 2.2.7-1 amd64
```

show package informtion on opensips
```
> apt show opensips

Package: opensips
Version: 2.2.7-1
Maintainer: Răzvan Crainea <razvan@opensips.org>
Installed-Size: 8378 kB
Depends: adduser, init-system-helpers (>= 1.13~), libc6 (>= 2.16), libncurses5 (>= 5.5-5~), libtinfo5
Suggests: opensips-b2bua-module, opensips-berkeley-module, opensips-carrierroute-module, opensips-compression-module, opensips-console, opensips-cpl-module, opensips-dbhttp-module, opensips-dialplan-module, opensips-emergency-module, opensips-geoip-module, opensips-http-modules, opensips-identity-module, opensips-jabber-module, opensips-json-module, opensips-ldap-modules, opensips-lua-module, opensips-memcached-module, opensips-mysql-module, opensips-mongodb-module, opensips-perl-modules, opensips-postgres-module, opensips-presence-modules, opensips-rabbitmq-module, opensips-radius-modules, opensips-redis-module, opensips-regex-module, opensips-restclient-module, opensips-sctp-module, opensips-snmpstats-module, opensips-sqlite-module, opensips-tls-module, opensips-wss-module, opensips-tlsmgm-module, opensips-unixodbc-module, opensips-xmlrpc-module, opensips-xmpp-module
Section: net
Priority: optional
Homepage: http://www.opensips.org/
SHA512: 715c2d0e5b300d2e7a01325fc1e5e6b3130e032270637aedd39900980cb8033691aff6425aabd971c95ee290567957f01497b3343ab26bfdf926f3719cf2b0b9
Download-Size: 2702 kB
APT-Sources: http://apt.opensips.org/ trusty/2.2-releases amd64 Packages
Description: very fast and configurable SIP server
OpenSIPS is a very fast and flexible SIP (RFC3261) server. Written entirely in C, OpenSIPS can handle thousands callsper second even on low-budget hardware.

.C Shell-like scripting language provides full control over the server's behaviour. Its modular architecture allows only required functionality to be loaded.

. Among others, the following modules are available: Digest Authentication, CPL scripts, Instant Messaging, MySQL support, Presence Agent, Radius Authentication, Record Routing, SMS Gateway, Jabber/XMPP Gateway, Transaction Module, Registrar and User Location, Load Balaning/Dispatching/LCR, XMLRPC Interface.

. This package contains the main OpenSIPS binary along with the principal modules and support binaries including opensipsmc configuration tool.
```