<<<<<<< HEAD
# chef-logstash cookbook

Basic Logstash cookbook for Chef that installs Logstash from the official [package repositories](http://logstash.net/docs/1.4.1/repositories).

# Requirements

Tested on Ubuntu/Debian but should work on other platforms as well.

# Usage

Just include the default recipe `logstash::default`.

## Server

Put the following in your logstash server attributes:

```json
{
    "logstash": {
        "version": "1.4",

        "server": {
            "enabled": true,
            "inputs": [
                {
                    "rabbitmq": {
                        "exchange": "rawlogs",
                        "user": "username",
                        "password": "verysecret",
                        "host": "rabbitmq.example.com",
                        "type": "all"
                    }
                }
            ],
            "outputs": [
                {
                    "elasticsearch": {
                        "host": "elasticsearch.example.com",
                        "cluster": "logstash"
                    }
                }
            ]
        },

        "agent": {
            "inputs": [
                {
                    "file": {
                        "path": ["/var/log/*.log"],
                        "exclude": ["*.gz"],
                        "type": "syslog"
                    }
                }
            ],

            "outputs": [
                {
                    "rabbitmq": {
                        "host": "rabbitmq.example.com",
                        "port": "5672",
                        "user": "username",
                        "password": "verysecret",
                        "exchange": "logstash",
                        "exchange_type": "fanout"
                    }
                }
            ]
        }
    }
}

```

## Agent

And put this in your agents' attributes:


```json
{
    "logstash": {
        "version": "1.4",

        "agent": {
            "inputs": [
                {
                    file {
                        "path": ["/var/log/*.log"],
                        "exclude": ["*.gz"],
                        "type": "syslog"
                    }
                }
            ],

            "outputs": [
                {
                    "rabbitmq": {
                        "host": "rabbitmq.example.com",
                        "port": "5672",
                        "user": "username",
                        "password": "verysecret",
                        "exchange": "logstash",
                        "exchange_type": "fanout"
                    }
                }
            ]
        }
    }
}
```

# Attributes

## Default

```ruby
node[:logstash][:version] = "1.4"            # The version to install.
```

## Agent

```ruby
node[:logstash][:agent][:enabled] = true    # Enable agent.
node[:logstash][:agent][:inputs]  = []      # Inputs configuration, see http://logstash.net/docs/1.4.1/
node[:logstash][:agent][:outputs] = []      # Outputs configuration, see http://logstash.net/docs/1.4.1/
```

## Server

```ruby
node[:logstash][:server][:enabled] = false   # Enable server (default: false, agent only).
node[:logstash][:server][:inputs]  = []      # Inputs configuration, see http://logstash.net/docs/1.4.1/
node[:logstash][:server][:outputs] = []      # Outputs configuration, see http://logstash.net/docs/1.4.1/
```

# Recipes

## Default

Meta recipe to install the packages and configure the server and/or agent.

## Apt

Installs Logstash from the Debian packages.

## Yumrepo

Installs Logstash from the Yum repo packages.

## Agent

Configures the agent.

## Server

Configures the server.

# Thanks to

- https://github.com/lusis/chef-logstash where I borrowed the idea and even some code from.


# Author

Author:: Wouter de Vos - [Springest](http://www.springest.com) (oss@rein.io)
=======
# Logstash

Logstash is a tool for managing events and logs. You can use it to collect
logs, parse them, and store them for later use (like, for searching). Speaking
of searching, Logstash comes with a web interface for searching and drilling
into all of your logs.

It is fully free and fully open source. The license is Apache 2.0, meaning you
are pretty much free to use it however you want in whatever way.

For more info, see <http://logstash.net/>

## logstash-contrib
### AKA "Where'd that plugin go??"

Since version 1.4.0 of Logstash, some of the community-contributed plugins were
moved to a new home in the
[Elasticsearch logstash-contrib repo](https://github.com/elasticsearch/logstash-contrib).
If you can't find a plugin here which you've previously used, odds are it is now
located there. The good news is that these plugins are simple to install using the
[Logstash manual plugin installation script](http://logstash.net/docs/latest/contrib-plugins).

## Need Help?

Need help? Try #logstash on freenode irc or the logstash-users@googlegroups.com
mailing list.

You can also find documentation on the <http://logstash.net> site.

## Developing

If you don't have JRuby already (or don't use rvm, rbenv, etc), you can have `bin/logstash` fetch it for you by setting `USE_JRUBY`:

    USE_JRUBY=1 bin/logstash ...

Otherwise, here's how to get started with rvm:

    # Install JRuby with rvm
    rvm install jruby-1.7.11
    rvm use jruby-1.7.11

Now install dependencies:

    # Install logstash ruby dependencies
    bin/logstash deps

Other commands:

    # to use Logstash gems or libraries in irb, use the following
    # this gets you an 'irb' shell with Logstash's environment
    bin/logstash irb

    # Run Logstash
    bin/logstash agent [options]

    # If running bin/logstash agent yields complaints about log4j/other things
    # This will download the elasticsearch jars so Logstash can use them.
    make vendor-elasticsearch

## Testing

There are a few ways to run the tests. For development, using `bin/logstash
rspec <some spec>` will suffice:

    % bin/logstash rspec spec/filters/grok.rb
    ...................

    Finished in 0.123 seconds
    19 examples, 0 failures

Alternately, if you have just built the tarball, you can run the tests
specifically on those like so:

    make tarball-test

If you want to run all the tests from source, do:

    make test

## Building

Building is not required. You are highly recommended to download the releases
we provide from the Logstash site!

If you want to build the release tarball yourself, run:

    make tarball

You can build rpms and debs, if you need those. Building rpms requires you have [fpm](https://github.com/jordansissel/fpm), then do this:

    make package

## Project Principles

* Community: If a newbie has a bad time, it's a bug.
* Software: Make it work, then make it right, then make it fast.
* Technology: If it doesn't do a thing today, we can make it do it tomorrow.

## Contributing

All contributions are welcome: ideas, patches, documentation, bug reports,
complaints, and even something you drew up on a napkin.

Programming is not a required skill. Whatever you've seen about open source and
maintainers or community members  saying "send patches or die" - you will not
see that here.

It is more important to me that you are able to contribute.

For more information about contributing, see the
[CONTRIBUTING](CONTRIBUTING.md) file.
>>>>>>> ccb453d557065425c530ddb413219b29ef31647f
