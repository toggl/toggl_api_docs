Project users
====================
Project user has the following properties
* pid: project ID (integer, required)
* uid: user ID, who is added to the project (integer, required)
* wid: workspace ID, where the project belongs to (integer, not-required, project's workspace id is used)
* manager: admin rights for this project (boolean, default false)
* rate: hourly rate for the project user (float, not-required, only for pro workspaces) in the currency of the project's client or in workspace default currency.
* at: timestamp that is sent in the response, indicates when the project user was last updated

Workspace id (wid), project id (pid) and user id (uid) can't be changed on update.


###Additional fields###
It's possible to get user's fullname. For that you have to send the `fields` parameter in request with desired property name.

* fullname: full name of the user, who is added to the project


##Actions for single project user##

###Create a project user###

`POST https://www.toggl.com/api/v8/project_users`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"project_user":{"pid":777,"uid":123,"rate":4.0,"manager":true}}' \
	-X POST https://www.toggl.com/api/v8/project_users

```

Successful response
```json
{
	"data": {
		"id":4692190,
		"pid":777,
		"uid":123,
		"wid":99,
		"manager":true,
		"rate":4
	}
}

```


###Update a project user###

`PUT https://www.toggl.com/api/v8/project_users/{project_user_id}`

Workspace id (wid), project id (pid) and user id (uid) can't be changed.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"project_user":{"manager":false,"rate":15,"fields":"fullname"}}' \
	-X PUT https://www.toggl.com/api/v8/project_users/4692190
```

Successful response
```json
{
	"data": {
		"id":4692190,
		"pid":777,
		"uid":123,
		"wid":99,
		"manager":false,
		"rate":15,
		"fullname":"John Swift",
		"at":"2013-03-05T09:21:44+00:00"
	}
}
```

###Delete a project user###

`DELETE https://www.toggl.com/api/v8/project_users/{project_user_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/project_users/4692190
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`


##Mass Actions##

###Create multiple project users for single project###
To create multiple project users for a single project, you must add multiple user ids separated with a comma with the `uid` parameter.

`POST https://www.toggl.com/api/v8/project_users`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"project_user":{"pid":777,"uid":"1267998,29624,112047","rate":4.0,"manager":true,"fields":"fullname"}}' \
	-X POST https://www.toggl.com/api/v8/project_users

```

Successful response is an array of project_users.
```json
{
	"data":[
		{
			"id":4692190,
			"pid":777,
			"uid":1267998,
			"wid":99,
			"manager":true,
			"rate":4,
		},{
			"id":4692192,
			"pid":777,
			"uid":29624,
			"wid":99,
			"manager":true,
			"rate":4,
		},{
			"id":4692191,
			"pid":777,
			"uid":112047,
			"wid":99,
			"manager":true,
			"rate":4,
		}
	]
}

```

###Mass update for project users###

By supplying multiple project user ids, you can mass update project users.
`PUT https://www.toggl.com/api/v8/project_users/{project_user_ids}`


Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"project_user":{"manager":false,"rate":15,"fields":"fullname"}}' \
	-X PUT https://www.toggl.com/api/v8/project_users/4692190,4692192,4692191
```

Successful response is an array of project_users.
```json
{
	"data":[
		{
			"id":4692190,
			"pid":777,
			"uid":1267998,
			"wid":99,
			"manager":false,
			"rate":15,
			"at":"2013-03-05T09:20:58+00:00"
		},{
			"id":4692192,
			"pid":777,
			"uid":29624,
			"wid":99,
			"manager":false,
			"rate":15,
			"at":"2013-03-05T09:20:58+00:00"
		},{
			"id":4692191,
			"pid":777,
			"uid":112047,
			"wid":99,
			"manager":false,
			"rate":15,
			"at":"2013-03-05T09:20:58+00:00"
		}
	]
}
```

###Delete multiple project users###

By supplying multiple project user ids, you can mass delete project users.
`DELETE https://www.toggl.com/api/v8/project_users/{project_user_ids}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/project_users/4692190,4692192,4692193
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`
