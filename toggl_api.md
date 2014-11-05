Toggl API v8
====================

The API accepts only JSON requests. Please make sure you're setting `Content-Type: application/json`in your request header. Each request returns a **JSON-encoded** body.
If the time entry is currently running, the *duration* attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch.

The result of each action is communicated via standard HTTP response codes.

###CORS###

If you wish to use the API using CORS, we'll need to whitelist you first. Please send us a note at support@toggl.com to whitelist your domain(s).

###Successful requests###

When request is successful (2xx), a nested response object is returned. Fields which value is NULL are not in the response.

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"time_entry":{"description":"New time entry","created_with":"API example code","start":"2012-02-12T15:35:47+02:00","duration":1200,"wid":31366}}' \
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
	-H 'Content-Type: application/json' \
	-d '{"time_entry":{"description":"New time entry","created_with":"API example code"}}' \
	-X POST https://www.toggl.com/api/v8/time_entries
```

Response

`["Start can't be blank"]`


##Authentication##

To use the API, you need to authenticate yourself. This can be done via HTTP POST or HTTP Basic Auth. After successful authentication a session is created using a cookie.

If authentication fails, HTTP status code 403 is returned. You can read more about authentication and see sample requests [here](chapters/authentication.md).

##Supported API requests##

* [Authenticate and get user data](chapters/authentication.md)
 - HTTP Basic Auth with e-mail and password
 - HTTP Basic Auth with API token
 - Authentication with a session cookie
 - Destroy the session
* [Clients](chapters/clients.md)
 - create a client
 - get client details
 - update a client
 - delete a client
 - get clients visible to user
 - get client projects
* [Projects](chapters/projects.md)
 - create a project
 - get project data
 - update project data
 - get project users
 - get project tasks
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
 - start a time entry
 - stop a time entry
 - get time entry details
 - update time entry
 - delete time entry
 - get time entries started in a specific time range
 - bulk update time entries tags
* [Users](chapters/users.md)
 - get current user data and time entries
 - update current user data
 - reset API token
 - sign up new user
* [Workspaces](chapters/workspaces.md)
 - get user workspaces
 - get workspace users
 - get workspace clients
 - get workspace projects
 - get workspace tasks
 - get workspace tags
* [Workspace users](chapters/workspace_users.md)
 - invite users to workspace
 - update workspace user
 - delete workspace user
 - get workspace users for a workspace
* [Dashboard](chapters/dashboard.md)
 - Get a generic overview of your team

