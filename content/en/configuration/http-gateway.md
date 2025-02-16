---
title: 'HTTP Gateway'
description: 'Service configuration overview'
date: '2021-10-01'
lastmod: '2021-10-01'
categories: [configuration, deployment]
keywords: [configuration]
menu:
  docs:
    parent: configuration
    weight: 30
toc: true
---

HTTP Gateway exposes the client's [REST API](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/plgd-dev/hub/main/http-gateway/swagger.yaml) to manage user's devices, as well as the Web UI known as [plgd Dashboard](https://try.plgd.cloud).

## Docker Image

```bash
docker pull plgd/http-gateway:latest
```

## YAML Configuration

A configuration template is available on [http-gateway/config.yaml](https://github.com/plgd-dev/hub/blob/main/http-gateway/config.yaml).

### Logging

| Property | Type | Description | Default |
| ---------- | -------- | -------------- | ------- |
| `log.level` | string | `Logging enabled from level.` | `"info"` |
| `log.encoding` | string | `Logging format. The supported values are: "json", "console"` | `"json"` |
| `log.stacktrace.enabled` | bool | `Log stacktrace.` | `"false` |
| `log.stacktrace.level` | string | `Stacktrace from level.` | `"warn` |
| `log.encoderConfig.timeEncoder` | string | `Time format for logs. The supported values are: "rfc3339nano", "rfc3339".` | `"rfc3339nano` |

### HTTP API

APIs of the HTTP Gateway service as defined [uri](https://github.com/plgd-dev/hub/blob/main/http-gateway/uri/uri.go) and [swagger](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/plgd-dev/hub/main/http-gateway/swagger.yaml) for REST API.

| Property | Type | Description | Default |
| ---------- | -------- | -------------- | ------- |
| `api.http.address` | string | `Listen specification <host>:<port> for http client connection.` | `"0.0.0.0:9100"` |
| `api.http.tls.caPool` | string | `File path to the root certificate in PEM format which might contain multiple certificates in a single file.` |  `""` |
| `api.http.tls.keyFile` | string | `File path to private key in PEM format.` | `""` |
| `api.http.tls.certFile` | string | `File path to certificate in PEM format.` | `""` |
| `api.http.tls.clientCertificateRequired` | bool | `If true, require client certificate.` | `true` |
| `api.http.websocket.streamBodyLimit` | int | `Limit a size for the buffer used while reading the grpc stream.` | `262144` |
| `api.http.websocket.pingFrequency` | string | `Ping frequency specifies the ping interval between pings.` | `10s` |
| `api.http.authorization.authority` | string | `Authority is the address of the token-issuing authentication server. Services will use this URI to find and retrieve the public key that can be used to validate the token’s signature.` | `""` |
| `api.http.authorization.audience` | string | `Identifier of the API configured in your OAuth provider.` | `""` |
| `api.http.authorization.http.maxIdleConns` | int | `It controls the maximum number of idle (keep-alive) connections across all hosts. Zero means no limit.` | `16` |
| `api.http.authorization.http.maxConnsPerHost` | int | `It optionally limits the total number of connections per host, including connections in the dialing, active, and idle states. On limit violation, dials will block. Zero means no limit.` | `32` |
| `api.http.authorization.http.maxIdleConnsPerHost` | int | `If non-zero, controls the maximum idle (keep-alive) connections to keep per-host. If zero, DefaultMaxIdleConnsPerHost is used.` | `16` |
| `api.http.authorization.http.idleConnTimeout` | string | `The maximum amount of time an idle (keep-alive) connection will remain idle before closing itself. Zero means no limit.` | `30s` |
| `api.http.authorization.http.timeout` | string | `A time limit for requests made by this Client. A Timeout of zero means no timeout.` | `10s` |
| `api.http.authorization.http.tls.caPool` | string | `File path to the root certificate in PEM format which might contain multiple certificates in a single file.` |  `""` |
| `api.http.authorization.http.tls.keyFile` | string | `File path to private key in PEM format.` | `""` |
| `api.http.authorization.http.tls.certFile` | string | `File path to certificate in PEM format.` | `""` |
| `api.http.authorization.http.tls.useSystemCAPool` | bool | `If true, use system certification pool.` | `false` |

### GRPC Gateway Client

Client configurations to internally connect to GRPC Gateway service.

| Property | Type | Description | Default |
| ---------- | -------- | -------------- | ------- |
| `clients.grpcGateway.grpc.address` | string | `GRPC Gateway service address.` | `"127.0.0.1:9100"` |
| `clients.grpcGateway.grpc.tls.caPool` | string | `File path to the root certificate in PEM format which might contain multiple certificates in a single file.` |  `""` |
| `clients.grpcGateway.grpc.tls.keyFile` | string | `File path to private key in PEM format.` | `""` |
| `clients.grpcGateway.grpc.tls.certFile` | string | `File path to certificate in PEM format.` | `""` |
| `clients.grpcGateway.grpc.tls.useSystemCAPool` | bool | `If true, use system certification pool.` | `false` |
| `clients.grpcGateway.grpc.keepAlive.time` | string | `After a duration of this time if the client doesn't see any activity it pings the server to see if the transport is still alive.` | `10s` |
| `clients.grpcGateway.grpc.keepAlive.timeout` | string | `After having pinged for keepalive check, the client waits for a duration of Timeout and if no activity is seen even after that the connection is closed.` | `20s` |
| `clients.grpcGateway.grpc.keepAlive.permitWithoutStream` | bool | `If true, client sends keepalive pings even with no active RPCs. If false, when there are no active RPCs, Time and Timeout will be ignored and no keepalive pings will be sent.` | `false` |

### Web UI

These configurations are for `PLGD Dashboard` as described in [here](https://github.com/plgd-dev/hub/blob/main/docs/guide/developing/dashboard.md).

| Property | Type | Description | Default |
| ---------- | -------- | -------------- | ------- |
| `ui.enabled` | string | `Set to true if you would like to run the web UI.` | `false` |
| `ui.directory` | string | `Path to default web ui built by nodejs` | `"/usr/local/var/www"` |
| `ui.webConfiguration.authority` | string | `Authority is the address of the token-issuing authentication server.` | `""` |
| `ui.webConfiguration.httpGatewayAddress` | string | `External address of Http gateway service.` | `""` |
| `ui.webConfiguration.webOAuthClient.clientID` | string | `Client ID to exchange an authorization code for an access token.` | `""` |
| `ui.webConfiguration.webOAuthClient.audience` | string | `Identifier of the API configured in your OAuth provider.` | `""` |
| `ui.webConfiguration.webOAuthClient.scopes` | string array | `List of required scopes.` | `""` |
| `ui.webConfiguration.deviceOAuthClient.clientID` | string | `Client ID to get an authorization code for the device.` | `""` |
| `ui.webConfiguration.deviceOAuthClient.audience` | string | `Identifier of the API configured in your OAuth provider.` | `""` |
| `ui.webConfiguration.deviceOAuthClient.scopes` | string array | `List of required scopes.` | `""` |
| `ui.webConfiguration.deviceOAuthClient.providerName` | string | `Name of provider, which needs to be set to cloud resource during cloud provisioning.` | `"plgd"` |

> Note that the string type related to time (i.e. timeout, idleConnTimeout, expirationTime) is decimal numbers, each with optional fraction and a unit suffix, such as "300ms", "1.5h" or "2h45m". Valid time units are "ns", "us", "ms", "s", "m", "h".
