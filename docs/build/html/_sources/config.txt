Configuration
=============

Configuration is normally handled through Metlog's configuration
system using INI configuration files. A psutil plugin must use the
`metlog_psutils.psutil_plugin:config_plugin` as the provider of the
plugin.  The suffix of the configuration section name is used to
set the method name on the Metlog client. Any part after
`metlog_plugin_` will be used as the method name.

In the following example, we will bind a method `procinfo` into the
Metlog client where we will allow network messages to be sent to
the Metlog server. ::

    [metlog_plugin_procinfo]
    provider=metlog_psutils.psutil_plugin:config_plugin
    net=True

Currently supported details options are:

    * net - details for each network connection
    * io - counters for bytes read, written and the # of syscalls
    * cpu - CPU time used by user space and kernel
    * mem - memory usage for RSS and VMS
    * threads - CPU usage for user/system per thread

Usage
=====

Logging your process details involves telling the plugin which details
you would like to log.  For each type of detail you would like to log,
you must *explicitly* tell the logger that you would like that
information.  This is done to allow the suppression of log details
through the configuration file.

Using the above example, the following snippet will log network
details. ::

    from metlog.decorators.base import CLIENT_WRAPPER
    client = CLIENT_WRAPPER.client
    client.procinfo(net=True)

The call to procinfo will send network details to the backend
Metlog server. The transmission of those network details
can be entirely suppressed through the configuration file. This is
useful in cases where collecting data is not useful due to
excessive logging or if the logs are simply not useful.  An example
with network messages disabled is illustrated below. ::

    [metlog_plugin_procinfo]
    provider=metlog_psutils.psutil_plugin:config_plugin
    net=False
