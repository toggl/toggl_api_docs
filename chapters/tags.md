Tags
====================

##Create tag##

`POST https://www.toggl.com/api/v8/tags`

Tag must have a workspace id (wid) and an unique name.

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"tag":{"name":"billed","wid":777}}' \
	-X POST https://www.toggl.com/api/v8/tags

```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"billed"
	}
}
```

##Update a tag##
`PUT https://www.toggl.com/api/v8/tags/{tag_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"tag":{"name":"not billed"}}' \
	-X PUT https://www.toggl.com/api/v8/tags/1239455
```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"not billed"
	}
}
```

##Delete a tag##

`DELETE https://www.toggl.com/api/v8/tags/{tag_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/tags/1239455
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`