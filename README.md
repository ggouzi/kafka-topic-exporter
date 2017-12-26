# Kafka Topic Exporter

Consume Kafka topics and export to Prometheus

### Start process

```
java -jar target/kafka-topic-exporter-0.0.4-jar-with-dependencies.jar config/kafka-topic-exporter.sample.properties 
```

### Configuration

```
exporter.port=12340
exporter.metric.expire=120
bootstrap.servers=localhost:9092
group.id=test
# Java regex
kafka.consumer.topics=export\..*
kafka.consumer.remove.prefix=export\.aggregated_
```

* exporter.metric.expire(default: 0 (no expire))
    * When a metric (name & labels) is not updated for this time period (in second), it will be removed from exporter response.

### Record format

Each record in the topics should be the following format. `labels` are optional.

```
{"name": <metric name>,
 "value": <metric value>,
 "labels: {
    "foolabel": "foolabelvalue",
    "barlabel": "barlabelvalue"
    }
}
```

Then the following item will be exported.

```
<kafka topic name>_<metric_name>{foolabel="foolabelvalue", barlabel="barlabelvalue"} <metric value>
```
