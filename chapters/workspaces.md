Workspaces
====================

##Get workspaces##
Workspace has the following properties:
* name: (string, required)
* premium: If it's a pro workspace or not. Shows if someone is paying for the workspace or not (boolean, not required)
* at: timestamp that is sent in the response, indicates the time item was last updated

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
		"at":"2012-12-03T13:06:06+00:00"
	},{
		"id":777,
		"name":"My Company Inc",
		"premium":true,
		"at":"2012-09-03T11:30:32+00:00"
	}
]
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
