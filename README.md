# ipreffeed

## Name

*ipreffeed* - feed IPREF address mapping information to IPREF mapper

## Description

With *ipreffeed* you can provide dynamic IPREF address mapping to IPREF mappers.
The plugin watches queries for AA addresses (IPREF addresses) in selected zones
and passes the resulting answers to configured address mappers.

This plugin can only be used once per Server Block.

## Syntax

~~~
ipref [MAPPER] [ZONES...]
~~~

* **MAPPER** is the endpoint to send the information to. Currently, only unix
sockets are supported. Default is unix:///var/run/ipref-mapper.sock.
* **ZONES** is the list of zones whose address mapping info to feed to IPREF
mappers. If missing information from all zones in the block will be fed.

## Metrics

If monitoring is enabled (via the *prometheus* directive) then the following metric is exported:

* `coredns_ipreffeed_feed_duration_seconds{server}` - duration per feed transfer.
* `coredns_ipreffeed_feed_count_total{server, rcode}` - count of feed transfers.

The `server` label indicates which server handled the request, see the *metrics* plugin for details.

## Examples

Feed information from all zones:
~~~ corefile
. {
    ipreffeed unix:///run/mapper.sock
}
~~~

Feed information from example.org to default mapper.

~~~ corefile
. {
    ipreffeed example.org
}
~~~

or

~~~ corefile
example.org {
    ipreffeed
}
~~~

## Bugs

IPREF needs new DNS resource record type. The plan is to register AA records with IANA. For now, as a workaround to allow development, the unavailable AA records are emulated by embedding them in TXT records.

Currently, only unix socket endpoints are supported which implies the relevant IPREF
zones must be served from the same machine where mapper is located. The plan is
to provide grpc transport which would eliminate this limitation.

## See Also

See <https://github.com/ipref/dns> for information on IPREF addressing.<br/>
See <https://github.com/ipref/meadow> for information on a sample IPREF implementation.<br/>
