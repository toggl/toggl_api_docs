Projects
=================

Project has the following properties
* name: The name of the project (string, required, unique for client and workspace)
* wid: workspace ID, where the project will be saved (integer, required)
* cid: client ID(integer, not required)
* active: whether the project is archived or not (boolean, by default true)
* is_private: whether project is accessible for only project users or for all workspace users (boolean, default true)
* template: whether the project can be used as a template (boolean, not required)
* template_id: id of the template project used on current project's creation
* billable: whether the project is billable or not (boolean, default true, available only for pro workspaces)
* at: timestamp that is sent in the response for PUT, indicates the time task was last updated


##Create project##

`POST https://www.toggl.com/api/v8/projects`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"project":{"name":"An awesome project","wid":777,"template_id":10237,"is_private":true,"cid":123397}}' \
	-X POST https://www.toggl.com/api/v8/projects
```

Successful response
```json
{
	"data": {
		"id":193838628,
		"wid":777,
		"cid":123397,
		"name":"An awesome project",
		"billable":false,
		"is_private":true,
		"active":true,
		"at":"2013-03-06T12:15:37+00:00",
		"template_id":10237
	}
}
```

##Get project data##

`GET https://www.toggl.com/api/v8/projects/{project_id}`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X GET https://www.toggl.com/api/v8/projects/193838628

```

Successful response
```json
{
	"data": {
		"id":193838628,
		"wid":777,
		"cid":123397,
		"name":"An awesome project",
		"billable":false,
		"is_private":true,
		"active":true,
		"at":"2013-03-06T12:15:37+00:00",
		"template":true
	}
}
```

##Update project data##

`PUT https://www.toggl.com/api/v8/projects/{project_id}`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"project":{"name":"Changed the name","is_private":false,"cid":123398}}' \
	-X PUT https://www.toggl.com/api/v8/projects/193838628
```


Successful response
```json
{
	"data": {
		"id":193838628,
		"wid":777,
		"cid":123398,
		"name":"Changed the name",
		"billable":false,
		"active":true,
		"at":"2013-03-06T12:15:37+00:00",
		"template":true
	}
}
```


##Get project users##

`GET https://www.toggl.com/api/v8/projects/{project_id}/project_users`
Read more about project user fields from [here](project_users.md).

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X GET https://www.toggl.com/api/v8/projects/193838628/project_users

```

Successful response is an array of the project's users
```json
[
	{
		"id":4692190,
		"pid":777,
		"uid":123,
		"wid":99,
		"manager":true,
		"rate":4
	},
	{
		"id":4692193,
		"pid":777,
		"uid":125,
		"wid":99,
		"manager":false,
		"rate":4
	}
]
```

##Get workspace projects##

Retrieving workspace projects is documented [here](workspaces.md#get-workspace-projects).