Toggl API v8
====================
Introduction
----------------
The API accepts only JSON requests. Please make sure you're setting `Content-type: application/json`in your request header. Each request returns a JSON-encoded body.
If the time entry is currently running, the *duration* attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch.

The result of each action is communicated via standard HTTP response codes.

Successful requests
----------------

When request is successful (2xx), a nested response object is returned.

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H 'Content-type: application/json' \
	-d '{"time_entry":{"description":"New time entry","created_with":"API example code","start":"2012-02-12T15:35:47+02:00","wid":{"id":31366}}}' \
	 -X POST https://www.toggl.com/api/v8/time_entries

```
Response

```json
{
    "data": {
        "description": "New time entry",
        "start": "2013-02-12T15:35:47+02:00",
        "billable": false,
        "wid": 31366,
        "pid": 9012,
        "duration": 1200,
        "stop": "2013-02-12T15:35:57+02:00",
        "tags": [
         	"billed"
        ],
        "id": 4269795
    }
}
```