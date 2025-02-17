uuid: 0ba58f32-7dba-4084-ab17-90c0be6b1f10
name: Cloudflare HTTP requests
type: intake

## Overview

Cloudflare is a global network designed to make everything you connect to the Internet secure, private, fast, and reliable.

In this documentation, you will learn how to collect and send Cloudflare HTTP requests to SEKOIA.IO.

{!_shared_content/operations_center/integrations/generated/cloudflare-http-requests_do_not_edit_manually.md!}

## Configure

### Create the intake on SEKOIA.IO

Go to the [intake page](https://app.sekoia.io/operations/intakes) and create a new intake from the format Cloudflare.

### Configure events forwarding on Cloudflare

{!_shared_content/operations_center/integrations/cloudflare_necessary_info.md!}

#### Configure Logpush

##### Retrieve all available fields with your subscription

Depending on your subscription, you first need to get all the fields that you can forward to SEKOIA.IO by using the Cloudflare API [Retrieve all available fields for a dataset](https://developers.cloudflare.com/logs/get-started/api-configuration/){:target="_blank"}.

With cURL you can use this command:
```bash
curl -s -H "X-Auth-Email: <Cloudflare Email Address>" \
-H "X-Auth-Key: <Cloudflare API Token>" \
"https://api.cloudflare.com/client/v4/zones/<Cloudflare Zone ID>/logpush/datasets/http_requests/fields" \
| jq '.result' # (1)
```

1. will return
```json
{
  "BotScore": "int; Cloudflare Bot Score. Scores below 30 are commonly associated with automated traffic. Available for Bot Management customers (please contact your account team to enable).",
  "BotScoreSrc": "string; detection engine responsible for generating the Bot Score. Possible values are Not Computed | Heuristics | Machine Learning | Behavioral Analysis | Verified Bot | JS Fingerprinting | Cloudflare Service",
  "BotTags": "array[string]; type of bot traffic (if available). Refer to [Bot Tags](https://developers.cloudflare.com/bots/concepts/cloudflare-bot-tags/) for the list of potential values. Available in Logpush v2 only.",
  "CacheCacheStatus": "string; cache status. Possible values are unknown | miss | expired | updating | stale | hit | ignored | bypass | revalidated | dynamic | stream_hit | deferred \"dynamic\" means that a request is not eligible for cache. This can mean, for example that it was blocked by the firewall. Refer to [Cloudflare cache responses](https://developers.cloudflare.com/cache/about/default-cache-behavior/#cloudflare-cache-responses) for more details.",
  "CacheReserveUsed": "bool; Cache Reserve was used to serve this request. Available in Logpush v2 only.",
  "CacheResponseBytes": "int; number of bytes returned by the cache",
  "CacheResponseStatus": "int; HTTP status code returned by the cache to the edge. All requests (including non-cacheable ones) go through the cache. Refer also to CacheCacheStatus field.",
  "CacheTieredFill": "bool; tiered Cache was used to serve this request",
  "ClientASN": "int; client AS number",
  "ClientCountry": "string; country of the client IP address",
  "ClientDeviceType": "string; client device type",
  "ClientIP": "string; IP address of the client",
  "ClientIPClass": "string; unknown | badHost | searchEngine | allowlist | monitoringService | noRecord | scan | tor",
  "ClientMTLSAuthCertFingerprint": "string; the SHA256 fingerprint of the certificate presented by the client during mTLS authentication. Only populated on the first request on an mTLS connection. Available in Logpush v2 only.",
  "ClientMTLSAuthStatus": "string; the status of mTLS authentication. Only populated on the first request on an mTLS connection. Available in Logpush v2 only. Possible values are unknown | ok | absent | untrusted | notyetvalid | expired",
  "ClientRequestBytes": "int; number of bytes in the client request",
  "ClientRequestHost": "string; host requested by the client",
  "ClientRequestMethod": "string; HTTP method of client request",
  "ClientRequestPath": "string; URI path requested by the client",
  "ClientRequestProtocol": "string; HTTP protocol of client request",
  "ClientRequestReferer": "string; HTTP request referrer",
  "ClientRequestScheme": "string; the URL scheme requested by the visitor. Available in Logpush v2 only.",
  "ClientRequestSource": "string; identifies requests as coming from an external source or another service within Cloudflare. Refer to [ClientRequestSource field](https://developers.cloudflare.com/logs/reference/clientrequestsource/) for the list of potential values. Available in Logpush v2 only.",
  "ClientRequestURI": "string; URI requested by the client",
  "ClientRequestUserAgent": "string; user agent reported by the client",
  "ClientSSLCipher": "string; client SSL cipher",
  "ClientSSLProtocol": "string; client SSL (TLS) protocol. The value \"none\" means that SSL was not used.",
  "ClientSrcPort": "int; client source port",
  "ClientTCPRTTMs": "int; the smoothed average of TCP round-trip time (SRTT). For the initial request on a connection, this is measured only during connection setup. For a subsequent request on the same connection, it is measured over the entire connection lifetime up until the time that request is received. Available in Logpush v2 only.",
  "ClientXRequestedWith": "string; X-Requested-With HTTP header",
  "Cookies": "object; string key-value pairs for Cookies",
  "EdgeCFConnectingO2O": "bool; true if the request looped through multiple zones on the Cloudflare edge. This is considered an orange to orange (o2o) request. Available in Logpush v2 only.",
  "EdgeColoCode": "string; IATA airport code of data center that received the request",
  "EdgeColoID": "int; Cloudflare edge colo id",
  "EdgeEndTimestamp": "int or string; timestamp at which the edge finished sending response to the client",
  "EdgePathingOp": "string; indicates what type of response was issued for this request (unknown = no specific action)",
  "EdgePathingSrc": "string; details how the request was classified based on security checks (unknown = no specific classification)",
  "EdgePathingStatus": "string; indicates what data was used to determine the handling of this request (unknown = no data)",
  "EdgeRateLimitAction": "string; the action taken by the blocking rule; empty if no action taken. Possible values are unknown | simulate | ban | challenge | jsChallenge",
  "EdgeRateLimitID": "int; the internal rule ID of the rate-limiting rule that triggered a block (ban) or log action. 0 if no action taken.",
  "EdgeRequestHost": "string; host header on the request from the edge to the origin",
  "EdgeResponseBodyBytes": "int; size of the HTTP response body returned to clients. Available in Logpush v2 only.",
  "EdgeResponseBytes": "int; number of bytes returned by the edge to the client",
  "EdgeResponseCompressionRatio": "float; edge response compression ratio",
  "EdgeResponseContentType": "string; edge response Content-Type header value",
  "EdgeResponseStatus": "int; HTTP status code returned by Cloudflare to the client",
  "EdgeServerIP": "string; IP of the edge server making a request to the origin. Possible responses are string in IPv4 or IPv6 format, or empty string. Empty string means that there was no request made to the origin server.",
  "EdgeStartTimestamp": "int or string; timestamp at which the edge received request from the client",
  "EdgeTimeToFirstByteMs": "int; total view of Time To First Byte as measured at Cloudflare's edge. Starts after a TCP connection is established and ends when Cloudflare begins returning the first byte of a response to eyeballs. Includes TLS handshake time (for new connections) and origin response time. Available in Logpush v2 only.",
  "FirewallMatchesActions": "array[string]; array of actions the Cloudflare firewall products performed on this request. The individual firewall products associated with this action be found in FirewallMatchesSources and their respective RuleIds can be found in FirewallMatchesRuleIDs. The length of the array is the same as FirewallMatchesRuleIDs and FirewallMatchesSources. Possible actions are unknown | allow | block | challenge | jschallenge | log | connectionClose | challengeSolved | challengeFailed | challengeBypassed | jschallengeSolved | jschallengeFailed | jschallengeBypassed | bypass | managedChallenge | managedChallengeSkipped | managedChallengeNonInteractiveSolved | managedChallengeInteractiveSolved | managedChallengeBypassed",
  "FirewallMatchesRuleIDs": "array[string]; array of RuleIDs of the firewall product that has matched the request. The firewall product associated with the RuleID can be found in FirewallMatchesSources. The length of the array is the same as FirewallMatchesActions and FirewallMatchesSources.",
  "FirewallMatchesSources": "array[string]; the firewall products that matched the request. The same product can appear multiple times, which indicates different rules or actions that were activated. The RuleIDs can be found in FirewallMatchesRuleIDs, the actions can be found in FirewallMatchesActions. The length of the array is the same as FirewallMatchesRuleIDs and FirewallMatchesActions. Validation matches only appear in Logpush and are not supported in Logpull. Possible sources are unknown | asn | country | ip | ipRange | securityLevel | zoneLockdown | waf | firewallRules | uaBlock | rateLimit | bic | hot | l7ddos | validation | botFight | apiShield | botManagement | dlp | firewallManaged | firewallCustom",
  "JA3Hash": "string; the MD5 hash of the JA3 fingerprint used to profile SSL/TLS clients. Available in Logpush v2 only.",
  "OriginDNSResponseTimeMs": "int; time taken to receive a DNS response for an origin name. Usually takes a few milliseconds, but may be longer if a CNAME record is used. Available in Logpush v2 only.",
  "OriginIP": "string; IP of the origin server",
  "OriginRequestHeaderSendDurationMs": "int; time taken to send request headers to origin after establishing a connection. Note that this value is usually 0. Available in Logpush v2 only.",
  "OriginResponseBytes": "int; number of bytes returned by the origin server",
  "OriginResponseDurationMs": "int; upstream response time, measured from the first datacenter that receives a request. Includes time taken by Argo Smart Routing and Tiered Cache, plus time to connect and receive a response from origin servers. This field replaces OriginResponseTime. Available in Logpush v2 only.",
  "OriginResponseHTTPExpires": "string; value of the origin 'expires' header in RFC1123 format",
  "OriginResponseHTTPLastModified": "string; value of the origin 'last-modified' header in RFC1123 format",
  "OriginResponseHeaderReceiveDurationMs": "int; time taken for origin to return response headers after Cloudflare finishes sending request headers. Available in Logpush v2 only.",
  "OriginResponseStatus": "int; status returned by the origin server. The value 0 means that there was no request made to the origin server and the response was served by Cloudflare's Edge.",
  "OriginResponseTime": "int; number of nanoseconds it took the origin to return the response to edge",
  "OriginSSLProtocol": "string; SSL (TLS) protocol used to connect to the origin",
  "OriginTCPHandshakeDurationMs": "int; time taken to complete TCP handshake with origin. This will be 0 if an origin connection is reused. Available in Logpush v2 only.",
  "OriginTLSHandshakeDurationMs": "int; time taken to complete TLS handshake with origin. This will be 0 if an origin connection is reused. Available in Logpush v2 only.",
  "ParentRayID": "string; Ray ID of the parent request if this request was made using a Worker script",
  "RayID": "string; ID of the request",
  "RequestHeaders": "object; string key-value pairs for RequestHeaders",
  "ResponseHeaders": "object; string key-value pairs for ResponseHeaders",
  "SecurityLevel": "string; the security level configured at the time of this request. This is used to determine the sensitivity of the IP Reputation system.",
  "SmartRouteColoID": "int; the Cloudflare datacenter used to connect to the origin server if Argo Smart Routing is used. Available in Logpush v2 only.",
  "UpperTierColoID": "int; the \"upper tier\" datacenter that was checked for a cached copy if Tiered Cache is used. Available in Logpush v2 only.",
  "WAFAction": "string; action taken by the WAF, if triggered",
  "WAFFlags": "string; additional configuration flags: simulate (0x1) | null",
  "WAFMatchedVar": "string; the full name of the most-recently matched variable",
  "WAFProfile": "string; low | med | high",
  "WAFRuleID": "string; ID of the applied WAF rule",
  "WAFRuleMessage": "string; rule message associated with the triggered rule",
  "WorkerCPUTime": "int; amount of time in microseconds spent executing a worker, if any",
  "WorkerStatus": "string; status returned from worker daemon",
  "WorkerSubrequest": "bool; whether or not this request was a worker subrequest",
  "WorkerSubrequestCount": "int; number of subrequests issued by a worker when handling this request",
  "WorkerWallTimeUs": "int; real-time in microseconds elapsed between start and end of worker invocation",
  "ZoneID": "int; internal zone ID",
  "ZoneName": "string; the human-readable name of the zone (e.g. 'cloudflare.com'). Available in Logpush v2 only."
}
```

Save the fields you want to send to SEKOIA.IO. It will be used to [configure the Logpush job](#configure-logpush). For instance :
```bash
ClientIP,ClientRequestHost,ClientRequestMethod,ClientRequestURI,EdgeEndTimestamp,EdgeResponseBytes,EdgeResponseStatus,EdgeStartTimestamp,RayID
```

##### Create a Logpush job

Configure a [Logpush job](https://developers.cloudflare.com/logs/reference/logpush-api-configuration/){:target="_blank"} with the following destination:

`https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>`

To do so, you can manage Logpush with cURL:

```bash
$ curl -X POST https://api.cloudflare.com/client/v4/zones/<Cloudflare Zone ID>/logpush/jobs \
-h "x-auth-email: <Cloudflare Email Address>" \
-H "X-Auth-Key: <Cloudflare API Token>" \
-H "Content-Type: application/json" \
--data '{
    "dataset": "http_requests",
    "enabled": true,
    "max_upload_bytes": 5000000,
    "max_upload_records": 1000,
    "logpull_options":"fields=<LIST_OF_FIELDS>&timestamps=rfc3339",
    "destination_conf": "https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>"
}' # (1)
```

1. will return
```json
{
  "errors": [],
  "messages": [],
  "result": {
    "id": 146,
    "dataset": "http_requests",
    "enabled": false,
    "name": "<DOMAIN_NAME>",
    "logpull_options": "fields=<LIST_OF_FIELDS>&timestamps=rfc3339",
    "destination_conf": "https://intake.sekoia.io/plain/batch?header_X-SEKOIAIO-INTAKE-KEY=<YOUR_INTAKE_KEY>",
    "last_complete": null,
    "last_error": null,
    "error_message": null
  },
  "success": true
}
```

!!! Important
    Replace :

    - `<YOUR_INTAKE_KEY>` with the Intake key you generated in the [Create the intake on SEKOIA.IO](#create-the-intake-on-sekoiaio) step.
    - `<LIST_OF_FIELDS>` with the fields you identified in the [Retrieve all available fields with your subscription](#retrieve-all-available-fields-with-your-subscription) section (for instance `ClientIP,ClientRequestHost,ClientRequestMethod,ClientRequestURI,EdgeEndTimestamp,EdgeResponseBytes,EdgeResponseStatus,EdgeStartTimestamp,RayID`).

{!_shared_content/operations_center/integrations/cloudflare_useful_api_endpoints.md!}