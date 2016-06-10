Collectd
========

Collectd role for deploying Collectd.

Installation
------------

As of 1.1.0, Collectd is distributed as an addon for Mantl. After a
successful initial run from your customized `sample.yml`, install it
with `ansible-playbook -e @security.yml addons/collectd.yml`.

Variables
---------

This role has the following global settings:

> Hostname
>
> > Hostname to append to metrics
> >
> > Default: `{{ inventory_hostname }}`
>
> Interval
>
> > Global interval for sampling and sending metrics
> >
> > Default: `10 seconds`

This role enables the following Collectd plugins and settings:

> cpu
>
> > Type: read Description: amount of time spent by the CPU in various
> > states
>
> disk
>
> > Type: read Description: performance statistics for block devices and
> > partitions
>
> df
>
> > Type: read Description : file system usage information Default:
> > exclude all system and obsure file system types
>
> interface
>
> > Type: read Description: network interface throughput, packets/s,
> > errors
>
> load
>
> > Type: read Description: system load
>
> memory
>
> > Type: read Description: physical memory utilization
>
> processes
>
> > Type: read Description: number of processes grouped by state
>
> swap
>
> > Type: read Description: amount of memory currently written to swap
> > disk
>
> uptime
>
> > Type: read Description: system uptime
>
> users
>
> > Type: read Description: counts the number of users currently logged
> > into the system
>
> network
>
> > Type: write Description: send metrics to collectd compatible
> > receiver Default: `Server "localhost" "25826"`
>
> syslog
>
> > Type: write Description: write collectd logs to syslog Default:
> > `LogLevel "err"`
