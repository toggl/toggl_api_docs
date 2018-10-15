# API v9, Reports API v3 - Basics and recommendations

Given the age of API v8 and Reports API v2 we are working on new versions of those APIs.
The old APIs are still be up and running; there will be an official announcement before the final deprecation.
The documentation for the new API endpoints will become available gradually and there can, and will be, changes at first.

## Authentication options
 - api_token and basic auth header
 - username and password to get a session cookie
 
This list may get additional options
 
## Generic responses and what to do
We're using standard HTTP status codes and kindly ask that API clients respect the following recommendations:
 - in case of 4xx error - don't try another request with the same payload, inspect the response body, most of the times it has a readable message
 - in case of 5xx error - introduce a random delay before the next request
 - in case of 429 (Too Many Requests) - back off for a few minutes (you can expect a rate of 1req/sec to be available)
 - in case of 410 (Gone) - don't try this endpoint again
 - in case of 402 (Payment required) - workspace should be upgraded to have access to said feature, don't repeat the request until that has happened

## General principles
 - API format is JSON (be nice and include a `Content-type: application/json` header to your request)
 - in case of an update request, send only the fields that have changed
 - do not include fields that aren't available for current workspace subscription level (example: on a free plan don't send the default workspace rate when updating workspace)
 - fetch only the data you need
 
## "Scopes"
Endpoints have been grouped by the scope of the entity using it.
Data that's for the currently logged in user is under `/me/*` endpoints, data for one workspace is under `/workspace/{workspace_id}/*` endpoints.
