Toggl API v8
====================

##Introduction##

The API accepts only JSON requests. Please make sure you're setting `Content-type: application/json`in your request header. Each request returns a **JSON-encoded** body.
If the time entry is currently running, the *duration* attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch.

The result of each action is communicated via standard HTTP response codes.

###Example requests###

The example requests here are done using a command line tool called [cURL](http://en.wikipedia.org/wiki/CURL). If you want to try the requests out yourself, you can download cURL from [here](http://curl.haxx.se/download.html). It is available for all possible operating systems.

Under Ubuntu installing cURL is terrifcally easy:

```shell
sudo apt-get install curl
```

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
 - HTTP Basic Auth with e-mail and password
 - HTTP Basic Auth with API token
 - Authentication with a session cookie
 - Destroy the session
* [Clients](chapters/clients.md)
 - create a client
 - update a client
 - delete a client
 - get clients visible to user
* [Projects](chapters/projects.md)
 - create a project
 - get project data
 - update project data
* [Project users](chapters/project_users.md)
 - create a project user
 - update a project user
 - delete a project user
 - add multiple users to a project
 - update multiple project users
 - delete multiple project users
* [Tags](chapters/tags.md)
 - create a tag
 - update a tag
 - delete a tag
* [Tasks](chapters/tasks.md) *(available only for pro workspaces)*
 - create a task
 - get task details
 - update a task
 - delete a task
 - update multiple tasks
 - delete multiple tasks
* [Time entries](chapters/time_entries.md)
 - create a time entry
 - get time entry details
 - update time entry
 - delete time entry
 - get time entries started in a specific time range
* [Users](chapters/users.md)
 - get current user data and time entries
* [Workspaces](chapters/workspaces.md)
 - get user workspaces
 - get workspace users
 - get workspace clients
 - get workspace projects
 - get workspace tasks


##Help us towards a better API##

The Toggl API has moved to Github so you could actively participate in helping us making the API better. If you have any requests or you found a bug, you can use Github issues to let us know. You can also fork the docs and send a pull request with improvements