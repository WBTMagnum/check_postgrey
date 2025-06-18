# check_postgrey.pl

This is a Nagios plugin that checks the status of a postgrey server that has been configured to listen on a unix or TCP socket. It connects to a running postgrey server and sends an example postfix policy request, parses the result to make sure there is one and returns the appropriate NAGIOS/NRPE response.

## OPTIONS
            'mode|m=s'      => \$mode,
            'socket|s=s'    => \$postgrey_socket,
            'host|H=s'      => \$postgrey_host,
            'port|p=i'      => \$postgrey_port,

* `--mode` (`-m`) Define the usage mode ([socket]|tcp).
* `--socket` (`-s`) Set the location of the postgrey socket.
* `--host` (`-H`) Set the IP address or hostname of the postgrey service.
* `--port` (`-p`) Set the port of the postgrey service.
* `--timeout` (`-t`) Sets the timeout, defaults to 10 seconds.
* `--warning=` (`-w`) Sets the warning period for the response time
* `--critical=` (`-c`) Sets the critical period for the response time
* `--version` (`-V`) Display current version and exit
* `--help` (`-h`) Display help message and exit
* `--man` Display man page and exit

## Installation

1) add the following to your local-commands file (e.g. `/etc/nagios/conf.d/local-commands.cfg`)

    ```
    # 'check_postgrey' command definition
    define command{
      command_name check_postgrey
      command_line $USER1$/check_postgrey.pl -w $ARG1$ -c $ARG2$
    }
    ```
    Note: If your postgrey unix socket is anywhere other than `/var/spool/postfix/postgrey/socket` you will either need to modify the source or add the `-s` argument to specify where it can be found.

2) Copy `check_postgrey.pl` to where your NAGIOS plugins are installed on the host that will be checked and set appropriate access permissions: e.g. `cp check_postgrey.pl /usr/lib64/nagios/plugins/` or `cp check_postgrey.pl /usr/lib/nagios/plugins/`

    and
    `chmod +x /usr/lib64/nagios/plugins/check_postgrey.pl`
    or
    `chmod +x /usr/lib64/nagios/plugins/check_postgrey.pl`

3) Add the following (adjust parameters to your own requirements) to each host specification that is to be checked:
  
    ```
    define service{
      use local-service
      host_name
      service_description Mail: Postgrey mail policy
      check_command check_postgrey!3!9
    }
    ```
