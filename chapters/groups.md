Groups
====================

Group has the following properties
* name: The name of the group (string, required)
* wid: workspace ID, where the group will be used (integer, required)
* at: timestamp that is sent in the response, indicates the time group was last updated

## Create a group

`POST https://track.toggl.com/api/v8/groups`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"group":{"name":"Developers","wid":777}}' \
	-X POST https://track.toggl.com/api/v8/groups

```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"Developers",
		"at":"2013-02-26T08:45:28+00:00"
	}
}
```

## Update a group
`PUT https://track.toggl.com/api/v8/groups/{group_id}`

Workspace id (wid) can't be changed.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"group":{"name":"Front-end Developers"}}' \
	-X PUT https://track.toggl.com/api/v8/groups/1239455
```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"Front-end Developers",
		"at":"2013-02-26T08:55:28+00:00"
	}
}
```

## Delete a group

`DELETE https://track.toggl.com/api/v8/groups/{group_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://track.toggl.com/api/v8/groups/1239455
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`


## Get workspace groups

Retrieving workspace groups is documented [here](workspaces.md#get-workspace-groups).
