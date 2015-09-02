#Weekly report#

The weekly report gives aggregated 7 day durations or earnings grouped by users and projects.

##Request##

The weekly report accepts all of the [standard request parameters](../reports.md#request-parameters), with the exception of the `until` parameter.  Instead, 7 days starting from `since` are shown.

Additional request parameters for this report are:
* `grouping`: `users`/`projects`, default projects. If one grouping is selected, the other acts as subgrouping.
* `calculate`: `time`/`earnings`, default time

##Response##

The response will include the [standard response parameters](../reports.md#successful-response), as well as:
* `week_totals`: array of total amounts/hours for every day (null if there's no work on a certain day)

###Data array###

Grouping is `projects` the main grouping looks like this
* title: object containing project and client name
* pid: project id
* totals: array of total amounts for the user during selected seven days
* details: array of projects and project totals for the user during the seven days

Example
```json
  {
    "title":{"project":"Toggl Desktop","client":"Toggl"},
    "pid":7363449,
    "totals":[null,null,null,null,14400000,null,null,14400000],
    "details":[
      {
        "uid":352243,
        "title":{"user":"John Swift"},
        "totals":[null,null,null,null,14400000,null,null,14400000]
      }
    ]
  }

```

Grouping is `users`
* title: object containing the name of the user
* uid: user id
* totals: array of total amounts
* details: array of users and their totals for this project during the seven days

Example
```json
  {
    "title":{"user":"John Swift"},
    "uid":352243,
    "totals":[null,null,14400000,null,14400000,null,null,28800000],
    "details":[
      {
        "pid":73649,
        "title":{ "client":"Toggl","project":"Toggl Desktop"},
        "totals":[null,null,null,null,14400000,null,null,14400000]
      },
      {
        "pid":1120651,
        "title":{"client":null,"project":"Super big client"},
        "totals":[null,null,14400000,null,null,null,null,14400000]
      }
    ]
  }
```

The totals array is different depending on the selected calculation method.
If `calculate=time`, it is a simple array with 8 numbers - each for one day and the 8th for the seven day total.
```
  totals:[null,null,0,null,40,null,null,40]
```

If `calculate=earnings`, it is an array of objects with currency string and the amounts array with 8 numbers - each for one day and the 8th for the seven day total.
```json
  "totals":[
    {
      "currency":"EUR",
      "amount":[null,null,0,null,40,null,null,40]
    },
    {
      "currency":"USD",
      "amount":[20,null,0,null,14,null,null,34]
    }
  ]
```

##Example##

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET "https://toggl.com/reports/api/v2/weekly?workspace_id=123&since=2013-05-19&until=2013-05-20&user_agent=api_test"
```

Successful response
```json
{
    "total_grand":36004000,
    "total_billable":14400000,
    "total_currencies":[{"currency":"EUR","amount":40.00}],
    "data":[
      {
        "title":{"project":"Toggl Desktop","client":"Toggl"},
        "pid":7363449,
        "totals":[null,null,null,null,14400000,null,null,14400000],
        "details":[
          {
            "uid":352243,
            "title":{"user":"John Swift"},
            "totals":[null,null,null,null,14400000,null,null,14400000]
          }
        ]
      },{
        "title":{"project":"Important Client","client":null},
        "pid":1651,
        "totals":[null,null,14400000,null,null,null,null,14400000],
        "details":[
          {
            "uid":31232243,
            "title":{"user":"Jane Doe"},
            "totals":[null,null,14400000,null,null,null,null,14400000]
          }
        ]
      },{
      "title":{"project":null,"client":null},
      "pid":null,
      "totals":[null,null,1000,7203000,null,null,null,7204000],
      "details":[
        {
          "uid":19569,
          "title":{"user":"John Swift"},
          "totals":[null,null,1000,7203000,null,null,null,7204000]
        }
      ]
    }
  ],
    "week_totals":[null,null,14401000,7203000,14400000,null,null,36004000]
  }
```

Example code snippet in Ruby
```ruby
require 'net/http'
require 'JSON'

wsid = # your workspace id
api_token = #your api token

uri = URI("https://toggl.com/reports/api/v2/weekly?workspace_id=#{wsid}&since=2014-03-01&until=2014-03-05&user_agent=api_example_test")

req = Net::HTTP::Get.new(uri)
req.basic_auth api_token, 'api_token'

http = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true)
resp = http.request(req)

puts resp.body
puts JSON.parse(resp.body)
```
