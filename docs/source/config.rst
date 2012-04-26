Configuration
=============

Configuration is normally handled through Metlog's configuration
system using INI configuration files. A psutil plugin must use the
`metlog_cef.cef_plugin:config_plugin` as the provider of the
plugin.  The suffix of the configuration section name is used to
set the method name on the Metlog client. Any part after
`metlog_plugin_` will be used as the method name.

In the following example, we will bind a method `cef` into the
Metlog client where we will allow network messages to be sent to
the Metlog server. ::

    [metlog_plugin_cef]
    provider=metlog_cef.cef_plugin:config_plugin

The CEF plugin requires no other options to be specified.

Usage
=====

Logging CEF records is similar to using the raw CEF library.

Instead of using `log_cef`, simply use the `cef` method of the metlog client.
If you have existing code that uses `log_cef`, your code will change
from this ::

    from cef import log_cef, AUTH_FAILURE

    ...

    log_cef("Authentication attemped without username", 5,
            request.environ, request.registry.settings,
            "", signature=AUTH_FAILURE)

to this ::

    from metlog.decorators.base import CLIENT_WRAPPER
    from metlog.plugins import namespace as ns

    ...

    client = CLIENT_WRAPPER.client
    client.cef("Authentication attemped without username", 5,
            request.environ, request.registry.settings,
            "", signature=ns.cef.AUTH_FAILURE)

Note that the CEF plugin has exported important constants into a
registry.

Constants exported are:

    # TODO: list exported stuff here
