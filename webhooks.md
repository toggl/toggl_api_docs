Toggl Webhooks API v1
=====================

### Important

This document has been migrated to https://developers.track.toggl.com/docs/webhooks_start

----

Here you can get the general information about how to use Toggl Webhooks API: how to authenticate, create and list webhooks subscription, validate their URL endpoints and validate the received payloads.

## Authentication
<details>

You can authenticate to the Webhooks API **only** with your API token. For HTTP Basic Auth you have to add the Authorization header with the request.
The API token is the user name and the string `api_token` is the password.
Whenever possible, please use the tools and interfaces provided by your http library to do Basic Auth (for example, curl uses the -u switch to allow for it).
</details>

## Interacting with the API
<details>

**Notes**:
 * the Webhooks API base URL is `https://track.toggl.com/webhooks/api/v1`,
 * we provide example requests using [curl](https://curl.se/),
 * we parse request responses using [jq](https://stedolan.github.io/jq/),
 * in the examples you should replace `{<variable>}` with the corresponding value.

### Request Parameters

The API expects the following parameters:
* `workspace_id`: **Required**. The workspace ID for your webhooks subscription/s, to be provided in the URL path.
* `User-Agent`: **Suggested**. The name of your application or your email address so we can get in touch in case you're doing something wrong. To be provided as a HTTP header.


### List available subscriptions for a given workspace
```
curl -u {api_token}:api_token \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id} | jq .
```

### Create a subscription
```
curl -u {api_token}:api_token \
    -d '{
        "url_callback": "{url_callback}",
        "event_filters": [
            {"entity": "project", "action": "created"},
            {"entity": "tag", "action": "*"}
        ],
        "enabled": true,
        "description": "My first Webhooks subscription"
    }' \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id} | jq .
```
Response example:
```json
{
  "subscription_id": 1,
  "workspace_id": {workspace_id},
  "user_id": ...,
  "enabled": true,
  "description": "My first Webhooks subscription",
  "event_filters": [
    {
      "entity": "project",
      "action": "created"
    },
    {
      "entity": "tag",
      "action": "*"
    }
  ],
  "url_callback": {url_callback},
  "secret": ...,
  "validated_at": null,
  "has_pending_events": false,
  "created_at": "2022-05-31T02:50:45.984607Z"
}
```

**Notes**:
 * replace `{url_callback}` with the URL endpoint that will receive request for this webhooks subscription,
 * after creating the subscription you still need to [validate](#validating-a-url-endpoint) its `url_callback`.

#### About Event Filters
Event Filters allow you to reduce the scope of received events. They currently provide filtering capabilities by entity type and HTTP action applied over them.

Some possible event filters that you can build would be:
1. All events for created projects:
```js
    event_filters": [
        {"entity": "project", "action": "created"}
    ]
```
2. All events for tags:
```js
    event_filters": [
        {"entity": "tag", "action": "*"}
    ]
```
3. All events that occurred in a workspace:
```js
    event_filters": [
        {"entity": "*", "action": "*"}
    ]
```
4. All updated time entries and clients:
```js
    event_filters": [
        {"entity": "time_entry", "action": "updated"},
        {"entity": "client", "action": "updated"}
    ]
```
In any case, there would be authorization checks in place to also filter out the events that the creator of the subscription does not have permission to see.

You can get a list of supported entities and actions by using the endpoint `https://track.toggl.com/webhooks/api/v1/event_filters`
```
curl -u {api_token}:api_token \
  -H 'User-Agent: {user_agent}' \
  https://track.toggl.com/webhooks/api/v1/event_filters | jq .
```

### Update a subscription
```
curl -u {api_token}:api_token \
    -X PUT \
    -d '{
        "url_callback": "{url_callback}",
        "event_filters": [
            {"entity": "*", "action": "*"}
        ]
        "enabled": true,
        "secret": "new secret string",
        "description": "My first Webhooks subscription"
    }' \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id}/{subscription_id} | jq .
```

### Change enabled flag in a subscription
```
curl -u {api_token}:api_token \
    -X PATCH \
    -d '{"enabled": false}' \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id}/{subscription_id} | jq .
```

### Delete a subscription
```
curl -u {api_token}:api_token \
    -X DELETE \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id}/{subscription_id} | jq .
```

### Ping a subscription to test the setup
You will receive a dummy event in the registered URL endpoint for `{subscription_id}`.
```
curl -u {api_token}:api_token \
    -X POST \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/ping/{workspace_id}/{subscription_id} | jq .
```

### Get information about matching events for a subscription
We store events that we attempted to deliver to a subscription for a week, you can get information about them (creation date, number of failed delivery attempts, last delivery error, ...) by using the special endpoint `/subscriptions/{workspace_id}/{subscription_id}/events`.

```
curl -u {api_token}:api_token \
    -H 'User-Agent: {user_agent}' \
    https://track.toggl.com/webhooks/api/v1/subscriptions/{workspace_id}/{subscription_id}/events` | jq .
```
Response example:
```json
{
  "total": 2,
  "events": [
    {
      "event_id": ...,
      "created_at": "2022-05-31T02:56:52.493Z",
      "creator_id": ...,
      "metadata": {
        "path": "/api/v9/workspaces/{workspace_id}/projects",
        "model": "project",
        "action": "created",
        "project_id": ...,
        "request_type": "POST",
        "workspace_id": "{workspace_id}",
        "event_user_id": ...
      },
      "payload": {
        ...
      },
      "consumer_id": ...,
      "last_delivery_attempt": "2022-05-31T02:56:54.205984Z",
      "last_delivery_error": null,
      "failed_delivery_attempts": 3
    },
  ...
  ]
}
```
</details>

## Validating a URL endpoint

<details>
For a subscription to receive events it has to be both enabled and validated.
The objective of the validation is for the end user to prove that they have access to the events we will send to the URL endpoint. Otherwise, a malicious user could provide us with a valid URL endpoint, that accepts POST requests and returns the expected 2xx status to our requests but that is not associated with the user who created the subscription. In such a scenario we would be sending unsolicited requests to this URL, potentially creating a denial of service.
The validation part can take any of two forms: one synchronous and one asynchronous.

#### Synchronous Validation
In our special PING events the system will include the `validation_code` field for non-validated subscriptions and we will read the response body trying to parse this structure:
```json
    {"validation_code": "<validation_code>"}
```
Where `<validation_code>` must be the code we included in our PING event.

#### Asynchronous Validation
Requiring users to implement this behavior in their endpoints could be too much of a burden or they could not even be able to accomplish it if the endpoint service is provided by a third party. In such scenarios we also provide an alternative asynchronous validation scheme where end users need to perform a GET request to `/webhooks/api/v1/validate/{workspace_id}/{subscription_id}/{validation_code}`. If everything is fine, then an OK response will be returned and the subscription will be set as validated.
</details>

## Validating received events

<details>

In order to be sure that Toggl is the one sending events to your URL endpoint we include a special HTTP header `X-Webhook-Signature-256` that you can use to validate that no one else is sending those requests.

In order to perform this validation you should:
- Set the field `secret` while creating a new subscription. If omitted, the system will assign one automatically.
- When delivering events to your subscription's URL endpoint the system will add the `X-Webhook-Signature-256` header.
- Signature has the form of `sha256={value}` where value is a HMAC hash based on SHA256 algorithm + secret + body.
- Example: `sha256=6d011bcd0b5bfb7e45372af01bc18f30cc04599df72eca189cdac1094008b095`.
</details>


## Glossary

<details>

<summary>Common used terms in this document</summary>


* **Event Filter**: allows you to specify which events a given subscription should receive in its configured URL callback. A subscription may have multiple event filters, but must have at least one.
* **Subscription**: a Webhooks subscription is an entity tied to a single workspace and user creator that has a description, a secret, a set of event filters, a url callback and a enabled and validation status. A user may have multiple subscriptions and a workspace may have multiple subscriptions from different users, but there's a limit of subscriptions per user and workspace.
* **URL callback**: is the URL destination endpoint for a subscription. The system will validate that it's both reachable and returns a 2xx status before starting to forward events to it.
* **User Creator**: given a subscription, the user creator is the one who set it up. Any event sent to the configured url callback will be validated using the permissions of this user. If the user creator doesn't have permission to see some entity, then events related to that entity won't be forwarded to the URL callback. A workspace administrator may list and edit subscriptions from other users in that workspace.
* **Validation Status**: each created subscription must be validated before it can start receiving events. This validation step is to ensure that the creator has access to the provided URL callback. Otherwise, it would mean that Toggl is sending unsolicited events to the configured URL. A non-validated and not enabled subscription may still receive the special PING event which is useful to test that you can receive events from Toggl.

</details>
