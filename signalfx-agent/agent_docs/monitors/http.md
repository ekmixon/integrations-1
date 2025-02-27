
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# http

Monitor Type: `http` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/http))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Overview

This monitor will generate metrics based on whether the HTTP response from
the configured URL match expectations (e.g. correct body, status code,
etc).

TLS information will automatically be fetched if applicable (from base URL
or redirection depending on `useHTTPS` parameter).

<!--- SETUP --->
## Setup

To create a webcheck from a URL, you need to split it into different 
configuration options. All of these will determine the `url` dimension 
value from its "normalized" url `{scheme}://{host}:{port}{path}`:
  * `scheme` will be `https` if `useHTTPS:true` or `http` else.
  * `host` should be the hostname of the site to check. It is mandatory.
  * `port` should be the port to connect to. If not defined, it will 
  be `443` if `useHTTPS:true` or `80` else.
  * `path` will contain the full query including resource path and 
  finally the `GET` method parameters with `?` separator.

__Notice__: `:port` will be removed from `url` if default because 
it is implicit and makes the behavior similar to what `curl` does.

In addition to information from the URL you can also configure the 
behavior of the request done on this URL:
  * request type like `GET` or `POST` are defined from `method`. [See go 
  doc](https://golang.org/src/net/http/method.go) for full list of 
  available methods.
  * basic authentification could be done from `username` and `password`
  configuration options.
  * request headers could be defined with `httpHeaders`. It could be 
  useful to override the `host` header.
  * it is possible to provide a body to the request through `requestBody`.
  The form of this body will often depend on the `Content-Type` header.
  For example, `{"foo":"bar"}` with `Content-Type: application/json`.
  * By default, it will follow redirects. It is possible to disable that behavior using 
  `noRedirects:false`.

See [Config Examples](#config-examples) for different request behaviors.

Some configuration options change the resulting values:
  * the `desiredCode` option will determine the `http.code_matched` value.
  By default it is `200` it could be useful to change it if you 
  expect different "normal" value.
  For example, use `desiredCode:301` and `noRedirects:false` to check a 
  redirect (and not the end redirected url) keeping value to `1` (success).
  * the `regex` option will do the same with `http.regex_matched` metric 
  where value will be `1` only if provided regex matchs the response body.
  * the `addRedirectURL` does not have impact on metrics but will add a 
  new dimension `redirect_url` with "dynamic" value. Indeed, if `url` 
  dimension could only change with monitor configuration, the `redirect_url` 
  could be impacted by any server change and will always be the last url 
  redirected. Disabled by default because this could cause problem with 
  heartbeat detector for example.

Common useful headers are:
  * `Cache-Control: no-cache` to ignore cache.
  * `Host` to change the request (i.e. bypass cdn or load balancer requesting 
  directly the backend)
  * `Content-Type` will allow to change application type (json, xml, octet-stream..)

<!--- SETUP --->
## Config Examples

Here are some `curl` examples commands with their corresponding configuration.

* `curl -L http://signalfx.com` (`http.status_code=200` because does follow 
redirect to splunk)

```
monitors:
 - type: http
   host: signalfx.com
```

* `curl -I http://signalfx.com` (`http.status_code=301` because it does not 
follow redirect to splunk)

```
monitors:
 - type: http
   host: signalfx.com
   noRedirects: true
   method: HEAD
```

* `curl -L -H 'Host: foobar' -A 'customAgent' https://signalfx.com` 
(`http.cert_valid=0` because host does not match certificate)

```
monitors:
 - type: http
   host: signalfx.com
   useHTTPS: true
   httpHeaders: 
     Host: foobar
     User-Agent: customAgent
```

* `curl -G -X GET -d 'foo=bar' -d 'leet=1337' http://signalfx.com/fakepage`

```
monitors:
 - type: http
   host: signalfx.com
   path: '/fakepage?foo=bar&leet=1337'
   method: GET
   httpHeaders:
     Content-Type: application/x-www-form-urlencoded
```

* `curl -X POST -d '{"foo":"bar"}' http://signalfx.com`

```
monitors:
 - type: http
   host: signalfx.com
   method: POST
   requestBody: '{"foo":"bar"}'
```

* `curl --resolve signalfx.com:443:127.0.0.1 https://signalfx.com`

```
monitors:
 - type: http
   host: 127.0.0.1
   port: 443
   useHTTPS: true
   sniServerName: signalfx.com
   httpHeaders:
     Host: signalfx.com
```

For a full list of options, see [Configuration](#configuration).


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: http
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | no | `string` | Host/IP to monitor |
| `port` | no | `integer` | Port of the HTTP server to monitor (**default:** `0`) |
| `path` | no | `string` | HTTP path to use in the test request |
| `httpTimeout` | no | `int64` | HTTP timeout duration for both read and writes. This should be a duration string that is accepted by https://golang.org/pkg/time/#ParseDuration (**default:** `10s`) |
| `username` | no | `string` | Basic Auth username to use on each request, if any. |
| `password` | no | `string` | Basic Auth password to use on each request, if any. |
| `useHTTPS` | no | `bool` | If true, the agent will connect to the server using HTTPS instead of plain HTTP. (**default:** `false`) |
| `httpHeaders` | no | `map of strings` | A map of HTTP header names to values. Comma separated multiple values for the same message-header is supported. |
| `skipVerify` | no | `bool` | If useHTTPS is true and this option is also true, the exporter's TLS cert will not be verified. (**default:** `false`) |
| `sniServerName` | no | `string` | If useHTTPS is true and skipVerify is true, the sniServerName is used to verify the hostname on the returned certificates. It is also included in the client's handshake to support virtual hosting unless it is an IP address. |
| `caCertPath` | no | `string` | Path to the CA cert that has signed the TLS cert, unnecessary if `skipVerify` is set to false. |
| `clientCertPath` | no | `string` | Path to the client TLS cert to use for TLS required connections |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use for TLS required connections |
| `requestBody` | no | `string` | Optional HTTP request body as string like '{"foo":"bar"}' |
| `noRedirects` | no | `bool` | Do not follow redirect. (**default:** `false`) |
| `method` | no | `string` | HTTP request method to use. (**default:** `GET`) |
| `urls` | no | `list of strings` | DEPRECATED: list of HTTP URLs to monitor. Use `host`/`port`/`useHTTPS`/`path` instead. |
| `regex` | no | `string` | Optional Regex to match on URL(s) response(s). |
| `desiredCode` | no | `integer` | Desired code to match for URL(s) response(s). (**default:** `200`) |
| `addRedirectURL` | no | `bool` | Add `redirect_url` dimension which could differ from `url` when redirection is followed. (**default:** `false`) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - ***`http.cert_expiry`*** (*gauge*)<br>    Certificate expiry in seconds. This metric is reported only if HTTPS  is available (from last followed URL).

 - ***`http.cert_valid`*** (*gauge*)<br>    Value is 1 if certificate is valid (including expiration test) or 0 else. This metric is reported only if HTTPS is available (from last followed URL).

 - ***`http.code_matched`*** (*gauge*)<br>    Value is 1 if `status_code` value match `desiredCode` set in config. Always reported.
 - ***`http.content_length`*** (*gauge*)<br>    HTTP response body length. Always reported.
 - ***`http.regex_matched`*** (*gauge*)<br>    Value is 1 if pattern match in response body. Only reported if `regex` is configured.
 - ***`http.response_time`*** (*gauge*)<br>    HTTP response time in seconds. Always reported.
 - ***`http.status_code`*** (*gauge*)<br>    HTTP response status code. Always reported.

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.

## Dimensions

The following dimensions may occur on metrics emitted by this monitor.  Some
dimensions may be specific to certain metrics.

| Name | Description |
| ---  | ---         |
| `method` | HTTP method used to do request. Not available on `http.cert_*` metrics. |
| `redirect_url` | Last URL retrieved (after redirects) from configured one. Only sent if `noRedirects: false` and `addLastURL: true` and if URL responds with a redirect different from the original `url`. |
| `url` | The normalized URL (including port and path) from the configuration of this monitor. Always available on every metrics. |



