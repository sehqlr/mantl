ELK
===

!!! note ""
	new in version 1.0

The ELK role combines Elasticsearch, Logstash, and Kibana to provide
automatic log shipping and metrics collection from all Mantl nodes to an
Elasticsearch cluster. Kibana is available to visualize and analyze this
data.

This role runs an Elasticsearch cluster via the [Elasticsearch Mesos
Framework](https://github.com/mesos/elasticsearch). It also configures
Logstash on all nodes to forward logs to that cluster. Finally, Kibana
is run via the [Kibana Mesos
Framework](https://github.com/mesos/kibana). It is configured to talk to
an Elasticsearch client node (which acts as a smart load balancer for
the Elasticsearch cluster) and includes a default sample dashboard.

Installation
------------

As of 1.0, the ELK stack is distributed as an addon for Mantl. After a
successful initial run (from your customized `sample.yml`), install it
with `ansible-playbook -e @security.yml addons/elk.yml`. It can take
several minutes for all components to deploy and become healthy.

Accessing User Interfaces
-------------------------

After the Elasticsearch framework and the Kibana application have been
successfully installed and initialized, it should be possible to access
their corresponding user interfaces directly from Mantl UI.

Default Configuration
---------------------

The default configuration of the ELK stack will require at least 4
worker nodes, each having at least 1 full CPU and 1 GB of memory
available to Mesos. In addition, each worker node will need to have at
least 5 GBs of free disk space.

While a cluster of this size will be sufficient to evaluate and test the
ELK stack on Mantl, we encourage you to review the configuration
variables below to size the cluster as appropriate for your environment.

Customizing your Installation
-----------------------------

The size of your elasticsearch cluster is controlled by the variables
documented below. As an example, let's say that you just wanted to stand
up an ELK stack on a small cluster for evaluation purposes. You only
want to run a single node since you are not worried about high
availability or data safety in this scenario. To do this, create a new
yaml file (`elasticsearch.yml`, for example) that looks something like
this:

```yaml
---
elasticsearch_ram: 512
elasticsearch_executor_ram: 512
elasticsearch_cpu: 0.5
elasticsearch_executor_cpu: 0.5
elasticsearch_nodes: 1
```

In this example, we are configuring both the Elasticsearch framework
scheduler and the Elasticsearch nodes (executors) to each use 512 MB of
memory and a half a CPU each. We are also indicating that we only want a
single Elasticsearch node launched in the cluster.

When you install the ELK addon, you can tell ansible to use this yaml
file to configure your installation:

```shell
ansible-playbook -e @security.yml -e @elasticsearch.yml addons/elk.yml
```

With this configuration, Kibana and the Elasticsearch client node will
still be deployed with their default configurations. Of course, you can
customize further as needed.

Kibana deployment
-----------------

By default, Kibana will be run via the Kibana Mesos framework. It is
also possible to run Kibana on Marathon. You can control this by setting
the `kibana_package` variable. Set it to `kibana` to run Kibana via
Marathon and `kibana-mesos` (the default) to run it via the Mesos
framework.

Uninstalling the ELK Addon
--------------------------

As of 1.1, we now distribute a playbook that can be used to uninstall
the addon:

```shell
ansible-playbook -e @security.yml addons/elk-uninstall.yml
```

This will remove the Elasticsearch framework, the Elasticsearch client
node, and Kibana from your cluster. By default, the Elasticsearch data
directories will not be removed. If you do not need to preserve your
Elasticsearch data, you can set the `elasticsearch_remove_data` variable
to true when you run the uninstall playbook:

```shell
ansible-playbook -e @security.yml -e 'elasticsearch_remove_data=true' addons/elk-uninstall.yml
```

Upgrading
---------

You do not need to re-install the addon on an existing pre-1.1 Mantl
cluster that is already running the ELK addon. The existing addon should
continue running fine on 1.1. If you do wish to switch to the updated
addon, you should uninstall the Elasticsearch framework and disable
Kibana on your control nodes (see the 1.0.3 uninstall instructions
below) prior to re-installing the addon. It will be up to you to backup
and migrate your Elasticsearch data in this scenario.

Uninstalling the Elasticsearch Framework (1.0.3)
------------------------------------------------

Uninstalling the Elasticsearch framework involves several steps. Below
are examples of the commands that you can run to completely remove the
framework from your cluster. You will need to adjust the `creds`, `url`,
and `control_node` variables to values that are applicable to your
cluster. You will also need to have the
[jq](https://stedolan.github.io/jq/) utility installed to follow this
example.

```shell
export creds='admin:password'
export url=https://mantl-control-01
export control_node=mantl-control-01

# remove scheduler from marathon
curl -sku $creds -XDELETE $url/marathon/v2/apps/elasticsearch

# find the mesos framework id
frameworkId=$(curl -sku $creds $url/api/1/frameworks | jq -r '.[] | select(.name == "elasticsearch") | .id')

# remove the mesos framework
curl -sku $creds -XDELETE $url/api/1/frameworks/$frameworkId

# clean up mesos framework state from zookeeper
ansible $control_node -s -m shell -a 'zookeepercli -servers zookeeper.service.consul -force -c rmr /elasticsearch'

# delete all elasticsearch data (optional)
ansible 'role=worker' -s -m shell -a 'rm -rf /data'
```

Uninstalling Kibana (1.0.3)
---------------------------

On Mantl 1.0.3, we do not have an uninstall process for Kibana. However,
it is easy to disable it on your cluster. The following commands can be
run to disable Kibana:

```shell
ansible 'role=control' -s -m shell -a 'consul-cli service-deregister kibana'
ansible 'role=control' -s -m shell -a 'rm /etc/consul/kibana.json'
ansible 'role=control' -s -m service -a 'name=kibana enabled=no state=stopped'
```

Variables
---------

elasticsearch\_ram
:   The amount of memory to allocate to the Elasticsearch scheduler instance (MB).
:   **default**: `1024`

elasticsearch\_executor\_ram
:   The amount of memory to allocate to each Elasticsearch executor instance (MB).
:   **default**: `1024`

elasticsearch\_disk
:   The amount of Disk resource to allocate to each Elasticsearch executor instance (MB).
:   **default**: `5120`

elasticsearch\_cpu
:   The amount of CPU resources to allocate to the Elasticsearch scheduler.
:   **default**: `1.0`

elasticsearch\_executor\_cpu
:   The amount of CPU resources to allocate to each Elasticsearch executor instance.
:   **default**: `1.0`

elasticsearch\_nodes
:   Number of Elasticsearch executor instances.
:   **default**: `3`

elasticsearch\_cluster\_name
:   The name of the Elasticsearch cluster.
:   **default**: `"mantl"`

elasticsearch\_service
:   The name of the service that is registered in Consul when the framework is deployed. This needs to match what would be derived via mesos-consul. For example, when `elasticsearch_framework_name` is set to `mantl/elasticsearch`, the service name should be `elasticsearch-mantl`.
:   **default**: `"elasticsearch-mantl"`

elasticsearch\_executor\_name
:   The name of the executor tasks in Mesos.
:   **default**: `"elasticsearch-executor-mantl"`

elasticsearch\_framework\_version
:   The version of the Elasticsearch mesos framework.
:   **default**: `"1.0.1"`

elasticsearch\_framework\_name
:   The name of the Elasticsearch mesos framework.
:   **default**: `"mantl/elasticsearch"`

elasticsearch\_framework\_ui\_port
:   The port that the Elasticsearch framework user interface listens on.
:   **default**: `31100`

elasticsearch\_client\_id
:   The id of the elasticsearch-client application in Marathon.
:   **default**: `"mantl/elasticsearch-client"`

elasticsearch\_client\_service
:   The name of the service that is registered in Consul when the Elasticsearch client node is deployed. This needs to match what would be derived via mesos-consul. For example, when `elasticseach_client_id` is set to `mantl/elasticsearch-client`, the service name should be `elasticsearch-client-mantl`.
:   **default**: `"elasticsearch-client-mantl"`

elasticsearch\_client\_elasticsearch\_service
:   The name of the service registered in Consul for the Elasticsearch client node to connect to.
:   **default**: `"transport\_port.{{ elasticsearch\_executor\_name }}"`

elasticsearch\_client\_client\_port
:   The HTTP port for the Elasticsearch client node to listen on.
:   **default**: `9200`

elasticsearch\_client\_transport\_port
:   The transport port for the Elasticsearch client node to listen on.
:   **default**: `9300`

elasticsearch\_client\_cpu
:   The amount of CPU resources to allocate to the Elasticsearch client node.
:   **default**: `0.5`

elasticsearch\_client\_ram
:   The amount of memory to allocate to the Elasticsearch client node (MB).
:   **default**: `512`

kibana\_package
The name of the package to use for the Kibana deployment. When set to `kibana-mesos`, the Kibana Mesos framework will be used. When set to `kibana`, Kibana will deployed in a Docker container running in Marathon.
:   **default**: `kibana-mesos`

kibana\_id
:   The id of the Kibana application in Marathon (Kibana on Marathon).
:   **default**: `mantl/kibana`

kibana\_service
:   The name of the service that is registered in Consul when Kibana is deployed. This needs to match what would be derived via mesos-consul. For example, when `kibana_id` is set to `mantl/kibana`, the service name should be `kibana-mantl` (Kibana on Marathon).
:   **default**: `kibana-mantl`

kibana\_image
:   The Docker image to use for Kibana (Kibana on Marathon).
:   **default**: `ciscocloud/mantl-kibana:4.3.2`

kibana\_elasticsearch\_service
:   The name of the Elasticsearch service registered in Consul for the Kibana instance to connect to (Kibana on Marathon).
:   **default**: `"{{ elasticsearch\_client\_service }}"`

kibana\_cpu
:   The amount of CPU resources to allocate to each Kibana instance (Kibana on Marathon).
:   **default**: `0.5`

kibana\_ram
:   The amount of memory to allocate to each instance of Kibana (MB) (Kibana on Marathon).
:   **default**: `512`

kibana\_instances
:   The number of Kibana instances to run (Kibana on Marathon).
:   **default**: `1`

kibana\_mesos\_id
:   The id of the Kibana framework application in Marathon (Kibana Mesos framework).
:   **default**: `mantl/kibana`

kibana\_mesos\_framework\_name
:   The name of the Kibana Mesos framework (Kibana Mesos framework).
:   **default**: `kibana-mantl`

kibana\_mesos\_service
:   The name of the service that is registered in Consul when the Kibana framework is deployed. This needs to match what would be derived via mesos-consul. For example, when `kibana_mesos_id` is set to `mantl/kibana`, the service name should be `kibana-mantl` (Kibana Mesos framework).
:   **default**: `kibana-mantl`

kibana\_mesos\_image
:   The Docker image to use for Kibana (Kibana Mesos framework).
:   **default**: `ciscocloud/mantl-kibana:4.3.2`

kibana\_mesos\_elasticsearch\_service
:   The name of the Elasticsearch service registered in Consul for the Kibana instance to connect to (Kibana Mesos framework).
:   **default**: `"{{ elasticsearch\_client\_service }}"`

kibana\_mesos\_kibana\_service
:   The name of the Kibana service registered in Consul (Kibana Mesos framework).
:   **default**: `"{{ kibana\_mesos\_framework\_name }}-task"`

kibana\_mesos\_scheduler\_cpu
:   The amount of CPU resources to allocate to the Kibana framework scheduler (Kibana Mesos framework).
:   **default**: `0.2`

kibana\_mesos\_scheduler\_ram
:   The amount of memory to allocate to the Kibana framework scheduler (MB) (Kibana Mesos framework).
:   **default**: `256`

kibana\_mesos\_executor\_cpu
:   The amount of CPU resources to allocate to each Kibana executor instance (Kibana Mesos framework).
:   **default**: `0.5`

kibana\_mesos\_executor\_ram
:   The amount of memory to allocate to each Kibana executor instance (MB) (Kibana Mesos framework).
:   **default**: `512`

kibana\_mesos\_instances
:   The number of Kibana executors to launch (Kibana Mesos framework).
:   **default**: `1`
