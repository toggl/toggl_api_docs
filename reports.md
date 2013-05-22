Reports API v2
=================

Here you can get the general information about how to use Toggl reports API, from how to authenticate, what filters to use and how the response is structured. The available reports are similar to the reports available in [Toggl reports page](https://www.toggl.com/report/show)

More detailed information for the reports.
* [Weekly report](reports/weekly.md)
* [Detailed report](reports/detailed.md)
* [Summary report](reports/summary.md)

##URLs##

The reports API base URL is `https://toggl.com/reports/api/v2`

Weekly report URL `GET https://toggl.com/reports/api/v2/weekly`
Detailed report URL: `GET https://toggl.com/reports/api/v2/details`
Summary report URL: `GET https://toggl.com/reports/api/v2/summary`


##Authentication##

You can authenticate in the reports API **only** with your API token. Read more about [HTTP Basic Auth with API token](chapters/authentication.md#http-basic-auth-with-api-token).

##Request Parameters##

The following parameters and filters can be used in all of the reports
* workspace_id: integer, **required**. The workspace which data you want to access.
* since: string, ISO 8601 date (YYYY-MM-DD), by default today
* until: string, ISO 8601 date (YYYY-MM-DD), by default start - 6 days.
* billable: possible values: yes/no/both, default both
* client_ids: client ids separated by a comma, **0** if you want to filter out time entries without a client
* project_ids: project ids separated by a comma, **0** if you want to filter out time entries without a project
* user_ids: user ids separated by a comma
* tag_ids: tag ids separated by a comma, **0** if you want to filter out time entries without a tag
* description: string, time entry description
* without_description: true/false, filters out the time entries which do not have a description ('(no description)')
* order_field: project/
* order_desc: 1/0
* distinct_rates: default off
* rounding: on/off, default off, rounds time according to workspace settings
* display_hours: decimal/minutes, display hours with minutes or as a decimal number, default minutes

##Response structure##

The general structure of the successful response
```json
  {
    "total_grand":null,
    "total_billable":null,
    "total_currency":[{"currency":null,"amount":null}],
    "data":[]
  }
```
The response may include some additional attributes depending on the report type, but the following are present in all of them. The main report data is inside the `data` array.

* total_grand: total time in milliseconds for the selected report
* total_billable: total billable time in milliseconds for the selected report
* total_currency: an array with amounts and currencies for the selected report
* data: an array with detailed information of the requested report. The structure of the data in the array depends on the report.

