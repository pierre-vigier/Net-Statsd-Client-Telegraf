[![Build Status](https://travis-ci.org/pierre-vigier/Net-Statsd-Client-Telegraf.svg?branch=master)](https://travis-ci.org/pierre-vigier/Net-Statsd-Client-Telegraf)

# NAME

Net::StatsD::Client::Telegraf - Send data to the statsd plugin of telegraf, with support for influxDB's tagging system

# SYNOPSIS

    use Net::Statsd::Client::Telegraf;
    my $stats = Net::Statsd::Telegraf->new(prefix => "service.frobnitzer.", tags => { job => "my_program" } );
    $stats->increment("requests", tags => { foo => "bar" }, sample_rate => 0.2 );
    my $timer = $stats->timer("request_duration");
    # ... do something expensive ...
    my $elapsed_ms = $timer->finish;

# DESCRIPTION

Net::StatsD::Client::Telegraf is a tiny layer on top of [Net::StatsD::Client](https://metacpan.org/pod/Net::StatsD::Client) to add support for
tags as implemented by the statsd collector of telegraf.
Defaults tags can be passed as a hashref in the tags attributes, each methods also accept a tags
arguments to add tags for the current statsd call

# ATTRIBUTES

Net::Statsd::Client::Telegraf takes the exact same parameters as [Net::StatsD::Client](https://metacpan.org/pod/Net::StatsD::Client), plus an additionnal tags,
for the documentation of the other attribute of the constructor, please refer to
[Net::StatsD::Client](https://metacpan.org/pod/Net::StatsD::Client)

## tags

**Optional:** A hashref containing key values to be used as tags in the metrics name, as accepted by influxDB and telegraf

# METHODS

Being a layer on top of [Net::Statsd::Client](https://metacpan.org/pod/Net::Statsd::Client), Net::Statsd::Client::Telegraf exposes the exact same methods,
the only difference begin in the parameters, for all of its methods, [Net::Statsd::Client](https://metacpan.org/pod/Net::Statsd::Client) accepts an optional
parameter `sample_rate`, instead Net::Statsd::Client::Telegraf accept a lsit of options in key value format
The key being `sample_rate` and `tags`.

     $stats->increment(
       "metric_name",
       tags => {
         name1 => "value1",
         name2 => "value2",
       },
       sample_rate => 0.2
    );

## $stats->increment($metric, \[%options\] )

Increment the named counter metric.

## $stats->decrement($metric, \[%options\] )

Decrement the named counter metric.

## $stats->update($metric, $count, \[%options\] )

Add `$count` to the value of the named counter metric.

## $stats->timing\_ms($metric, $time, \[%options\] )

Record an event of duration `$time` milliseconds for the named timing metric.

## $stats->timer($metric, \[%options\] )

Returns a [Net::Statsd::Client::Timer](https://metacpan.org/pod/Net::Statsd::Client::Timer) object for the named timing metric.
The timer begins when you call this method, and ends when you call `finish`
on the timer.  Calling `finish` on the timer returns the elapsed time in
milliseconds.

## $stats->gauge($metric, $value, \[%options\] )

Send a value for the named gauge metric. Instead of adding up like counters
or producing a large number of quantiles like timings, gauges simply take
the last value sent in any time period, and don't require scaling.

## $statsd->set\_add($metric, $value, \[%options\] )

Add a value to the named set metric. Sets count the number of \*unique\*
values they see in each time period, letting you estimate, for example, the
number of users using a site at a time by adding their userids to a set each
time they load a page.

# SEE ALSO

This module is a tiny layer on top of [Net::Stats::Client](https://metacpan.org/pod/Net::Stats::Client), to add the tags functionality

# AUTHOR

Pierre VIGIER <pierre.vigier@gmail.com>

# COPYRIGHT

Copyright 2018- Pierre VIGIER

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
