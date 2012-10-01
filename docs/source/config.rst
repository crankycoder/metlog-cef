Configuration
=============

Configuration is normally handled through Metlog's configuration
system using INI configuration files. A CEF plugin must use the
`metlog_cef.cef_plugin:config_plugin` as the provider of the
plugin.  The suffix of the configuration section name is used to
set the method name on the Metlog client. Any part after
`metlog_plugin_` will be used as the method name.

In the following example, we will bind a method `cef` into the
Metlog client where we will allow network messages to be sent to
the Metlog server. ::

    [metlog_plugin_cef]
    provider=metlog_cef.cef_plugin:config_plugin

The CEF plugin provides some optional configuration settings for 
setting the syslog options, syslog facility, syslog ident and syslog
priority.

By default, the syslog facility will be set to LOCAL4.

Valid facility settings are :

  * KERN
  * USER
  * MAIL
  * DAEMON
  * AUTH
  * LPR
  * NEWS
  * UUCP
  * CRON
  * LOCAL0
  * LOCAL1
  * LOCAL2
  * LOCAL3
  * LOCAL4
  * LOCAL5
  * LOCAL6
  * LOCAL7

Valid priority settings are :

  * EMERG
  * ALERT
  * CRIT
  * ERR
  * WARNING
  * NOTICE
  * INFO
  * DEBUG

Syslog options can be set using a comma delimited list with the
following options :

  * PID
  * CONS
  * NDELAY
  * NOWAIT
  * LOG_PERROR

Here is one sample configuration demonstrating using all available
configuration keys ::

    [metlog_plugin_cef]
    provider=metlog_cef.cef_plugin:config_plugin
    syslog_options=NDELAY,PID
    syslog_facility=KERN
    syslog_ident=my_funny_app
    syslog_priority=EMERG

Usage
=====

Obtaining a client can be done in multiple ways, please refer to the
metlog documentation for complete details.

That said, if you are impatient you can obtain a client using
`get_client`.  We strongly suggest you do not do this though. ::

    from metlog.holder import get_client

Logging CEF records is similar to using the raw CEF library.
Constants from the `cef` library have been exported in the `metlog_cef` module.

For existing code that uses the `cef` library, you will use the `cef`
method of the metlog client.  Your code will change from this ::

    from cef import log_cef, AUTH_FAILURE

    ...

    log_cef("Authentication attemped without username", 5,
            request.environ, request.registry.settings,
            "", signature=AUTH_FAILURE)

to this ::

    from metlog.holder impot get_client
    import metlog_cef

    ...

    client = get_client('metlog_cef')
    client.cef("Authentication attemped without username", 5,
            request.environ, request.registry.settings,
            "", signature=metlog_cef.AUTH_FAILURE)

Note that the CEF plugin has exported important constants into the
`metlog_cef` module.

Constants exported are:

- AUTH_FAILURE
- CAPTCHA_FAILURE
- OVERRIDE_FAILURE
- ACCOUNT_LOCKED
- PASSWD_RESET_CLR

See the `cef <http://pypi.python.org/pypi/cef>`_ library for details on each of the constants.
