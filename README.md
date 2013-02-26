Toggl API v8
====================

##Introduction##

**This API is still under development.**

The API accepts only JSON requests. Please make sure you're setting `Content-type: application/json`in your request header. Each request returns a **JSON-encoded** body.
If the time entry is currently running, the *duration* attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch.

The result of each action is communicated via standard HTTP response codes.


###Successful requests###

When request is successful (2xx), a nested response object is returned. Fields which value is NULL are not in the response.

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

###Failed requests###

If a create or update action failed, HTTP status code 404 and an array of localized error messages will be returned.

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H 'Content-type: application/json' \
	-d '{"time_entry":{"description":"New time entry","created_with":"API example code"}}' \
	-X POST https://www.toggl.com/api/v8/time_entries
```

Response

`["Start can't be blank"]`


##API token##

Each user in Toggl.com has an API token. They can find it under "My Profile" in their Toggl account.

##Authentication##

To use the API, you need to authenticate yourself. This can be done via HTTP POST or HTTP Basic Auth. After successful authentication a session is created using a cookie.

If authentication fails, HTTP status code 403 is returned. You can read more about authentication and see sample requests [here](chapters/authentication.md).

##Supported API requests##

* [Authentication](chapters/authentication.md)
* [Tags](chapters/tags.md)
* [Clients](chapters/clients.md)
* [Tasks](chapters/tasks.md) *available only for pro workspaces*

##Help us towards a better API##

The Toggl API has moved to Github so you could actively participate in helping us making the API better. If you have any requests or you found a bug, you can use Github issues to let us know. You can also fork the docs and send a pull request with improvements