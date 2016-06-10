ZooKeeper
=========

Version Added:: 0.1

[ZooKeeper](https://zookeeper.apache.org/) is used for coordination
among Mesos and Marathon nodes. Rather than storing things in this
service yourself, you should prefer consul.

Variables
---------

You can use these variables to customize your ZooKepeer installation.

> zk\_id
>
> > The value of `zk_id` in the ZooKeeper configuration file. If not
> > provided it will be set by the playbook.
>
> zookeeper\_service
>
> > default: zookeeper
>
> zookeeper\_env
>
> > default: `dev`
>
> zookeeper\_ensemble
>
> > default: `mantl`
>
> zookeeper\_container\_name
>
> > The name that will be used for the container at runtime. Generated
> > automatically from zookeeper\_service, zookeeper\_env,
> > zookeeper\_ensemble, and zk\_id if not set.
>
> zookeeper\_data\_volume
>
> > The name of the data volume to store state in. Generated
> > automatically from zookeeper\_env, zookeeper\_ensemble, and zk\_id
> > if not set.
>
> zookeeper\_docker\_image
>
> > default: `asteris/zookeeper`
>
> zookeeper\_docker\_tag
>
> > default: `latest`
>
> zookeeper\_docker\_ports
>
> > Port arguments, which will be passed directly to docker. Opens TCP
> > 2181, 2888, and 3888 by default.
> >
> > default: `"-p 2181:2181 -p 2888:2888 -p 3888:3888"`
>
> zookeeper\_docker\_env
>
> > default: `"/etc/default/{{ zookeeper_service }}"`
>
> zookeeper\_log\_threshold
>
> > Log level for ZooKeeper
> >
> > default: `WARN`
>
> > zookeeper\_log\_retain\_count
> >
> > > Number of zookeeper transaction logs and snapshots to keep.
> > >
> > > default: `3`
>
> zookeeper\_log\_purge\_interval
>
> > Interval in hours that zookeeper waits to purge transacton logs and
> > snapshots.
> >
> > default: `12`
