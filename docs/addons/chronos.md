Chronos
=======

!!! note ""
    new in version 0.1

[Chronos](http://mesos.github.io/chronos/) is a distributed and
fault-tolerant scheduler that runs on top of Apache Mesos that can be
used for job orchestration. You can think of it as distributed cron
service. It supports custom Mesos executors as well as the default
command executor. By default, Chronos executes sh (on most systems bash)
scripts.

Installation
------------

As of 1.1, Chronos is distributed as an addon for Mantl. After a
successful initial build (from your customized `sample.yml`), you can
install it by running:

```shell
ansible-playbook addons/chronos.yml
```

It can take a few minutes before Chronos becomes available and healthy.

Accessing the Chronos User Interface
------------------------------------

After Chronos has been successfully installed and initialized, it should
be possible to access the user interface directly from Mantl UI.

Default Configuration
---------------------

The default configuration of the Chronos addon will require at 1 worker
node with at least 1 CPU and 1 GB of memory available.

Customizing your Installation
-----------------------------

There are a number of configuration options available for Chronos (each
documented in the Variables section below).

As an example, let's say you wanted to run 3 Chronos instances for
high-availability purposes and you wanted each to have more CPU and
memory allocated. To do this, create a new yaml file (`chronos.yml`, for
example) that looks something like this:

```yaml
---
chronos_instances: 3
chronos_cpus: 2.0
chronos_mem: 2048.0
```

When you install the Chronos addon, you can tell ansible to use this
yaml file to configure your installation:

```shell
ansible-playbook -e @chronos.yml addons/chronos.yml
```

Uninstalling the Chronos addon
------------------------------

Uninstalling the Chronos addon can be done with a single API call. For
example:

```shell
export creds='admin:password'
export url=https://mantl-control-01

# uninstall chronos framework
curl -sku $creds -XDELETE -d "{\"name\": \"chronos\"}" $url/api/1/install
```

You will need to adjust the `creds` and `url` variables with values that
are applicable to your cluster.

Upgrading from 1.0
------------------

If you are upgrading from a Mantl 1.0 cluster that is already running
Chronos, there is actually little reason to switch over to the addon
version that runs in Marathon. Feel free to continue using your existing
Chronos installation. However, if for some reason you want to switch,
you can run the following steps to disable the existing Chronos install.

!!! warning "Scheduled Tasks"
	Please note that you will need to recreate any tasks you already have
	scheduled in Chronos. They will not be automatically migrated.

		ansible 'role=control' -s -m shell -a 'consul-cli service-deregister chronos'
		ansible 'role=control' -s -m shell -a 'rm /etc/consul/chronos.json'
		ansible 'role=control' -s -m service -a 'name=chronos enabled=no state=stopped'

The new method of installing Chronos requires a version of mantl-api
later than 0.1.7. You can upgrade mantl-api manually, or run a sample
playbook from a more recent version of Mantl (after 1.0.4) to get it.
After upgrading mantl-api, you can install the addon in the usual way:

```shell
ansible-playbook addons/chronos.yml
```

Variables
---------

chronos\_cassandra\_port
:   Port for Cassandra.
:   **default**: `9042`

chronos\_cassandra\_ttl
:   TTL for records written to Cassandra.
:   **default**: `31536000`

chronos\_cpus
:   CPU shares to allocate to each Chronos instance.
:   **default**: `1.0`

chronos\_instances
:   Number of Chronos instances to run.
:   **default**: `1`

chronos\_decline\_offer\_duration
:   The duration (milliseconds) for which to decline offers by **default**.
:   **default**: `5000`

chronos\_disable\_after\_failures
:   Disables a job after this many failures have occurred.
:   **default**: `0`

chronos\_failover\_timeout
:   The failover timeout in seconds for Mesos.
:   **default**: `604800`

chronos\_failure\_retry
:   Number of ms between retries.
:   **default**: `60000`

chronos\_framework\_name
:   The framework name.
:   **default**: `"chronos"`

chronos\_graphite\_reporting\_interval
:   Graphite reporting interval (seconds).
:   **default**: `60`

chronos\_hostname
:   The advertised hostname stored in ZooKeeper so another standby host can redirect to this elected leader.
:   **default**: `"\$HOST"`

chronos\_id
:   Unique identifier for the app consisting of a series of names separated by slashes.
:   **default**: `"/chronos"`

chronos\_mem
:   Memory (MB) to allocate to each Chronos instance.
:   **default**: `1024.0`

chronos\_mesos\_task\_cpu
:   Number of CPUs to request from Mesos for each task.
:   **default**: `0.1`

chronos\_mesos\_task\_disk
:   Amount of disk capacity to request from Mesos for each task (MB).
:   **default**: `256.0`

chronos\_mesos\_task\_mem
:   Amount of memory to request from Mesos for each task (MB).
:   **default**: `128.0`

chronos\_min\_revive\_offers\_interval
:   Do not ask for all offers (also already seen ones) more often than this interval (ms).
:   **default**: `5000`

chronos\_reconciliation\_interval
:   Reconciliation interval in seconds.
:   **default**: `600`

chronos\_revive\_offers\_for\_new\_jobs
:   Whether to call reviveOffers for new or changed jobs.
:   **default**: `false`

chronos\_schedule\_horizon
:   The look-ahead time for scheduling tasks in seconds.
:   **default**: `60`

chronos\_task\_epsilon
:   The **default** epsilon value for tasks, in seconds.
:   **default**: `60`

chronos\_zk\_timeout
:   The timeout for ZooKeeper in milliseconds.
:   **default**: `10000`
