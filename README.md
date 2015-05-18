Toggl API Documentation
====================

Toggl API is divided into two

* [Toggl API](toggl_api.md)
* [Reports API](reports.md)


For changing data, including tracking time, you'll need to use the **Toggl API**.

If you want to get time entries of all the workspace users and aggregated data for reporting, you need to use the read-only **Reports API**, which gives you many options for filtering, grouping and sorting.

##The API Format##

The API accepts only JSON requests. Please make sure you're setting `Content-Type: application/json`in your request header. Each request returns a **JSON-encoded** body.

The result of each action is communicated via standard HTTP response codes.

Times and dates use the ISO 8601 standard, more specifically a subset described in [RFC 3339](http://www.ietf.org/rfc/rfc3339.txt).

Please do note that the times and dates are stored in UTC (GMT), on return the data is set into the appropriate timezone according to the setting in user profile.
3rd party applications should make sure that they are using correct timezones and also consider daylight saving (where applicable).

For rate limiting we have implemented a [Leaky bucket](http://en.wikipedia.org/wiki/Leaky_bucket). When a limit has been hit the request will get a HTTP 429 response and it's the task of the client to sleep/wait until bucket is empty. Limits will and can change during time, but a safe window will be *1 request per second*.
Limiting is applied per api token per IP, meaning two users from the same IP will get their rate allocated separately.

###Example requests###

The example requests here are done using a command line tool called [cURL](http://en.wikipedia.org/wiki/CURL). If you want to try the requests out yourself, you can download cURL from [here](http://curl.haxx.se/download.html). It is available for all possible operating systems.

Under Ubuntu installing cURL is very easy:

```shell
sudo apt-get install curl
```

##API token##

Each user in Toggl.com has an API token. They can find it under "My Profile" in their Toggl account.


##Help us towards a better API##

The Toggl API has moved to Github so you could actively participate in helping us making the API better. If you have any requests or you found a bug, you can use Github issues to let us know. You can also fork the docs and send a pull request with improvements

##Code examples##

### Java ###
* [Benno](https://github.com/bennob) has updated JToggl, a Java wrapper for the Toggl API to support v8: http://code.google.com/p/jtoggl/

### Python ###
* [Mosab Ahmad](https://github.com/mos3abof) has created a project using Toggl API to calculate how many hours he should work to achieve monthly goals: https://github.com/mos3abof/toggl_target
* [Mikhail Novikov](https://github.com/kurtgn) has created a graphical utility for displaying historical data: https://github.com/kurtgn/chronicl
* [Robert Adams](https://github.com/drobertadams) and [Beau Raines](https://github.com/beauraines) have updated [toggl-cli](https://github.com/drobertadams/toggl-cli) to API v8. It provides a set of Python objects for interacting with Toggl, as well as a command-line interface: https://github.com/drobertadams/toggl-cli
* [Matthew Downey](https://github.com/matthewdowney) has started a Toggl API wrapper: https://github.com/matthewdowney/TogglPy

### Ruby ###
* [Tom Kane](https://github.com/kanet77) has written a Ruby wrapper for Toggl API v8: https://github.com/kanet77/togglv8

### Node.js ###
* [Alexander Makarenko](https://github.com/estliberitas) has written a library for Node.js for Toggl API v8: https://github.com/7eggs/node-toggl-api

### C++ ###

* TogglDesktop has an [open source, cross platform library](https://github.com/toggl/toggldesktop/tree/master/src/lib) that can be reused in your own apps.

### .NET ###

* Toggl mobile apps have a [shared C# library (Phoebe)](https://github.com/toggl/mobile/tree/master/Phoebe) which provides access to API and also some common clientside business logic we use. Feel free to use as little or much of it you want.
* [Ilya Pirozhenko](https://github.com/sochix) has written a .NET library: https://www.nuget.org/packages/TogglAPI.Net/

### Scala ###
* [Gabriele Alese](https://github.com/gabalese) has written a command line client for Toggl: https://github.com/gabalese/toggl-cli

### PHP ###
* [Arend Jan Tetteroo](https://github.com/arendjantetteroo) has written a library for PHP for Toggl API v8, based on the excellent Guzzle library: https://github.com/arendjantetteroo/guzzle-toggl

##3rd party apps##
* [Federico Vaga](https://github.com/FedericoVaga) has written a little plasmoid for KDE: https://github.com/FedericoVaga/plasmoggl (on opendesktop: http://opendesktop.org/content/show.php/Plasmoggl?content=168536)
