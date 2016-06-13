Kafka
=====

Version Added:: 1.1

This role installs the [Kafka Mesos
Framework](https://github.com/mesos/kafka) and starts Kafka brokers.

Installation
------------

After a successful initial run (from your customized `sample.yml`), you
can install Kafka with

```shell
ansible-playbook -e @security.yml addons/kafka.yml
```

It can take
several minutes for all components to deploy and become healthy.

Accessing the Kafka Mesos REST API
----------------------------------

After the Kafka framework and the Kafka brokers have been successfully
started and initialized, it should be possible to access the Kafka Mesos
REST API at `/kafka` on control nodes.

Default Configuration
---------------------

The default configuration of the Kafka brokers will require at least 3
worker nodes that each have at least 4 CPUs and 4 GBs of memory
available to Mesos.

Depending on your planned environment, you may wish to customize the
sizing of your Kafka cluster using the variables documented below.

Customizing your Installation
-----------------------------

The size of your Kafka cluster is controlled by the variables documented
below.

Variables
---------

kafka\_scheduler\_name
:   The application ID of the Kafka scheduler in Marathon.
:   **default**: `"mantl/kafka"`

kafka\_service\_name
:   The name of the service that is registered in Consul when the
:   framework is deployed. This needs to match what would be derived via
:   mesos-consul. For example, when `kafka_scheduler_name` is set to
:   `mantl/kafka`, the service name should be `kafka-mantl`.
:   **default**: `"kafka-mantl"`

kafka\_scheduler\_cpu
:   The amount of CPU to allocate to the Kafka scheduler instance (MB).
:   **default**: `0.2`

kafka\_scheduler\_mem
:   The amount of memory to allocate to the Kafka scheduler instance (MB).
:   **default**: `512`

kafka\_broker\_count
:   The number of Kafka brokers to start.
:   **default**: `3`

kafka\_broker\_cpu
:   The amount of CPU to allocate to each Kafka broker.
:   **default**: `4`

kafka\_broker\_mem
:   The amount of memory to allocate to each Kafka broker (MB).
:   **default**: `4096`

kafka\_broker\_heap
:   The amount of heap to allocate to each Kafka broker (MB).
:   **default**: `4096`

kafka\_broker\_port
:   The port to bind to for the Kafka brokers.
:   **default**: `9092`

kafka\_broker\_options
:   The Kafka options to pass to the brokers.
:   **default**:
:   ```
    "log.flush.interval.ms=10000,num.recovery.threads.per.data.dir=1,delete.topic.enable=true,log.index.size.max.bytes=10485760,num.partitions=8,num.network.threads=3,socket.request.max.bytes=104857600,log.segment.bytes=536870912,log.cleaner.enable=true,zookeeper.connection.timeout.ms=1000000,log.flush.scheduler.interval.ms=2000,log.retention.hours=72,log.flush.interval.messages=20000,log.dirs=/mantl/a/dfs-data/kafka-logs\\,/mantl/b/dfs-data/kafka-logs\\,/mantl/c/dfs-data/kafka-logs\\,/mantl/d/dfs-data/kafka-logs\\,/mantl/e/dfs-data/kafka-logs\\,/mantl/f/dfs-data/kafka-logs,log.index.interval.bytes=4096,socket.receive.buffer.bytes=1048576,min.insync.replicas=2,replica.lag.max.messages=10000000,replica.lag.time.max.ms=1000000,log.retention.check.interval.ms=3600000,message.max.bytes=20480,default.replication.factor=2,zookeeper.session.timeout.ms=500000,num.io.threads=8,auto.create.topics.enable=false,socket.send.buffer.bytes=1048576"
    ```

kafka\_broker\_jvm\_options
:   The Kafka JVM options to pass to the brokers.
:   default:
    ```
    "-Dcom.sun.management.jmxremote
    -Dcom.sun.management.jmxremote.port=9010
    -Dcom.sun.management.jmxremote.local.only=false
    -Dcom.sun.management.jmxremote.authenticate=false
    -Dcom.sun.management.jmxremote.ssl=false"
    ```
