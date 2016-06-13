Mesos
=====

!!! note ""
    new in version 0.1

[Mesos](https://mesos.apache.org/) is the distributed system kernel that
manages resources across multiple nodes. When combined with marathon,
you can basically think of it as a distributed init system.

Modes
-----

Mesos can be run in one of two "modes":

-   A server mode (called "master" or "leader")
-   A client mode (called "follower" or "agent". The term "slave" is used but deprecated.)

This project prefers the "leader/follower nomenclature". In addition to
the "official" modes described below, mesos\_mode supports running both
modes on a single machine for testing or development scenarios.

### Leader

Leaders will communicate with each other via zookeeper to coordinate
which leader controls the cluster. Because of this, you can run as many
leader nodes as you like, but you should consider keeping an odd number
in the cluster to make attaining a quorum easier. A single leader node
will also work fine, but will not be highly available.

### Follower

Follower nodes need to know where the leaders are, and there can be any
number of them. You should keep the follower machines free of "heavier"
services running outside Mesos, as this will cause inaccurate resource
availability counts in the cluster.

Upgrading
---------

!!! note "file `playbooks/upgrade-mesos-marathon.yml`"
    new in version 1.0

If you are running Mantl 0.5.1, you'll need to run the
`playbooks/upgrade-mesos-marathon.yml` playbook before reprovisioning
your cluster to 1.0. The packaging format changed in the 1.0 release,
this will ensure a smooth upgrade.

Upgrades from releases prior to Mantl 0.5.1 have not been tested.

Variables
---------

You can use these variables to customize your Mesos installation.

mesos\_mode
:   Set to `leader` for leader mode, and `follower` for follower mode. Set to `mixed` to run both leader and follower on the same machine (useful for development or testing.)
:   **default**: `follower`

mesos\_log\_dir
:   **default**: `/var/log/mesos`


mesos\_work\_dir
:   **default**: `/var/run/mesos`

mesos\_leader\_port
:   **default**: `5050`

mesos\_follower\_port
:   **default**: `5051`

mesos\_leader\_cmd
:   **default**: `mesos-master`

mesos\_follower\_cmd
:   **default**: `mesos-slave`

mesos\_attributes
:   Set attributes for mesos agents. Provide these as a list to set multiple attributes.
:   **format**: `"key:value"` (as elements of a YAML list)
:   **default**: `node_id:{{ inventory_hostname }}`

mesos\_resources
:   Set resources for mesos agents. (useful for setting available ports that applications can be bound to). Provide these as a list to set multiple resources.
:   **Format**: `name(role):value` (as elements of a YAML list)
:   **default**: `ports(*):[4000-5000, 7000-8000, 9000-10000, 25000-26000, 31000-32000]`

mesos\_cluster
:   **default**: `mantl`

mesos\_zk\_hosts
:   A ZooKeeper connection string in the the `host:mesos_zk_port` format, generated from the hosts in `zookeeper_server_group`.

mesos\_zk\_dns
:   Consul DNS entries for ZooKeeper hosts.
:   **default**: `zookeeper.service.consul`

mesos\_zk\_port
:   **default**: `2181`

mesos\_zk\_chroot
:   ZooKeeper znode to use as a base for mesos data.
:   **default**: `mesos`

mesos\_credentials
:   A list of credentials to add for authentication.
:   **format**: `{ principal: "...", secret: "..." }`.
:   **default**: `[]`

mesos\_authenticate\_frameworks
:   Enable Mesos authentication for frameworks. You should set mesos\_credentials for credentials if this is set.
:   **default**: set automatically if framework credentials are present

mesos\_authenticate\_followers
:   Enable Mesos authentication from followers. If set, each follower will need mesos\_follower\_secret set in their host variables.
:   **default**: set automatically if follower credentials are present

mesos\_follower\_principal
:   The principal to use for follower authentication
:   **default**: `follower`

mesos\_follower\_secret
:   The secret to use for follower authentication
:   **default**: not set. Set this to enable follower authentication.

mesos\_logging\_level
:   The log level for Mesos. This is set for all components.
:   **default**: `WARNING`
