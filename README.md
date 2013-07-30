logshipper
==========

Ship log files into Redis for subsequent consumption by LogStash.

Logshipper reads every line of a file (or stdin), packages it up as
a LogStash JSON event object, and pushes it onto the named Redis
list. From there, a logstash consumer process can pull, process, and
store into the configured backing store.


Command Line Options
--------------------

- *debug*	turn on debugging (optional)
- *host*	redis server host
- *port*	redis server port (optional, default is 6379)
- *db*	redis database number, (optional, default 0)
- *key*	redis key into which events are RPUSH'd
- *type*	set logstash "type" field for events (optional, no default)
- *tags*	CSV list of tags (optional)
- *field*	add a field as foo=bar (optional, may be repeated)
- *source_host*	override source host name (optional)


Example
-------

    logshipper --host=logstasher --key=fluffy --type=apache access.log

LogStash Configuration
----------------------

With the above example command line, a suitable LogStash input section
would be:

    input {
      redis {
        host => "127.0.0.1"
        # these settings should match the output of the agent
        data_type => "list"
        key => "fluffy"
        # We use json_event here since the sender is a logstash agent
        format => "json_event"
      }
	}

Then follow that by necessary filters and outputs to handle the
"apache" event type, as we set in the logshipper.


Requirements
------------

This program requires the following Perl modules to be installed. The
only other dependency is a recent version of perl, 5.10 should be
sufficient. It has only been tested on 5.16 and newer.

* JSON
* Redis
* DateTime


Copyright
---------

This program is copyright Vivek Khera. You may use it for whatever
purposes you wish under the Artistic License (Perl license).
