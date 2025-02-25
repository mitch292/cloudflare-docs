---
_build:
  publishResources: false
  render: never
  list: never
inputParameters: productName
---

# Querying $1 tunnel bandwidth analytics with GraphQL

In this example, you are going to use the GraphQL Analytics API to query $1 ingress tunnel traffic over a specified time period.

The following API call will request $1 ingress tunnel traffic over a one-hour period and output the requested fields. Be sure to replace `<CLOUDFLARE_ACCOUNT_ID>` with your account ID, `<EMAIL>`, `<API_KEY>`, and `<API_TOKEN>` with your API credentials, and adjust the `datetime_geq` and `datetime_leq` values as needed.

The following example queries for ingress traffic. To query for egress, change the value in the direction filter.

## API Call

```bash
PAYLOAD='{ "query":
  "query GetTunnelHealthCheckResults($accountTag: string, $datetimeStart: string, $datetimeEnd: string) {
      viewer {
        accounts(filter: {accountTag: $accountTag}) {
          magicTransitTunnelTrafficAdaptiveGroups(
            limit: 100,
            filter: {
              datetime_geq: $datetimeStart,
              datetime_lt:  $datetimeEnd,
              direction: $direction
            }
          ) {
            avg {
              bitRateFiveMinutes
            }
            dimensions {
              tunnelName
              datetimeFiveMinutes
            }
          }
        }
      }
  }",
    "variables": {
      "accountTag": "<CLOUDFLARE_ACCOUNT_ID>",
      "direction": "ingress",
      "datetimeStart": "2022-05-04T11:00:00.000Z",
      "datetimeEnd": "2022-05-04T12:00:00.000Z"
    }
  }
}'

# curl with Legacy API Key
curl https://api.cloudflare.com/client/v4/graphql \
--header "X-Auth-Email: <EMAIL>" \
--header "X-Auth-Key: <API_KEY>" \
--header "Accept: application/json" \
--header "Content-Type: application/json" \
--data "$(echo $PAYLOAD)"

# curl with API Token
curl https://api.cloudflare.com/client/v4/graphql \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Accept: application/json" \
--header "Content-Type: application/json" \
--data "$(echo $PAYLOAD)"
```

The returned values represent the total bandwidth in bits/second during the five minute interval for a particular tunnel. To use aggregations other than five minutes, make sure that you use the same window for both your metric and date time. For example, to see hourly groups, use `bitRateHour` and `datetimeHour`.

The result will be in JSON (as requested), so piping the output to `jq` will make it easier to read, like in the following example:

```bash
curl https://api.cloudflare.com/client/v4/graphql \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Accept: application/json" \
--header "Content-Type: application/json" \
--data "$(echo $PAYLOAD)" | jq .

## Example response:
#=> {
#=>   "data": {
#=>     "viewer": {
#=>       "accounts": [
#=>         {
#=>           "magicTransitTunnelTrafficAdaptiveGroups": [
#=>             {
#=>               avg: { bitRateFiveMinutes:  327680 },
#=>               dimensions: {
#=>                 datetimeFiveMinute: '2021-05-12T22:00-00:00',
#=>                 tunnelName: 'tunnel_name'
#=>               }
#=>             },
#=>             {
#=>               avg: { bitRateFiveMinutes:  627213680 },
#=>               dimensions: {
#=>                 datetimeFiveMinute: '2021-05-12T22:05-00:00',
#=>                 tunnelName: 'another_tunnel'
#=>              }
#=>             }
#=>           ]
#=>         }
#=>       ]
#=>     }
#=>   },
#=>   "errors": null
#=> }
```