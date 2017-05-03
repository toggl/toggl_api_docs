CORS whitelists
====================

A CORS whitelist record has the following properties
* id: record ID, where the record will be saved (integer)
* user_id: user ID, for whom the domain is whitelisted (integer)
* domain: the whitelisted domain (string)

Record id (id) can't be changed on update.

## Actions ##
### Create an entry ###

`POST https://www.toggl.com/api/v9/me/cors`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-Type: application/json" \
	-d '{"domain":"url.com"}' \
	-X POST https://www.toggl.com/api/v9/me/cors

```

Successful response
```json
{
	"id":1335076912,
 	"domain":"url.com",
  	"user_id":456
}
```

### Get entries for current user ###

`GET https://www.toggl.com/api/v9/me/cors`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X GET https://www.toggl.com/api/v9/me/cors
```

Successful response
```json
[
	{
		"id":1335076912,
		"domain":"url.com",
		"user_id":456
	},
	{
		"id":1335076982,
		"domain":"url2.com",
		"user_id":456
	},
	{
		"id":1335076993,
 		"domain":"url3.com",
		"user_id":456
	}
]
```

### Delete an entry ###

`DELETE https://www.toggl.com/api/v9/me/cors/{id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v9/me/cors/1335076912
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`
