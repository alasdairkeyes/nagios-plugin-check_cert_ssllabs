# nagios-plugin-check_cert_ssllabs
Check a website's SSL config against the SSLLabs API

## Dependencies

* ssllabs-scan tool https://github.com/ssllabs/ssllabs-scan
* Nagios::Plugin::Functions Perl module
* Nagios::Plugin::Getopt Perl module
* Data::Validate::Domain Perl module
* DateTime Perl module


## Usage

This plugin has two modes, scan and report.

This is due to the amount of time it takes for the scan to run, Nagios will timeout.

`scan` mode shouldbe run no more frequently than once per day via a cron, which writes the score to a file.

`report` mode should be called by Nagios/NRPE, which reads the file and reports the score to Nagios.


## Installation

* Download and compile https://github.com/ssllabs/ssllabs-scan (Requires Go)

* Copy the `check_cert_ssllabs` to your nagios plugin folder and set executable.

* Add the following to your `nrpe.cfg` file
```
command[check_cert_ssllabs]=/usr/lib/nagios/plugins/check_cert_ssllabs --mode=report -H example.com
```

* Add the following to a crontab file for the nagios user
```
34 03 * * *     /var/lib/nagios/plugins/check_cert_ssllabs --mode=scan -H example.com
```

## Examples

* Run with the --help argument to see all options
```
check_cert_ssllabs --help
```

### Scan Mode

* Run in scan mode
```
check_cert_ssllabs --mode=scan -H example.com
```

* Check and specify path to the ssllabs-scan binary
```
check_cert_ssllabs --mode=scan -H example.com -s /path/to/ssllabs-scan
```

* Check cached version. If the certificate has been recently checked at ssllabs, return
the cached information
```
check_cert_ssllabs --mode=scan -H example.com --usecache
```

* Use custom report file to store result
```
check_cert_ssllabs --mode=scan -H example.com -r /var/tmp/example.com-nagios-report-file
```


### Report Mode

* Run with custom warning/critical values
```
check_cert_ssllabs --mode=report -H example.com -c D -w A-
```

* Use custom report file to read result
```
check_cert_ssllabs --mode=report -H example.com -r /var/tmp/example.com-nagios-report-file
```


## Future Work

None planned

## Changelog

* 2017-04-09 :: 0.2.1   :: Add in hostname to output
* 2017-04-09 :: 0.2.0   :: Implementation of scan/report modes
* 2017-04-08 :: 0.1.0   :: First draft

## Site

https://github.com/alasdairkeyes/nagios-plugin-check_cert_ssllabs

## Author

* Alasdair Keyes - https://akeyes.co.uk/

## License

Released under GPL V3 - See included license file.
