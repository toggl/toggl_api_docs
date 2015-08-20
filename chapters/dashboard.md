Dashboard
==========

Dashboard's main purpose is to give an overview of what users in the workspace are doing and have been doing.
Dashboard request returns two objects:
* Activity
* Most active user

The activity object holds the data of 10 latest actions in the workspace.
Activity object has the following properties
* user_id: user ID
* project_id: project ID (ID is 0 if time entry doesn't have project connected to it)
* duration: time entry duration in seconds. If the time entry is currently running, the duration attribute contains a negative value, denoting the start of the time entry in seconds since epoch (Jan 1 1970). The correct duration can be calculated as current_time + duration, where current_time is the current time in seconds since epoch.
* description: (Description property is not present if time entry description is empty)
* stop: time entry stop time (ISO 8601 date and time. Stop property is not present when time entry is still running)
* tid: task id, if applicable

The most active user object holds the data of the top 5 users who have tracked the most time during last 7 days.
Most active user object has the following properties
* user_id: user ID
* duration: Sum of time entry durations that have been created during last 7 days

##Get Dashboard data##

`GET https://www.toggl.com/api/v8/dashboard/{workspace_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
-X GET https://www.toggl.com/api/v8/dashboard/3134975
```

Successful response
```json
{
   "most_active_user":[
      {
         "user_id":426060,
         "duration":97652
      },
      {
         "user_id":682638,
         "duration":30600
      },
      {
         "user_id":426054,
         "duration":23400
      },
      {
         "user_id":682650,
         "duration":21600
      },
      {
         "user_id":426055,
         "duration":7506
      }
   ],
   "activity":[
      {
         "user_id":6822650,
         "project_id":34654059,
         "duration":-1392018288,
         "description":"new design"
      },
      {
         "user_id":4260544,
         "project_id":30349753,
         "duration":-1392019190,
         "description":"backend"
      },
      {
         "user_id":426055,
         "project_id":3689864,
         "duration":-1392021417,
         "description":"writing post"
      },
      {
         "user_id":6822650,
         "project_id":36898655,
         "duration":10800,
         "description":"design",
         "stop":"2014-02-14T14:30:00+00:00"
      },
      {
         "user_id":35224123,
         "project_id":0,
         "duration":65573,
         "stop":"2014-02-13T08:12:06+00:00"
      },
      {
         "user_id":35224123,
         "project_id":3689865,
         "duration":3600,
         "stop":"2014-02-10T15:30:00+00:00"
      },
      {
         "user_id":35224123,
         "project_id":3535357,
         "duration":10800,
         "description":"backend",
         "stop":"2014-02-10T14:30:00+00:00"
      },
      {
         "user_id":6822638,
         "project_id":3534557,
         "duration":12600,
         "description":"call",
         "stop":"2014-02-10T12:22:00+00:00"
      },
      {
         "user_id":35224123,
         "project_id":303258,
         "duration":5280,
         "description":"research for presentation",
         "stop":"2014-02-10T11:58:00+00:00"
      },
      {
         "user_id":35224123,
         "project_id":346409,
         "duration":7200,
         "description":"Meeting",
         "stop":"2014-02-10T10:00:00+00:00"
      }
   ]
}
