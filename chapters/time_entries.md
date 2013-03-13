Time Entries
====================

The requests are scoped with the user whose API token is used. Only his/her time entries are updated, retrieved and created.

Time entry has the following properties
* description: (string, required)
* wid: workspace ID (integer, required if pid or tid not supplied)
* pid: project ID (integer, not required)
* tid: task ID (integer, not required)
* billable: (boolean, not required, default false, available for pro workspaces)
* start: time entry start time (string, required, ISO 8601 date and time)
* stop: time entry stop time (string, not required, ISO 8601 date and time)
* duration: time entry duration in seconds. If the time entry is currently running, the duration attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch. (integer, required)
* created_with: the name of your client app (string, required)
* tags: a list of tag names (array of strings, not required)
* duronly: should Toggl show the start and stop time of this time entry? (boolean, not required)
* at: timestamp that is sent in the response, indicates the time item was last updated

##Create a time entry##

`POST https://www.toggl.com/api/v8/time_entries/`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"time_entry":{"description":"Meeting with possible clients","tags":["billed"],"duration":1200,"start":"2013-03-05T07:58:58.000Z","pid":123}}' \
	-X POST https://www.toggl.com/api/v8/time_entries

```

Successful response
```json
{
	"data":
	{
		"id":436694100,
		"pid":123,
		"wid":777,
		"billable":false,
		"start":"2013-03-05T07:58:58.000Z",
		"duration":1200,
		"description":"Meeting with possible clients",
		"tags":["billed"]
	}
}
```

##Get time entry details##

`GET https://www.toggl.com/api/v8/time_entries/{time_entry_id}`

Example request:

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET http://localhost:8080/api/v8/time_entries/436694100
```

Successful response
```json
{
	"data":{
		"id":436694100,
		"wid":777,
		"pid":193791,
		"tid":13350500,
		"billable":false,
		"start":"2013-02-27T01:24:00+00:00",
		"stop":"2013-02-27T07:24:00+00:00",
		"duration":21600,
		"description":"Some serious work",
		"tags":["billed"],
		"at":"2013-02-27T13:49:18+00:00"
	}
}
```


##Update a time entry##
`PUT https://www.toggl.com/api/v8/time_entries/{time_entry_id}`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"time_entry":{"description":"Meeting with possible clients","tags":[""],"duration":1240,"start":"2013-03-05T07:58:58.000Z","stop":"2013-03-05T08:58:58.000Z","duronly":true,"pid":123,"billable":true}}' \
	-X PUT https://www.toggl.com/api/v8/time_entries/436694100

```

Successful response
```json
{
	"data":
	{
		"id":436694100,
		"pid":123,
		"wid":777,
		"billable":false,
		"start":"2013-03-05T07:58:58.000Z",
		"stop":"2013-03-05T08:58:58.000Z",
		"duration":1240,
		"description":"Meeting with possible clients",
		"billable": true,
		"at": "2013-03-05T12:34:50+00:00"
	}
}
```

##Delete a time entry##


`POST https://www.toggl.com/api/v8/time_entries/{time_entry_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/time_entries/1239455
```

Successful request will return `200 OK`


##Get time entries started in a specific time range##

`GET https://www.toggl.com/api/v8/time_entries`
With `start_date` and `end_date` parameters you can specify the date range of the time entries returned. If `start_date` and `end_date` are not specified, time entries started during the last 9 days are returned.
`start_date` and `end_date` must be ISO 8601 date and time strings.

Example request with start date 2013-03-10T15:42:46+02:00 and end_date 2013-03-12T15:42:46+02:00

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X GET "https://www.toggl.com/api/v8/time_entries?start_date=2013-03-10T15%3A42%3A46%2B02%3A00&end_date=2013-03-12T15%3A42%3A46%2B02%3A00"
```

Successful response
```json
[
	{
		"id":436691234,
		"wid":777,
		"pid":123,
		"billable":true,
		"start":"2013-03-11T11:36:00+00:00",
		"stop":"2013-03-11T15:36:00+00:00",
		"duration":14400,
		"description":"Meeting with the client",
		"tags":[""],
		"at":"2013-03-11T15:36:58+00:00"
	},{
		"id":436776436,
		"wid":777,
		"billable":false,
		"start":"2013-03-12T10:32:43+00:00",
		"stop":"2013-03-12T14:32:43+00:00",
		"duration":18400,
		"description":"important work",
		"tags":[""],
		"at":"2013-03-12T14:32:43+00:00"
	}
]
```