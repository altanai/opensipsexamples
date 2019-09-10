# Branch handling in Opensips 

Branch route is an outbound route to handle the SIP requests leaving OpenSIPS
can be used only with TM module since setting a branch route is per request/transaction

used in SIP forking scenarios—parallel or serial forking ( ie an incoming request branches to multiple outgoing requests )
Branch route gives you individual access to each outgoing branch.

Branch route is executed inside the t_relay() function, which is the one sending out the SIP branches on the network