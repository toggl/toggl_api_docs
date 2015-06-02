#Project dashboard#

[Project dashboard](http://support.toggl.com/project-dashboard/) returns at-a glance information for a single project. This feature is only available with Toggl pro.

##Request##

`GET /reports/api/v2/project`
 
Parameters are:

* `user_agent` string, **required**, email, or other way to contact client 
   application developer
* `workspace_id` integer, **required**. The workspace whose data you want to
 access
* `project_id` integer, **required**. The project whose data you want to
 access
* `page` integer, optional.  number of 'tasks_page' you want to fetch
* `order_field` string: name/assignee/duration/billable_amount/estimated_seconds
* `order_desc` string:  on/off, `on` for descending and `off` for ascending 
order

##Response##

Project dashboard response has following strucure: ([json schema]
(project_dashboard_schema.json))

```json
{
  "name": "string, name of project",
  "duration": "integer, total time tracked for this project (ms)",
  "billable_duration": "integer, billable time tracked for this project (ms)",
  "billable_amount": "number, sum of earnings",
  "tasks_count": "integer, total number of tasks in this project",
  "currency": "string, currency of billable_amount",
  "estimated_seconds": "integer, estimate of how long project would take in seconds",
  "task_estimated_seconds": "integer, total of each task time estimations in seconds",
  "use_task_estimated_seconds": "boolean, flag indicating if total of task time estimations (true) should be used as grand-estimate, for false value estimated_seconds should be used instead",
  "tasks_per_page": "integer,  number of tasks per page in #tasks_page, use together with #tasks_count to build pagination links",
  "users": [{  
      "name": "string, toggl user name",
      "duration": "integer, time tracked for this project by given user (ms)",
      "billable_duration": "integer, billable time tracked for this project by given user (ms)",
      "billable_amount": "number, billable amount contributed by given user"
  }],
  "top_tasks": [{
      "duration": "integer, time tracked for this project (ms)",
      "billable_amount": "number, billable amount contributed by this task",
      "name": "string, name of task, null value for name has special meaning - 'Sum of Others'",
      "estimated_seconds": "integer, estimate of how long task should take in seconds",
      "asignee": "string, toggl user assigned to this task"
   }],
  "tasks_page": [{
      "name": "string, name of task",
      "assignee": "string, toggl user assigned to this task",
      "estimated_seconds": "integer, estimate of how long task should take in seconds",
      "duration": "integer, time tracked for this project (ms)",
      "billable_amount": "number, billable amount contributed by this task"
}]
}
```

* `users` array holds all users who contributed time to project
* `top_tasks` is array of tasks that contributed more than 10% of total time
 to project
* `tasks_page` is array holding all tasks of given project, one page 
 at a time

##Example##
 
request:
```shell
curl -u my-secret-toggl-api-token:api_token -X GET "https://www.toggl.com/reports/api/v2/project/?page=1&user_agent=devteam@example.com&workspace_id=1&project_id=2"
```

response (formatted for readability):
```json
{
    "name": "Accounting",
    "duration": 74417000,
    "duration_without_task": 21137000,
    "billable_duration": 43097000,
    "billable_amount": 838,
    "tasks_count": 4,
    "currency": "USD",
    "estimated_seconds": 25200,
    "task_estimated_seconds": 72000,
    "use_task_estimated_seconds": true,
    "tasks_per_page": 10,
    "users": [
        {
            "name": "Paul McMurdoh",
            "duration": 74417000,
            "billable_duration": 43097000,
            "billable_amount": 838
        }
    ],
    "top_tasks": [
        {
            "duration": 25200000,
            "billable_amount": 371,
            "name": "Invoices",
            "estimated_seconds": 18000,
            "asignee": null
        },
        {
            "duration": 10800000,
            "billable_amount": null,
            "name": "Writing",
            "estimated_seconds": 21600,
            "asignee": null
        },
        {
            "duration": 10080000,
            "billable_amount": 196,
            "name": "Planning",
            "estimated_seconds": 14400,
            "asignee": null
        },
        {
            "duration": 7200000,
            "billable_amount": 140,
            "name": "Billing",
            "estimated_seconds": 18000,
            "asignee": null
        }
    ],
    "tasks_page": [
        {
            "name": "Billing",
            "assignee": null,
            "estimated_seconds": 18000,
            "duration": 7200000,
            "billable_amount": 140
        },
        {
            "name": "Invoices",
            "assignee": null,
            "estimated_seconds": 18000,
            "duration": 25200000,
            "billable_amount": 371
        },
        {
            "name": "Planning",
            "assignee": null,
            "estimated_seconds": 14400,
            "duration": 10080000,
            "billable_amount": 196
        },
        {
            "name": "Writing",
            "assignee": null,
            "estimated_seconds": 21600,
            "duration": 10800000,
            "billable_amount": null
        }
    ]
}
```