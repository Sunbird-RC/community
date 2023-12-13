---
description: >-
  The following document describes how the metrics can be emitted through
  registry
---

# Metrics

Sunbird RC enables emitting events from the registry. Any operation like ADD, DELETE, UPDATE and READ can emit events

For Sunbird RC to start emitting events, there are few configurations you need to enable.

1. **event\_enabled:-** Boolean value which indicates whether the events will be emitted or not
2. **event\_topic:-** Kafka Topic to which events will be emitted if **event\_providerName** is set to **dev.sunbirdrc.registry.service.impl.KafkaEventService**
3. **event\_providerName:- dev.sunbirdrc.registry.service.impl.KafkaEventService** for Kafka based and **dev.sunbirdrc.registry.service.impl.FileEventService.java** for File based event logging

The events emitted are in the format of Sunbird telemetry specs. Following is the sample of the event that is emitted from the registry

```json
{
    "eid": "ADD", //operationType
    "ets": 1678958110872, 
    "ver": "3.1", 
    "mid": "abc-123", //message id
    "actor": {
      "id": "123", // userId of creator or anonymous
      "type":  "User"
    },
    "context": {
      "channel": "sunbird-rc-core",
      "env": "dev"
    },
    "object": {
      "id": "xyz-123",
      "type": "Student"
    },
    
    "edata": {
       "identityDetails": {
           "fullName": "kevin",
           "dob": "XX-XX-XX94",
           "identityHolder": {
               "type": "XXXXport",
               "value": "XXX741"
           }
       },
       "contactDetails": {
           "email": "XXXXom@gmail.com",
           "mobile": "XXXXXX6789",
           "address": {
               "plot": "XX",
               "street": "XXXnue"
           }
       },
       "guardianDetails": {
           "fullName": "John",
           "relation": "Father"
       }
    }
}
```

Sunbird RC supports two types in which events can be emitted.

1. [Kafka based](metrics.md#kafka-based)
2. [File based](metrics.md#file-based)

You can enable this two types by passing a configuration variable. **event\_providerName.**

The value for this will be class name. For eg: `dev.sunbirdrc.registry.service.impl.KafkaEventService` for Kafka based\
`dev.sunbirdrc.registry.service.impl.FileEventService.java` For File based

### **- Kafka based**

Registry sends the event in the above format i.e. Sunbird Telemetry format to the Kafka topic configured(default events). Kafka will hold the data and then the consumer can run on this to store this in the Clickhouse database

OperationType here can be ADD, UPDATE, DELETE, READ

Data of the entity will be masked using the configuration defined in the schema.

You can check the configuration for masking in [schema configuration](schema-setup/schema-configuration.md).

1. InternalFields [here](schema-setup/schema-configuration.md#internalfieldconfig)
2. PrivateFields [here](schema-setup/schema-configuration.md#privatefieldconfig)

**Metrics Service**

The service is a consumer for the events the registry emits into the Kafka topic.

Metric service by default uses Clickhouse as the database to store the events. The table Schema for events is as follows

```sql
create table <table_name> (
    operationType String,
    createdAt Date,
    entityId String,
    entity JSON
)
```

For every distinct type of schema, the service creates a new table and stores the data related to that object.

Metric Service also exposes two APIs

* The service also exposes an API which returns the count of all the events emitted. It retrieves the data from all the tables created. ([API Spec](../../api-reference/metrics-apis/get-count.md))
* The service also exposes another API which returns aggregates on all the tables created. The cron job will run and save the result in a Redis. This can be fetched through the API. ([API Spec](../../api-reference/metrics-apis/get-aggregates.md))\
  To configure Metrics Service for aggregates, the following configurations need to be setup for Metrics micro-service\
  \
  **CRON\_ENABLE** :- Boolean value which will tell metrics service to run cron job if the value is set to true\
  \
  **SCHEDULE\_INTERVAL**:- Interval period in days to run the cron job after\
  \
  **SCHEDULE\_TIME**:- Time in GMT at which the cron job should be running

Based on this, the cron job will run at the scheduled time and the results will be stored in redis. The aggregates API will then return back this results.

The Configuration for Metric Service can be found [here](configuration/#metrics-service)

### - File Based

The events emitted by the registry are logged into a log file which is present at `metrics_log/metrics.log` . The log also has a rolling policy based on the size (30MB).

Following is a pictorial representation of the current approach

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-15 at 3.58.29 PM.png" alt=""><figcaption><p>Interaction Diagram for Events Thrown and Metric Consuming Events</p></figcaption></figure>

The discussion for all of these features can be found here, [https://github.com/orgs/Sunbird-RC/discussions/356](https://github.com/orgs/Sunbird-RC/discussions/356)
