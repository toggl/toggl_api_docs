#Summary report#

Summary report returns the aggregated time entries data.
Additional parameters for this report
* grouping
* subgrouping

Use the grouping and subgrouping params to organize the data as needed. By default `grouping:projects` and `subgrouping:time_entries`

Following groupings with subgroupings are available in the summary report
* projects
  * time_entries
  * tasks
  * users
* clients
  * time_entries
  * tasks
  * projects
  * users
* users
  * time_entries
  * tasks
  * projects
  * clients

##Request##
In order to construct a valid query, you have to include a mandatory query parameter, `user_agent`, which can contain a name for your application or an email so that we can get in touch with you in case we need to.
Please note, that `the user_agent` variable needs to be a query parameter in the URL and that sending a valid `UserAgent` HTTP header with the same information wont be considdered valid by the API.

##Response##

###Data array###

General structure of the item in the data array
* id: the id of the grouping object
* title: description of the item, this is an object depending of the grouping as follows:
  * `{project: "Project name", client: "Client name"}` if the grouping is projects
  * `{client: "Client name"}` if the grouping is clients
  * `{user: "User's name"}` if the grouping is users
* items: array of subgrouping items
  * title: title of the subgrouping item, an object depending of the grouping as follows
    * `{project: "Project name", client: "Client name"}` if the grouping is projects
    * `{client: "Client name"}` if the subgrouping is clients
    * `{user: "User's name"}` if the subgrouping is users
    * `{task: "Task name"}` if the subgrouping is tasks
    * `{time_entry: "Time entry description"}` if the subgrouping is time_entries
  * time: time in milliseconds spent on the subgrouping object
  * cur: currency of the subgrouping
  * sum: total amount of the subgrouping
  * rate: hourly rate that is used for calculating the subgrouping amount

```json
  {
    "id":1,
    "title":{},
    "time":14400000,
    "total_currencies":[{"currency":"","amount":null}],
    "items":[
      {
        "title":{},
        "time":14400000,
        "cur":"",
        "sum":null,
        "rate":null
      }
    ]
  }
```

##Example##

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET "https://toggl.com/reports/api/v2/summary?workspace_id=123&since=2013-05-19&until=2013-05-20&user_agent=myclient"
```


Successful response
```json
  {
    "total_grand":36004000,
    "total_billable":14400000,
    "total_currencies":[{"currency":"EUR","amount":40}],
    "data": [
      {
        "id":73569,
        "title":{"project":"Toggl Desktop","client":"Toggl"},
        "time":14400000,
        "total_currencies":[{"currency":"EUR","amount":40}],
        "items":[
          {
            "title":{"time_entry":"Implementing some important things"},
            "time":14400000,
            "cur":"EUR",
            "sum":40,
            "rate":10
          }
        ]
      },{
        "id":193009951,
        "title":{"project":"Toggl Development","client":null},
        "time":14400000,
        "total_currencies":[{"currency":"EUR","amount":0}],
        "items":[
          {
            "title":{"time_entry":"Hard work"},
            "time":14400000,
            "cur":"EUR",
            "sum":0,
            "rate":50
          }
        ]
      },{
        "id":null,
        "title":{"project":null,"client":null},
        "time":7204000,
        "total_currencies":[],
        "items":[
          {
            "title":{"time_entry":"No title yet"},
            "time":1000,
            "cur":"EUR",
            "sum":0,
            "rate":50
          },{
            "title":{"time_entry":"Did nothing"},
            "time":1000,
            "cur":"EUR",
            "sum":0,
            "rate":50
          },{
            "title":{"time_entry":"Hard work again"},
            "time":7202000,
            "cur":"EUR",
            "sum":0,
            "rate":50
          }
        ]
      }
    ]
  }
```
