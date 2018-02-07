# mangocli
mangocli v1.0.3 - A utility for querying Mango's REST API

Author: Adam S. Levy <adam@ia3.io>

MangoCLI is a utility for querying the REST API for [Mango SCADA by Infinite
Automation](https://infiniteautomation.com/mango-overview/). It is written in
Bash and uses [curl](https://curl.haxx.se/) and
[jq](https://stedolan.github.io/jq/).

## Example Usage
### Query all data points
```
$ mangocli -a ./jwt-token data-points
```
The default HTTP action is GET. The default host is http://localhost:8080.

### Create a new data point
```
$ mangocli -a ./jwt-token -d @./new-point.json data-points
```
The `-d` option is just the `--data` option for `curl`. The `@` prefix causes
the argument to be parsed as a file. Using the `-d` option changes the default
HTTP action to POST.

### Enable and restart a data point
```
$ mangocli -a ./jwt-token -u enabled=true -u restart=true PUT data-points/enable-disable/DP_XID
```
Using the `-u` option defines a URL query parameter.

### Delete a data point.
```
$ mangocli -a ./jwt-token DELETE data-points/DP_XID
```

## Installation
Ensure that the following dependencies are installed and up to date:
- Bash (4.4.18)
- curl (7.58.0)
- jq (1.5)
They should be available for most every Linux distribution. Earlier versions of
these programs will most likely work as well.

Simply download the script and mark it executable. 
```
$ git clone https://github.com/iA3io/mangocli.git
$ cd mangocli/
$ chmod u+x ./mangocli
```
Install it somewhere in your PATH for convenience.
```
# cp ./mangocli /usr/bin/mangocli
```

## Basic Setup
You will need a valid JWT token from the Mango server you are interacting with.
To do this login to the Mango web interface, navigate to edit Users, and create
a JWT authentication token. Copy this value and use it with the `-a` option. It
can be used directly or loaded from a file or environment variable. See the
manual for more information.

## Manual
```
Usage: mangocli [OPTION...] [ACTION] PATH

Options:
  -h HOST      HOST is the domain name or IP address of the Mango server.
               Can be set using the environment variable MANGOCLI_HOST.
               Defaults to 'http://localhost:8080' if not specified.
               A HOST without a specified protocol will default to HTTPS.

  -a AUTH      AUTH is the JWT token or the path to a file containing it.
               Can be set using the environment variable MANGOCLI_AUTH

  -d DATA      DATA to include in the body of a request. Can be specified multiple times.
               Use `-d @-' to read from stdin or `-d @filename' to read from a file.
               Equivalent to `curl --data="DATA"'. Changes default ACTION to POST.

  -u PARAM     PARAM is URL encoded data. Can be specified multiple times.
               Using this option will pass all data as URL parameters.
               Use `-u @-' to read from stdin or `-u @file' to read from a file.
               Equivalent to `curl --get --data-urlencode="PARAM"'.

  -C CURL_OPTS CURL_OPTS is a space separated list of additional options to pass to curl. 
               Can be specified multiple times.

  -V VERSION   VERSION is the REST API version number: 1 or 2.
               Can be set using the environment variable MANGOCLI_VERSION.
               Defaults to v2.

  -c           Compact JSON output. Removes all whitespace.

  -v           Increase verbosity. Once for info, twice for debug output.

  -q           Quiet mode. Suppresses all error, info and debug messages. 
               Overrides any `-v' options. 

Arguments:
  ACTION       The HTTP action: GET, POST, PUT, DELETE
               Defaults to GET.

  PATH         The path or route for the request
```

## License and Copywrite
MIT License

Copyright (c) 2018 iA3 Inc.
