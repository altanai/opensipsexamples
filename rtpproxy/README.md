## RTPPRoxy 

symmetric RTP proxy , can be used with any SIP Server proxy / b2bua 
rewrites SDP bodies in SIP messages that it processes
relays RTP packets to reach either peers via  NAT(s) (Network Address Translator) 
also bridging IPv4/IPv6 traversal of RTP packets.

* syntax *

rtpproxy [-?] [-2] [-f] [-v] [-R] [-l addr1[/addr2]] [-6 addr1[/addr2]] [-s ctrl_socket] [-t tos] [-p pidfile] [-T max_ttl] [-r rdir [-S sdir]] [-m min_port] [-M max_port] [-u uname[:gname]] [-F] [-i] [-n timeout_socket] [-P] [-a] [-d log_level[:log_facility]]

*options*

-? help

-2  - Send every RTP packet twice in sessions that use low-bitrate codecs. Only packets that are smaller than 128 bytes will be sent twice. This option can improve audio quality on lossy links.

-f - Rtpproxy will stay in foreground mode if this option is set.

-v - Show version of program

-l addr1[/addr2] - IPv4 listen IP address(es). If two addresses have been specified then rtpproxy will work in bridging mode

-6 addr1[/addr2] - IPv6 listen IP address(es). If two addresses have been specified then rtpproxy will work in bridging mode.

-s ctrl_socket - configures rtpproxy control socket used by nathelper module to create/modify/delete RTP sessions to be relayed. Format of ctrl_socket is <type>:<socket>. 
Following types are supported:
	- udp: Create UDP control socket eg : -s udp:127.0.0.1:9000
	- udp6 : UDP/IPv6 for control messages eg :  -s udp6:::1:9000
	- unix : both rpxy and nathelper must be running on same node eg : -s unix:/var/run/rtpproxy.sock

-t tos -  Type of Service in the outgoing UDP packets default 0xB8

-r rec_dir - Directory where recorded RTP sessions will be stored.