Workspaces
====================

Workspace has the following properties
* name: the name of the workspace (string)
* premium: If it's a pro workspace or not. Shows if someone is paying for the workspace or not (boolean)
* admin: shows whether currently requesting user has admin access to the workspace (boolean)
* default_hourly_rate: default hourly rate for workspace, won't be shown to non-admins if the only_admins_see_billable_rates flag is set to true (float)
* default_currency: default currency for workspace (string)
* only_admins_may_create_projects: whether only the admins can create projects or everybody (boolean)
* only_admins_see_billable_rates: whether only the admins can see billable rates or everybody (boolean)
* rounding: type of rounding (integer)
* rounding_minutes: round up to nearest minute (integer)
* at: timestamp that indicates the time workspace was last updated
* logo_url: URL pointing to the logo (if set, otherwise omited) (string)


##Get workspaces##

`GET https://www.toggl.com/api/v8/workspaces`
Get data about all the workspaces where the token owner belongs to.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces
```

Successful response is an array of workspaces
```json
[
	{
		"id":3134975,
		"name":"John's personal ws",
		"premium":true,
		"admin":true,
		"default_hourly_rate":50,
		"default_currency":"USD",
		"only_admins_may_create_projects":false,
		"only_admins_see_billable_rates":true,
		"rounding":1,
		"rounding_minutes":15,
		"at":"2013-08-28T16:22:21+00:00",
		"logo_url":"my_logo.png"
	},{
		"id":777,
		"name":"My Company Inc",
		"premium":true,
		"admin":true,
		"default_hourly_rate":40,
		"default_currency":"EUR",
		"only_admins_may_create_projects":false,
		"only_admins_see_billable_rates":true,
		"rounding":1,
		"rounding_minutes":15,
		"at":"2013-08-28T16:22:21+00:00"
	}
]
```

##Get single workspace##
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/3134975
```

Successful response
```json
{
	"data":	{
		"id":3134975,
		"name":"John's personal ws",
		"premium":true,
		"admin":true,
		"default_hourly_rate":150,
		"default_currency":"USD",
		"only_admins_may_create_projects":false,
		"only_admins_see_billable_rates":false,
		"rounding":1,
		"rounding_minutes":15,
		"at":"2013-08-28T16:22:21+03:00",
		"logo_url":"my_logo.png"
	}
}
```

##Update workspace##

`PUT https://www.toggl.com/api/v8/workspaces/{workspace_id}`

Example request


```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"workspace":{"default_currency": "EUR", "default_hourly_rate": 50, "name": "John's ws", "only_admins_may_create_projects": false, "only_admins_see_billable_rates": true, "rounding": 1, "rounding_minutes": 60}}' \
	-X PUT https://www.toggl.com/api/v8/workspaces/3134975
```


Successful response
```json
{
	"data": {
		"id":3134975,
		"name":"John's ws",
		"premium":true,
		"admin":true,
		"default_hourly_rate":50,
		"default_currency":"USD",
		"only_admins_may_create_projects":false,
		"only_admins_see_billable_rates":true,
		"rounding":1,
		"rounding_minutes":60,
		"at":"2013-08-28T16:22:21+03:00",
		"logo_url":"my_logo.png"
	}
}
```

##Get workspace users##

To get a successful response, the token owner must be workspace admin.
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/users`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/users
```

Successful response is an array of workspace users
```json
[
	{
		"id":123123,
		"default_wid":777,
		"email":"john@swift.com",
		"fullname":"John Swift",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":0,
		"language":"en_US",
		"image_url":"https://www.toggl.com/system/avatars/123123/small/open-uri20121116-2767-b1qr8l.png",
		"sidebar_piechart":false,
		"at":"2013-03-06T08:57:12+00:00",
		"retention":9,
		"record_timeline":true,
		"render_timeline":true,
		"timeline_enabled":true,
		"timeline_experiment":true,
		"new_blog_post":{},
		"should_upgrade":true,
		"invitation":{}
	},{
		"id":321321,
		"email":"Happy@worker.com",
		"fullname":"Happy Worker",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":1,
		"language":"en_US",
		"image_url":"https://www.toggl.com/images/profile.png",
		"sidebar_piechart":false,
		"at":"2013-03-06T08:46:07+00:00",
		"created_at":"2013-03-06T07:52:03+00:00",
		"retention":9,
		"render_timeline":true,
		"timeline_experiment":true,
		"new_blog_post":{},
		"should_upgrade":true,
		"invitation":{}
	}
]
```

##Get workspace clients##

To get a successful response, the token owner must be workspace admin.
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/clients`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/clients
```

Successful response is an array of workspace clients
```json
[
	{
		"id":123,
		"wid":777,
		"name":"Rising Start-Up",
		"at":"2013-03-06T09:06:13+00:00",
		"notes":"Arrange a discount for them",
		"hrate":2,
		"cur":"USD"
	},{
		"id":987,
		"wid":777,
		"name":"Big Company Inc",
		"at":"2013-03-06T09:05:40+00:00",
		"notes":"We had some lovely projects with them",
		"hrate":10,
		"cur":"EUR"
	}
]
```

##Get workspace projects##

To get a successful response, the token owner must be workspace admin.
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/projects`

To filter projects by their state you can add the additional param to the request url:
* active: possible values `true`/`false`/`both`. By default true. If false, only archived projects are returned.

To get the completed hours per project you can add the additional param to the request url:
* actual_hours: `true`. By default false.

To get only project templates add the additional param to the request url:
* only_templates: `true`. By default false.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/projects
```

Successful response is an array of active workspace projects
```json
[
	{
		"id":909,
		"wid":777,
		"cid":987,
		"name":"Very lucrative project",
		"billable":false,
		"is_private":true,
		"active":true,
		"at":"2013-03-06T09:15:18+00:00"
	},{
		"id":32123,
		"wid":777,
		"cid":123,
		"name":"Factory server infrastructure",
		"billable":true,
		"is_private":true,
		"active":true,
		"at":"2013-03-06T09:16:06+00:00"
	}
]
```

##Get workspace tasks##

Available only for pro workspaces
To get a successful response, the token owner must be workspace admin.
Get all not done tasks in this workspace.
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/tasks`

To filter tasks by their state you can add the additional param to the request url:
* active: possible values `true`/`false`/`both`. By default true. If false, only done tasks are returned.


Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/tasks
```

Successful response is an array of workspace tasks
```json
[
	{
		"name":"SWOT",
		"id":13512097,
		"wid":777,
		"pid":32123,
		"uid":123123,
		"active":true,
		"at":"2013-03-06T09:15:51+00:00",
		"estimated_seconds":7200
	},{
		"name":"development",
		"id":133504498,
		"wid":777,
		"pid":32123,
		"active":true,
		"at":"2013-03-06T09:15:59+00:00",
		"estimated_seconds":0
	},{
		"name":"analyze SEO",
		"id":1335112300,
		"wid":777,
		"pid":909,
		"active":true,
		"at":"2013-03-06T09:18:57+00:00",
		"estimated_seconds":21600
	}

]
```

##Get workspace tags##

`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/tags`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/tags
```

Successful response is an array of active workspace tags
```json
[
	{
		"id":151285,
		"wid":777,
		"name":"Billed"
	},{
		"id":1596623,
		"wid":777,
		"name":"Invoiced"
	},{
		"id":159643,
		"wid":777,
		"name":"Discarded"
	}
]
```
