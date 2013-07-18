Users
=================

User has the following properties
* api_token: (string)
* default_wid: default workspace id (integer)
* email: (string)
* jquery_timeofday_format: (string)
* jquery_date_format:(string)
* timeofday_format: (string)
* date_format: (string)
* store_start_and_stop_time: whether start and stop time are saved on time entry (boolean)
* beginning_of_week: (integer, Sunday=0)
* language: user's language (string)
* image_url: url with the user's profile picture(string)
* sidebar_piechart: should a piechart be shown on the sidebar (boolean)
* at: timestamp of last changes
* new_blog_post: an object with toggl blog post title and link

## Get current user data ##
`GET https://www.toggl.com/api/v8/me`

By default the request responds with user properties.
To get all the workspaces, clients, projects, tasks, time entries and tags which the user can see, add the parameter `with_related_data=true`
If you want to retrieve objects which have changed after certain time, add `since` parameter to the query. The value should be a unix timestamp (e.g. `since=1362579886`)

Example request *without* related data

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET https://www.toggl.com/api/v8/me
```

Successful response
```json
{
	"since":1362575771,
	"data": {
		"id":9000,
		"api_token":"1971800d4d82861d8f2c1651fea4d212",
		"default_wid":777,
		"email":"johnt@swift.com",
		"fullname":"John Swift",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":0,
		"language":"en_US",
		"image_url":"https://www.toggl.com/system/avatars/9000/small/open-uri20121116-2767-b1qr8l.png",
		"sidebar_piechart":false,
		"at":"2013-03-06T12:18:42+00:00",
		"retention":9,
		"record_timeline":true,
		"render_timeline":true,
		"timeline_enabled":true,
		"timeline_experiment":true,
		"new_blog_post":{},
		"invitation":{}
	}
}
```

Example request with all the connected data

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET https://www.toggl.com/api/v8/me?with_related_data=true
```

Successful response
```json
{
	"since":1362575771,
	"data": {
		"id":9000,
		"api_token":"1971800d4d82861d8f2c1651fea4d212",
		"default_wid":777,
		"email":"johnt@swift.com",
		"fullname":"John Swift",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":0,
		"language":"en_US",
		"image_url":"https://www.toggl.com/system/avatars/9000/small/open-uri20121116-2767-b1qr8l.png",
		"sidebar_piechart":false,
		"at":"2013-03-06T12:18:42+00:00",
		"retention":9,
		"record_timeline":true,
		"render_timeline":true,
		"timeline_enabled":true,
		"timeline_experiment":true,
		"new_blog_post":
		{
			"title":"Increasing perceived performance with _.throttle()",
			"url":"http://blog.toggl.com/2013/02/increasing-perceived-performance-with-_throttle/?utm_source=rss&utm_medium=rss&utm_campaign=increasing-perceived-performance-with-_throttle"
		},
		"time_entries":[
			{
				"id":435559433,
				"wid":777,
				"billable":false,
				"start":"2013-03-06T10:08:23+00:00",
				"stop":"2013-03-06T14:08:23+00:00",
				"duration":14400,
				"description":"Best work so far",
				"tags":[""],
				"at":"2013-03-06T14:08:23+00:00"
			}
		],
		"projects":[
			{
				"id":1230994,
				"wid":777,
				"name":"Important project",
				"billable":false,
				"active":false,
				"at":"2013-03-06T09:13:31+00:00"
			}
		],
		"tags":[
			{
				"id":159637,
				"wid":188309,
				"name":"billable",
				"at":"2013-02-21T14:57:46+00:00"
			},{
				"id":159654,
				"wid":188309,
				"name":"important",
				"at":"2013-02-22T14:06:17+00:00"
			}
		],
		"workspaces":[
			{
				"id":777,
				"name":
				"John's WS",
				"premium":true,
				"at":"2013-03-06T09:00:30+00:00"
			}
		],
		"clients":[
			{
				"id":923476,
				"wid":777,
				"name":"Best client",
				"at":"2013-03-06T09:00:30+00:00"
			}
		]

	}
}
```

##Get workspace users##

Retrieving workspace users is documented [here](workspaces.md#get-workspace-users).

##Sign up new user##

To create a user you must provide two parameters for the user:
* email: a valid email for the user whose account is created (string, required)
* password: password at least 6 characters long (string, required)

Additional fields, that are not mandatory yet, are:
* tzoffset: timezone offset from UTC, in seconds. If it's blank, UTC is used as user timezone (int, not required)
* created_with: in free form, name of the app that signed the user app (string, not required)

`POST https://www.toggl.com/api/v8/signups`

Example request
```shell
curl -H "Content-type: application/json" \
-d '{"user":{"email":"test.user@toggl.com","password":"StrongPassword"}}' \
-X POST https://www.toggl.com/api/v8/signups
```

Successful response includes created user's data and API token
```json
{
	"data":{
		"id":599978901,
		"api_token":"808lolae4eab897cce9729a53642124effe",
		"default_wid":983493,
		"email":"test.user@toggl.com",
		"fullname":"Test User",
		"jquery_timeofday_format":"",
		"jquery_date_format":"",
		"timeofday_format":"",
		"date_format":"",
		"store_start_and_stop_time":false,
		"beginning_of_week":0,
		"sidebar_piechart":false,
		"timeline_experiment":false,
		"new_blog_post":{},
		"invitation":{}
	}
}
```
