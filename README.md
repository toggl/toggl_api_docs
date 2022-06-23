Toggl API Documentation
====================

## ⚠️ Important ⚠️

We're moving our documentation to https://developers.track.toggl.com/docs/ where you will find up-to date docs for both the Toggl API and Webhooks API services. We're also working on moving the Reports API documentation soon, but for now this repository can still be used for the Reports API.

If you're running into issues or require help with anything, please contact our dedicated 24/5 support staff over on [our support pages](https://support.toggl.com/en/). You can chat with them using the icon on the right bottom.

----

Toggl API is divided into three:

* [Toggl API](toggl_api.md)
* [Reports API](reports.md)
* [Webhooks API](webhooks.md)

For changing data, including tracking time, you'll need to use the **Toggl API**.

If you want to get time entries of all the workspace users and aggregated data for reporting, you need to use the read-only **Reports API**, which gives you many options for filtering, grouping and sorting.

For monitoring data changes in a workspace, you'll need the **Webhooks API**.

## The API Format

The API accepts only JSON requests. Please make sure you're setting `Content-Type: application/json`in your request header. Each request returns a **JSON-encoded** body.

The result of each action is communicated via standard HTTP response codes.

Times and dates use the ISO 8601 standard, more specifically a subset described in [RFC 3339](http://www.ietf.org/rfc/rfc3339.txt).

Please do note that the times and dates are stored in UTC (GMT), on return the data is set into the appropriate timezone according to the setting in user profile.
3rd party applications should make sure that they are using correct timezones and also consider daylight saving (where applicable).

For rate limiting we have implemented a [Leaky bucket](http://en.wikipedia.org/wiki/Leaky_bucket). When a limit has been hit the request will get a HTTP 429 response and it's the task of the client to sleep/wait until bucket is empty. Limits will and can change during time, but a safe window will be *1 request per second*.
Limiting is applied per api token per IP, meaning two users from the same IP will get their rate allocated separately.

### Example requests

The example requests here are done using a command line tool called [cURL](http://en.wikipedia.org/wiki/CURL). If you want to try the requests out yourself, you can download cURL from [here](http://curl.haxx.se/download.html). It is available for all possible operating systems.

Under Ubuntu installing cURL is very easy:

```shell
sudo apt-get install curl
```

## API token

Each user in Toggl.com has an API token. They can find it under "My Profile" in their Toggl account.


## Help us towards a better API

The Toggl API has moved to Github so you could actively participate in helping us making the API better. If you have any requests or you found a bug, you can use Github issues to let us know. You can also fork the docs and send a pull request with improvements

## Code examples

### Java
* [Benno](https://github.com/bennob) has updated JToggl, a Java wrapper for the Toggl API to support v8: https://github.com/bbaumgartner/jtoggl
* [rocketbase-io](https://github.com/rocketbase-io) build Toggl-Report-Api, a Java wrapper for the Toggl Report API: https://github.com/rocketbase-io/toggl-report-api

### Python
* [Mosab Ahmad](https://github.com/mos3abof) has created a project using Toggl API to calculate how many hours he should work to achieve monthly goals: https://github.com/mos3abof/toggl_target
* [Mikhail Novikov](https://github.com/kurtgn) has created a graphical utility for displaying historical data: https://github.com/kurtgn/chronicl
* [Robert Adams](https://github.com/drobertadams) and [Beau Raines](https://github.com/beauraines) have updated [toggl-cli](https://github.com/drobertadams/toggl-cli) to API v8. It provides a set of Python objects for interacting with Toggl, as well as a command-line interface: https://github.com/drobertadams/toggl-cli
* [Matthew Downey](https://github.com/matthewdowney) has started a Toggl API wrapper: https://github.com/matthewdowney/TogglPy
* [Matthias Büchi](https://github.com/ynop) has created a simple tool to calculate the difference between the tracked hours in Toggl and the hours one should work in a given period: https://github.com/ynop/togglore
* [David Cako](https://github.com/david-cako) has written an efficient command line interface for inputting time-insensitive toggl entries: https://github.com/david-cako/toggl-hammer

### Ruby
* [Tom Kane](https://github.com/kanet77) has written a Ruby wrapper for Toggl API v8: https://github.com/kanet77/togglv8

### Node.js
* [Damian Mee](https://github.com/meeDamian) has written a CLI tool in Node.js for Toggl API v8: https://github.com/meeDamian/toggl-cli
* [Alexander Makarenko](https://github.com/estliberitas) has written a library for Node.js for Toggl API v8: https://github.com/7eggs/node-toggl-api

### C++

* TogglDesktop has an [open source, cross platform library](https://github.com/toggl-open-source/toggldesktop/tree/master/src/lib) that can be reused in your own apps.

### .NET

* The Toggl mobile apps have [shared C# libraries](https://github.com/toggl/mobileapp) which provide access to the API and also common clientside business logic we use. Feel free to use as little or much of it as you want.

### Scala
* [Gabriele Alese](https://github.com/gabalese) has written a command line client for Toggl: https://github.com/gabalese/toggl-cli

### PHP
* [Arend Jan Tetteroo](https://github.com/arendjantetteroo) has written a library for PHP for Toggl API v8, based on the excellent Guzzle library: https://github.com/arendjantetteroo/guzzle-toggl
* [Morning Train](https://morningtrain.dk) has written PHP classes for Toggl API v8 (It is based on Guzzle 6): https://github.com/Morning-Train/toggl-api
* [Ixudra](https://ixudra.be) has written a Laravel PHP library for Toggl API v8: https://github.com/ixudra/toggl

### Go
* [Doug Chimento](https://github.com/dougEfresh) has written a Go wrapper for Toggl API v8: https://github.com/dougEfresh/gtoggl 

### Elixir
* [Víctor Viruete](https://github.com/hopsor) has written an Elixir wrapper for Toggl API v8 and Reports API: https://github.com/diacode/togglex

## 3rd party apps
* [Federico Vaga](https://github.com/FedericoVaga) has written a little plasmoid for KDE: https://github.com/FedericoVaga/plasmoggl (on opendesktop: http://opendesktop.org/content/show.php/Plasmoggl?content=168536)
* [rocketbase-io](https://github.com/rocketbase-io/toggl-reporter) has written a tiny SpringBoot-Application, build with vaadin and mongodb, that pulls your TimeEntries from toggl. The stored information allow a fine grained reporting and analysis that aren't possible within toggl (like detailed working hours in comparison between team-members and many more). A [docker-image](https://hub.docker.com/r/rocketbaseio/toggl-reporter/) is also provided...
 * [Chingiz Nazar](https://github.com/sultanbekuly) created an Arduino project to track time using Toggl Track API. This project allows building a home-made paper cube which can be configured to start tracking time depending on which side of the cube is facing upwards: [time_tracker_cube](https://github.com/sultanbekuly/time_tracker_cube). An article explaining the development and usage of this project is also available: [Building an Arduino Time Tracker Cube with Toggl's Open API](https://hackernoon.com/building-an-arduino-time-tracker-cube-with-toggls-open-api).

### Perl
* [Jason Kruczynski](https://github.com/jkruczynski) has written a perl wrapper for the API. It creates and authenticates a session for API V8 or API V2 depending on the URL. It implements a few functions for exporting reports. https://github.com/jkruczynski/Toggl-API-Wrapper-Perl

### R
* [Vincent Guyader](http://thinkr.fr) has written a simple R and Rstudio implementation for the API. https://github.com/ThinkRstat/togglr

### Swift
* [Nghia Tran](https://github.com/NghiaTranUIT) has written a macOS wrapper in Swift 4.2. So, we can integrate the Toggl API v8 to your own app easier: https://github.com/NghiaTranUIT/Toggl-Swift

