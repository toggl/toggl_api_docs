Workspace Users
====================

Workspace user has the following properties:
* id: workspace user id (integer)
* uid: user id of the workspace user (integer)
* admin: if user is workspace admin (boolean)
* active: if the workspace user has accepted the invitation to this workspace (boolean)
* invite_url: if user has not accepted the invitation the url for accepting his/her invitation is sent when the request is made by workspace_admin

##Invite users to workspace##

You can add users to workspace by email addresses. A letter inviting the user to your workspace is sent to the user's email.

`POST https://www.toggl.com/api/v8/workspaces/{workspace_id}/invite`

Request has the following properties:
* emails: array of emails

Response has the following properties:
* data:  array of created workspace user objects
* notifications: array of strings. If some emails did not pass the validation the error is described here.


Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"emails":["john.doe@toggl.com", "Jane.Swift@toggl.com"]}' \
	-X POST https://www.toggl.com/api/v8/workspaces/777/invite
```

Successful response
```json
{
	"data":{[
		"id":3511220,
		"uid":35934278,
		"wid":777,
		"admin":false,
		"active":false,
		"email":"jane.swift@toggl.com",
		"invite_url":"https://toggl.com/user/accept_invitation?code=ec2876e421234dfasd0fa1c55370d3940"
	]},
	"notifications":["To add more users you have to Upgrade"]
}
```


##Update workspace user##

Only the admin flag can be changed.

`PUT https://www.toggl.com/api/v8/workspace_users/{workspace_user_id}`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"workspace_user":{"admin":false}}' \
	-X PUT https://www.toggl.com/api/v8/workspace_users/19012628
```


Successful response
```json
{
	"data": {
		"id":19012628,
		"uid":1625,
		"wid":777,
		"admin":false,
		"active":true
	}
}
```

##Delete workspace user##

`DELETE https://www.toggl.com/api/v8/workspace_users/{workspace_user_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/workspace_users/19012628
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`

##Get workspace users##

This request returns not the user objects, but the `workspace_user` objects (the connection between user and workspace)
`GET https://www.toggl.com/api/v8/workspaces/{workspace_id}/workspace_users`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/workspaces/777/workspace_users
```

Successful response is an array of workspace's workspace users
```json
[
	{
		"id":3123855,
		"uid":35224123,
		"wid":777,
		"admin":false,
		"active":false,
		"email":"John@toggl.com",
		"at":"2013-05-17T16:50:36+03:00",
		"name":"John Swift",
		"invite_url":"https://toggl.com/user/accept_invitation?code=fb3ad3db5dasd123c2b529e3a519826"
	},{
		"id":200066,
		"uid":21961,
		"wid":777,
		"admin":true,
		"active":true,
		"email":"Jane@toggl.com",
		"at":"2012-04-11T22:59:33+03:00",
		"name":"Jane"
	}
]
```
