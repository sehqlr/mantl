etcd
====

Version Added:: 0.4

[etcd](https://github.com/coreos/etcd) is used in the project by calico
to distribute information about workloads, endpoints, and policy to each
Docker host. It's run in a Docker container on each host and managed by
systemd.

Variables
---------

You can use these variables to customize your etcd installation. Beware,
you will need to update `ETCD_AUTHORITY` in the Calico role as well.

> etcd\_client\_port
>
> > Port for etcd client communication
> >
> > Default: `2379`
>
> etcd\_peer\_port
>
> > Port for etcd server-to-server communication
> >
> > Default: `2380`
