Toggl Reports API v2
=================

Here you can get the general information about how to use Toggl reports API, from how to authenticate, what filters to use and how the response is structured. The available reports are similar to the reports available in [Toggl reports page](https://www.toggl.com/report/show).

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

You can authenticate in the reports API **only** with your API token. For HTTP Basic Auth you have to add the Authorization header with the request. The Authorization header is constructed as follows:
* String with api token `xxxx:api_token` (xxx indicating user's personal token)
* The resulting string literal is then encoded using Base64
* The authorization method and a space i.e. "Basic " is then put before the encoded string.
* For example, if the user agent uses 'Aladdin' as the username and 'open sesame' as the password then the header is formed as follows: `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`

##Request Parameters##

The API expects the request parameters as the query string of the URL.

The following parameters and filters can be used in all of the reports
* workspace_id: integer, **required**. The workspace which data you want to access.
* since: string, ISO 8601 date (YYYY-MM-DD), by default until - 6 days.
* until: string, ISO 8601 date (YYYY-MM-DD), by default today
* billable: possible values: yes/no/both, default both
* client_ids: client ids separated by a comma, **0** if you want to filter out time entries without a client
* project_ids: project ids separated by a comma, **0** if you want to filter out time entries without a project
* user_ids: user ids separated by a comma
* tag_ids: tag ids separated by a comma, **0** if you want to filter out time entries without a tag
* task_ids: task ids separated by a comma, **0** if you want to filter out time entries without a task
* time_entry_ids: time entry ids separated by a comma
* description: string, time entry description
* without_description: true/false, filters out the time entries which do not have a description ('(no description)')
* order_field:
  * date/description/duration/user in detailed reports
  * title/duration/amount in summary reports
  * title/day1/day2/day3/day4/day5/day6/day7/week_total in weekly report
* order_desc: on/off, `on` for descending and `off` for ascending order
* distinct_rates: on/off, default off
* rounding: on/off, default off, rounds time according to workspace settings
* display_hours: decimal/minutes, display hours with minutes or as a decimal number, default minutes

##Successful response##

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

##Failed requests##

In case of unsuccessful request the API returns the error in JSON and corresponding HTTP status code
* message: the general message of the occurred error
* tip: what to do in case of this error
* code: status code of the response

Example error
```json
  {
    "error": {
      "message":"We are sorry, this Error should never happen to you",
      "tip":"Please contact support@toggl.com with information on your request",
      "code":500
    }
  }

```