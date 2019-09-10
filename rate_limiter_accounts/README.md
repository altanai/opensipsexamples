# Limit calls rates by external REST APIs

```
loadmodule "rest_client.so"
...
modparam("rest_client", "curl_timeout", 5)
modparam("rest_client", "connection_timeout", 5)
modparam("rest_client", "connect_poll_interval", 2)
modparam("rest_client", "max_async_transfers", 300)
```
