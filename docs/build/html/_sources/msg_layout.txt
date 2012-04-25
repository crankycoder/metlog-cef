Detailed message layout
=======================

The plugin will only send the process details that you explicitly
ask for. These details are formatted as JSON blobs that are formatted
such that a statsd message can be generated on the metlog server side.
The following is an example that creates network, cpu, io, memory and
thread statistics.

Each statsd messages is namespaced with `psutil.<group>.<hostname>.<pid>`

Note that hostnames have periods replaced with underscore characters
so that statsd and graphite will properly namespace the messages.

Open sockets are represented with a two part key.

The key is comprised of:

  * the host and port of the socket on the server side.  Note that the
    IP address has periods converted into underscores.
  * the TCP connection status

For the following examples, the hostname of the server is
`MyHostName` and the process ID of the parent process is 9973.

Open sockets
------------

An example of a server socket that is listening ::

    {'ns': 'psutil.net.MyHostName.9973',
     'key': '127_0_0_1:50007.LISTEN', 
     'rate': 1, 
     'value': 1}

This will then get serialized into a statsd message in form of: ::

    psutil.net.MyHostName.9973.127_0_0_1:50007.LISTEN|1|1


CPU time
--------

CPU information for seconds in user space, seconds in kernel space,
and percentage of CPU time used is represented as ::

    {'ns': 'psutil.cpu.MyHostName.9973', 
     'key': 'user', 
     'rate': '', 
     'value': 0.12 }

    {'ns': 'psutil.cpu.MyHostName.9973', 
     'key': 'sys', 
     'rate': '', 
     'value': 0.02 }

    {'ns': 'psutil.cpu.MyHostName.9973', 
     'key': 'pcnt', 
     'rate': '', 
     'value': 0.0 }

These are formatted into the following statsd messages: ::

    psutil.cpu.MyHostName.9973.user|0.12
    psutil.cpu.MyHostName.9973.sys|0.02
    psutil.cpu.MyHostName.9973.pcnt|0.0

I/O counters
------------

I/O metrics provide bytes read, written and the number of system calls
used for read and write operations. ::

    {'ns': 'psutil.io.MyHostName.9973', 
     'key': 'read_bytes', 
     'rate': '', 
     'value': 50} 

    {'ns': 'psutil.io.MyHostName.9973', 
     'key': 'write_bytes', 
     'rate': '', 
     'value': 200} 

    {'ns': 'psutil.io.MyHostName.9973', 
     'key': 'read_count', 
     'rate': '', 
     'value': 3115} 

    {'ns': 'psutil.io.MyHostName.9973', 
     'key': 'write_count', 
     'rate': '', 
     'value': 5434} 

This will then get serialized into a statsd message in form of: ::

    psutil.io.MyHostName.9973.read_bytes|50
    psutil.io.MyHostName.9973.write_bytes|200
    psutil.io.MyHostName.9973.write_count|3115
    psutil.io.MyHostName.9973.write_bytes|5434

Memory Usage
------------

Memory stats provide percentage of memory used as well as RSS and VMS
usage. ::

    {'ns': 'psutil.meminfo.MyHostName.9973', 
     'key': 'pcnt', 
     'rate': '', 
     'value': 2.193876346582101}

    {'ns': 'psutil.meminfo.MyHostName.9973', 
     'key': 'rss', 
     'rate': '', 
     'value': 11415552}

    {'ns': 'psutil.meminfo.MyHostName.9973', 
     'key': 'vms', 
     'rate': '', 
     'value': 52461568}

This will then get serialized into a statsd message in form of: ::

    psutil.meminfo.MyHostName.9973.pcnt|2.193876346582101
    psutil.meminfo.MyHostName.9973.rss|11415552
    psutil.meminfo.MyHostName.9973.vms|52461568

Thread level CPU usage
----------------------

Thread level CPU usage adds the thread id as a prefix to the key. 
statsd is provided with CPU usage for user space and kernel space in
seconds.  The key is prefixed with the thread id so that statistics
per thread per process can be monitored. In the following example, CPU
stats for thread 17177 are monitored.  ::

    {'ns': 'psutil.thread.MyHostName.9973',
     'key': '17177.sys',
     'rate': '',
     'value': 0.02}

    {'ns': 'psutil.thread.MyHostName.9973',
     'key': '17177.user',
     'rate': '',
     'value': 0.13}

This will then get serialized into a statsd message in form of: ::

    psutil.thread.MyHostName.9973.17177.sys|0.02
    psutil.thread.MyHostName.9973.17177.user|0.13
