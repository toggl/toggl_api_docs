Workspace Users
====================

Workspace user has the following properties:
* id: workspace user id (integer)
* uid: user id of the workspace user (integer)
* admin: if user is workspace admin (boolean)
* active: if the workspace user has accepted the invitation to this workspace (boolean)

##Update workspace user##

Only the admin flag can be changed.

`PUT https://www.toggl.com/api/v8/workspace_users/{workspace_user_id}`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
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

Retrieving workspace users is documented [here](workspaces.md#get-workspace-users).