Clients
====================

Client has the following properties
* name: The name of the client (string, required, unique in workspace)
* wid: workspace ID, where the client will be used (integer, required)
* notes: Notes for the client (string, not required)
* hrate: The hourly rate for this client (float, not required, available only for pro workspaces)
* cur: The name of the client's currency (string, not required, available only for pro workspaces)
* at: timestamp that is sent in the response, indicates the time client was last updated

##Create a client##

`POST https://www.toggl.com/api/v8/clients`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"client":{"name":"Very Big Company","wid":777}}' \
	-X POST https://www.toggl.com/api/v8/clients

```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"Very Big Company",
		"at":"2013-02-26T08:45:28+00:00"
	}
}
```

##Update a client##
`PUT https://www.toggl.com/api/v8/clients/{client_id}`

Workspace id (wid) can't be changed.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"client":{"name":"Very Big Company","notes":"something about the client"}}' \
	-X PUT https://www.toggl.com/api/v8/clients/1239455
```

Successful response
```json
{
	"data": {
		"id":1239455,
		"wid":777,
		"name":"Very Big Company",
		"notes":"something about the client",
		"at":"2013-02-26T08:55:28+00:00"
	}
}
```

##Delete a client##

`DELETE https://www.toggl.com/api/v8/clients/{client_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/clients/1239455
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`