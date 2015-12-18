---
title: Name Servers
excerpt: This page documents the DNSimple Name Servers API v1.
---

# Name Server API

* TOC
{:toc}


## List domain name servers {#list}

List the delegated name servers for a domain.

    GET /domains/:domain/name_servers

### Parameters

Name | Type | Description
-----|------|------------
`:domain` | `string`, `integer` | The domain name or id

### Example

List delegated name servers for domain `example.com`:

    curl  -H 'X-DNSimple-Token: <email>:<token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          https://api.dnsimple.com/v1/domains/example.com/name_servers

### Response

Responds with HTTP 200 on success, returns the array of assigned name servers.

~~~json
[
  "ns1.example.com",
  "ns2.example.com",
  "ns3.example.com",
  "ns4.example.com"
]
~~~


## Change domain name servers {#change}

Change the delegated name servers for a domain, either to external name servers or back to DNSimple's name servers.

    POST /domains/:domain/name_servers

### Parameters

Name | Type | Description
-----|------|------------
`:domain` | `string`, `integer` | The domain name or id

### Example

Change delegated name servers for domain `example.com`:

    curl  -H 'X-DNSimple-Token: <email>:<token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          -d '<json>' \
          https://api.dnsimple.com/v1/domains/example.com/name_servers

### Input

This API accepts up to 6 name servers.

~~~json
{
  "name_servers": {
    "ns1": "ns1.example.com",
    "ns2": "ns2.example.com",
    "ns3": "ns3.example.com",
    "ns4": "ns4.example.com"
  }
}
~~~

To change the name servers back to DNSimple's name servers, send the following body:

~~~json
{
  "name_servers": {
    "ns1": "ns1.dnsimple.com",
    "ns2": "ns2.dnsimple.com",
    "ns3": "ns3.dnsimple.com",
    "ns4": "ns4.dnsimple.com"
  }
}
~~~

### Response

Responds with HTTP 200 on success, returns the array of assigned name servers.

~~~json
[
  "ns1.example.com",
  "ns2.example.com",
  "ns3.example.com",
  "ns4.example.com"
]
~~~

Responds with HTTP 400 if bad request.


## Register a name server at the registry {#register}

    POST /domains/:domain/registry_name_servers

### Parameters

Name | Type | Description
-----|------|------------
`:domain` | `string`, `integer` | The domain name or id

The domain must be registered with DNSimple in order for this command to work.

### Example

Register a name server belonging to `example.com`:

    curl  -H 'X-DNSimple-Token: <email>:<token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          -d '<json>' \
          https://api.dnsimple.com/v1/domains/example.com/registry_name_servers

### Input

Name | Type | Description
-----|------|------------
`name_server.name` | `string` | **Required**.
`name_server.ip` | `string` | **Required**.

~~~json
{
  "name_server": {
    "name": "ns1.example.com",
    "ip": "1.2.3.4"
  }
}
~~~

### Response

Responds with HTTP 201 on success, returns the name server.

~~~json
{
  "name": "ns1.example.com",
  "ip": 12
}
~~~

Responds with HTTP 400 if bad request.


## De-register a name server at the registry {#deregister}

    DELETE /domains/:domain/registry_name_servers/:name

De-register the name server `ns1.example.com` belonging to `example.com`:

### Parameters

Name | Type | Description
-----|------|------------
`:domain` | `string`, `integer` | The domain name or id
`:name` | `string` | The name server name

### Example

    curl  -H 'X-DNSimple-Token: <email>:<token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X DELETE \
          https://api.dnsimple.com/v1/domains/example.com/registry_name_servers/ns1.example.com

### Response

Responds with HTTP 204 on success.

Responds with HTTP 400 if bad request.
