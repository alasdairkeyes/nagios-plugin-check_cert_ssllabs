# nagios-plugin-check_cert_ssllabs
Check a website's SSL config against the SSLLabs API

## Dependencies

* ssllabs-scan tool https://github.com/ssllabs/ssllabs-scan
* Nagios::Plugin::Functions;
* Nagios::Plugin::Getopt;
* Data::Validate::Domain;


## Installation

* Copy the `check_cert_ssllabs` to your nagios plugin folder and set executable.

* If running via NRPE add a line to your `nrpe.cfg` file
```
command[check_cert_ssllabs]=/usr/lib/nagios/plugins/check_cert_ssllabs -H example.com
```

* If running via Nagios add a line to your nagios config file
```
define command{
    command_name    check_cert_ssllabs
    command_line    /usr/lib/nagios/plugin/check_cert_ssllabs -H example.com
}
define service{
        use                             generic-service         ; Name of service template to use
        host_name                       localhost
        service_description             CHECK_CERT_SSLLABS
        check_command                   check_cert_ssllabs
        }
```

## Notes

* The tool usually takes about 2-3 minutes to run at peak times. You will need to increase your nagios timeouts
* This is a check that needs to be run rarely, just to ensure your site config is optimal. Don't run more than once per week.
* Thanks to SSLlabs for their great tool https://www.ssllabs.com/ssltest/

## Examples

* Run with the --help argument to see all options
```
check_cert_ssllabs --help
```

* Check with custom warning/critical values
```
check_cert_ssllabs -H example.com -c D -w A-
```

* Check and specify path to the ssllabs-scan binary
```
check_cert_ssllabs -H example.com -s /path/to/ssllabs-scan
```

* Check cached version. If the certificate has been recently checked at ssllabs, return
the cached information
```
check_cert_ssllabs -H example.org --usecache
```

## Future Work

None planned

## Site

https://github.com/alasdairkeyes/nagios-plugin-check_cert_ssllabs

## Author

* Alasdair Keyes - https://akeyes.co.uk/

## License

Released under GPL V3 - See included license file.
