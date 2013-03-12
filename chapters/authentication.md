Authentication
==========

To use the API, you need to authenticate yourself. This can be done via HTTP POST or HTTP Basic Auth. After successful authentication a session is created using a cookie.

If authentication fails, HTTP status code 403 is returned.

## HTTP Basic Auth with e-mail and password ##

Example request
```shell
curl -v -u john.doe@gmail.com:secret \
-X GET https://www.toggl.com/api/v8/me

```

Response
```json
{
	"since":1361780172,
	"data": {
		"id":123,
		"api_token":"1971800d4d82861d8f2c1651fea4d212",
		"default_wid":777,
		"email":"john.doe@gmail.com",
		"fullname":"John Doe",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":1,
		"language":"en_US",
		"image_url":"https://www.toggl.com/images/profile.png",
		"new_blog_post":{},
		"projects": [
			{
				"id":90123,
				"wid":777,
				"name":"Our best project",
				"billable":true,
				"active":true,
				"at":"2013-02-12T09:47:57+00:00"
			}
		],
		"tags": [
			{
				"id":238526,
				"wid":777,
				"name":"billed"
			}
		],
		"tasks": [],
		"workspaces": [
			{
				"id":777,"name":"John's WS","at":"2012-11-28T11:56:49+00:00"
			}
		],
		"clients": []
	}
}

```

##HTTP Basic Auth with API token##
When using Basic Auth and API token, use the API token as username and string "api_token" as password.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token -X GET https://www.toggl.com/api/v8/me
```

Response
```json
{
	"since":1361780172,
	"data": {
		"id":123,
		"api_token":"1971800d4d82861d8f2c1651fea4d212",
		"default_wid":777,
		"email":"john.doe@gmail.com",
		"fullname":"John Doe",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":1,
		"language":"en_US",
		"image_url":"https://www.toggl.com/images/profile.png",
		"new_blog_post":{},
		"projects": [],
		"tags": [],
		"tasks": [],
		"workspaces": [
			{
				"id":777,"name":"John's WS","at":"2012-11-28T11:56:49+00:00"
			}
		],
		"clients": []
	}
}

```

## Authentication with a session cookie ##

`POST https://www.toggl.com/api/v8/sessions`

It's possible to create a session. The session creation request sets a cookie in the response header `toggl_api_session`, which you can use for authentication in all the API requests. The cookie expires in 24 hours.

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X POST https://www.toggl.com/api/v8/sessions
```

Successful response header includes the cookie

```shell
< Set-Cookie: toggl_api_session=MTM2MzA4MJa8jA3OHxEdi1CQkFFQ180SUFBUkFCRUFBQVlQLUNBQUVHYzNSeWFXNW5EQXdBQ25ObGMzTnBiMjVmYVdRR2MzUnlhVzVuREQ0QVBIUnZaMmRzTFdGd2FTMXpaWE56YVc5dUxUSXRaalU1WmpaalpEUTVOV1ZsTVRoaE1UaGhaalpqWkRkbU5XWTJNV0psWVRnd09EWmlPVEV3WkE9PXweAkG7kI6NBG-iqvhNn1MSDhkz2Pz_UYTzdBvZjCaA==; Path=/; Expires=Wed, 13 Mar 2013 09:54:38 UTC; Max-Age=86400; HttpOnly
```
And body contains user's data

```json
{
	"since":0,
	"data": {
		"id":9000,
		"api_token":"1971800d4d82861d8f2c1651fea4d212",
		"default_wid":777,
		"email":"johnt@swift.com",
		"fullname":"John Swift",
		"jquery_timeofday_format":"h:i A",
		"jquery_date_format":"m/d/Y",
		"timeofday_format":"h:mm A",
		"date_format":"MM/DD/YYYY",
		"store_start_and_stop_time":true,
		"beginning_of_week":0,
		"language":"en_US",
		"image_url":"https://www.toggl.com/system/avatars/9000/small/open-uri20121116-2767-b1qr8l.png",
		"sidebar_piechart":false,
		"at":"2013-03-06T12:18:42+00:00",
		"retention":9,
		"record_timeline":true,
		"render_timeline":true,
		"timeline_enabled":true,
		"timeline_experiment":true,
		"new_blog_post":{},
		"invitation":{}
	}
}
```

## Destroy the session ##

Destroy the session manually by sending an according request to the API.

`DELETE https://www.toggl.com/api/v8/sessions`

Example request

```shell
curl -v --cookie toggl_api_session=MTM2MzA4MJa8jA3OHxEdi1CQkFFQ180SUFBUkFCRUFBQVlQLUNBQUVHYzNSeWFXNW5EQXdBQ25ObGMzTnBiMjVmYVdRR2MzUnlhVzVuREQ0QVBIUnZaMmRzTFdGd2FTMXpaWE56YVc5dUxUSXRaalU1WmpaalpEUTVOV1ZsTVRoaE1UaGhaalpqWkRkbU5XWTJNV0psWVRnd09EWmlPVEV3WkE9PXweAkG7kI6NBG-iqvhNn1MSDhkz2Pz_UYTzdBvZjCaA== \
	-X DELETE https://www.toggl.com/api/v8/sessions
```

Successful request will return `200 OK`.