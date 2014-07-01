#Detailed report#

The detailed report returns the time entries for the requested parameters/filters.

##Request##

In addition to the [standard request parameters](../reports.md#request-parameters), there is one additional parameter in detailed reports. As the returned data is paginated you have to add the page parameter.
* `page`: integer, default 1

##Response##

The repsonse will include the [standard response parameters](../reports.md#successful-response), as well as:
* `total_count`: total number of time entries that were found for the request. Pay attention to the fact that the amount of time entries returned is max the number which is returned with the `per_page` field (currently 50). To get the next batch of time entries you need to do a new request with same parameters and the incremented `page` parameter. It is not possible to get all the time entries with one request.
* `per_page`: how many time entries are displayed in one request

###Data array###

Time entry data
* id: time entry id
* pid: project id
* project: project name for which the time entry was recorded
* client: client name for which the time entry was recorded
* tid: task id
* task: task name for which the time entry was recorded
* uid: user id whose time entry it is
* user: full name of the user whose time entry it is
* description: time entry description
* start: start time of the time entry in ISO 8601 date and time format (YYYY-MM-DDTHH:MM:SS)
* end: end time of the time entry in ISO 8601 date and time format (YYYY-MM-DDTHH:MM:SS)
* dur: time entry duration in milliseconds
* updated: last time the time entry was updated in ISO 8601 date and time format (YYYY-MM-DDTHH:MM:SS)
* use_stop: if the stop time is saved on the time entry, depends on user's personal settings.
* is_billable: boolean, if the time entry was billable or not
* billable: billed amount
* cur: billable amount currency
* tags: array of tag names, which assigned for the time entry

##Example##

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET "https://toggl.com/reports/api/v2/details?workspace_id=123&since=2013-05-19&until=2013-05-20&user_agent=api_test"
```


Successful response
```json
  {
    "total_grand":23045000,
    "total_billable":23045000,
    "total_count":2,
    "per_page":50,
    "total_currencies":[{"currency":"EUR","amount":128.07}],
    "data":[
      {
        "id":43669578,
        "pid":1930589,
        "tid":null,
        "uid":777,
        "description":"tegin tööd",
        "start":"2013-05-20T06:55:04",
        "end":"2013-05-20T10:55:04",
        "updated":"2013-05-20T13:56:04",
        "dur":14400000,
        "user":"John Swift",
        "use_stop":true,
        "client":"Avies",
        "project":"Toggl Desktop",
        "task":null,
        "billable":8.00,
        "is_billable":true,
        "cur":"EUR",
        "tags":["paid"]
      },{
        "id":43669579,
        "pid":1930625,
        "tid":1334973,
        "uid":7776,
        "description":"agee",
        "start":"2013-05-20T09:37:00",
        "end":"2013-05-20T12:01:41",
        "updated":"2013-05-20T15:01:41",
        "dur":8645000,
        "user":"John Swift",
        "use_stop":true,
        "client":"Apprise",
        "project":"Development project",
        "task":"Work hard",
        "billable":120.07,
        "is_billable":true,
        "cur":"EUR",
        "tags":[]
      }
    ]
  }
```
