Logstash
========

Logging role for deploying and managing [Logstash](http://logstash.net)
with Docker and systemd as a part of the ELK stack.

Variables
---------

You can use these variables to customize your Logstash installations:

> logstash\_output\_stdout
>
> > A simple output which prints to the STDOUT
> >
> > Default: false
>
> logstash\_output\_elasticsearch
>
> > Configures an elasticsearch output for the local Logstash agent on
> > all nodes. You can set this variable to a dictionary that maps to
> > the [Logstash Elasticsearch
> > output](https://www.elastic.co/guide/en/logstash/1.5/plugins-outputs-elasticsearch.html)
> > settings.
> >
> > This is not needed if you are planning to use the Mantl ELK addon.
> > Use this if you want to send Logstash data to an Elasticsearch
> > cluster that is not managed by Mantl.
> >
> > Default: n/a
> >
> > Example:

``` {.sourceCode .yaml}
logstash_output_elasticsearch:
  host: "elasticsearch.example.com"
  protocol: "http"
```

> logstash\_output\_kafka
>
> > Configures an elasticsearch output for a Kafka cluster. You can set
> > this variable to a dictionary that maps to the [Logstash Kafka
> > output](https://www.elastic.co/guide/en/logstash/1.5/plugins-outputs-kafka.html)
> > settings.
> >
> > Default: n/a
> >
> > Example:

``` {.sourceCode .yaml}
logstash_output_kafka:
  broker_list: "broker1:port,broker2:port,..."
  topic_id: "logstash"
```

> logstash\_input\_log4j
>
> > Read events over a TCP socket from a Log4j SocketAppender
> >
> > Default: false
>
> logstsh\_log4j\_port
>
> > TCP port
> >
> > Default: 4560
