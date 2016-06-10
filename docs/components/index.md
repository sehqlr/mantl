Components
==========

Mantl is made up of a number of components which can be customized,
generally using Ansible variables.

TOC

:   

    maxdepth

    :   1

    calico.rst chronos.rst collectd.rst consul.rst distributive.rst
    dnsmasq.rst docker.rst elk.rst etcd.rst glusterfs.rst haproxy.rst
    kafka.rst kubernetes.rst logstash.rst marathon.rst mesos.rst
    traefik.rst zookeeper.rst

The project also includes a number of Ansible roles that multiple
components can use:

TOC

:   

    maxdepth

    :   1

    common.rst consul-template.rst logrotate.rst nginx.rst

The following technology previews are also included. These may be used
more fully in the future, but now just exist for preview purposes to
gather feedback and build initial implementations against:

TOC

:   

    maxdepth

    :   1

    vault.rst

Mantl includes some logic that is provided via our own packaging system,
and so is not visible in the Ansible roles. Here are the links to our
package sources:

> -   [mantl-packaging](https://github.com/asteris-llc/mantl-packaging)
> -   [mesos-packaging](https://github.com/asteris-llc/mesos-packaging)
