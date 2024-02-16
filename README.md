# Tinystatus

tinystatus generate an html status page via shell script.
Forked from [bderenzo/tinystatus](https://github.com/bderenzo/tinystatus)

## Features

* Parallel checks
* HTTP, ping, port checks
* HTTP expected status code (401, ...)
* Minimal dependencies (curl, nc and coreutils)
* Easy configuration and customisation
* Tiny (~1kb) optimized result page
* Incident history (manual)

### Changes from original

* Added usage information;
* Ping check will return an average response time, based on pingcount variable;
* Added an scheduled maintenance section (based on [suggestion](https://github.com/bderenzo/tinystatus/issues/19) from the original project). Section has the following logic:
	* Checks for maintenancefile; If no such file exists, no panel will be generated;
	* If maintenance file exists but it is empty, a "success" panel will be generated informing "No maintenance scheduled yet!"
	* If maintenance file exists and has content, a "failure" panel will be generated with the content of the file.
* Slightly changed HTML layout and CSS, added an highlight effect on list items;
* Added a refresh link as an indicator (page will NOT update automatically);
* Slightly changed README, adding additional information;
* Added an attribution link to the origional project on the generated html's footer.

## Demo

An example site is available [here](https://lab.bdro.fr/tinystatus/).

## Setup

To install tinystatus:

* Clone the repository and go to the created directory
* Edit the checks file `checks.csv`
* To add incidents, edit `incidents.txt`
* Generate status page `./tinystatus > index.html`
* Serve the page with your favorite web server

### Example: automatic update

The script creates an static html file that should be served through your web server. The following cronjob is an example, it should generate a new file every minute directly to the web server document folder:

`*/1 * * * * /path/to/tinystatus -c /path/to/checks.csv -i /path/to/incidents.txt -m /path/to/maintenance.txt > /path/to/web/server/www/index.html`

Any changes made to any file (checks, incidents, maintenance or the script itself) will be displayed without requiring any additional action.

## Configuration file

The syntax of `checks.csv` file is:
```
Command, Expected Code, Status Text, Host to check
```

Command can be:
* `http` - Check http status
* `ping` - Check ping status 
* `port` - Check open port status

There are also `http4`, `http6`, `ping4`, `ping6`, `port4`, `port6` for IPv4 or IPv6 only check.  
Note: `port4` and `port6` require OpenBSD `nc` binary.

`incidents.txt` is a simple text file, and each line will be split into a different li tag.
`maintenance.txt` is also a simple text file, and it's content will be displayed as p tags, in a simillar way to the incidents file.

