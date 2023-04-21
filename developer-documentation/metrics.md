---
description: >-
  The following document describes how the metrics can be emitted through
  registry
---

# Metrics

Sunbird RC enables emitting events from registry. Any operation like ADD, DELETE, UPDATE can emit events. Currently, registry only emits the ADD metrics

Sunbird RC supports two types in which events can be emitted.&#x20;

The events emitted are in the format of sunbird telemetry specs. Following is the sample of event that is emitted from registry

```
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

1. Kafka
2. File based

### **Kafka based -**&#x20;

Registry sends the event in the above format i.e Sunbird Telemetry format to kafka topic configured. Kafka will hold the data and then consumer can run on this to store this into clickhouse

OperationType here can be ADD, UPDATE, DELETE

Data of the entity will be masked using configuration defined in the schema.&#x20;

Schema contains osConfig. In this, you need to define 2 keys namely privateFieldConfig and internalFieldConfig for privateFields and internalFields.&#x20;

There are 5 types of masking supported\
1\. NONE - field value will not be emitted\
2\. FULL - emit the field value as-is \
3\. HASH - emit the field as a one-way hash (salted) \
4\. MASK - emit the field using a masking strategy \
5\. HASH-MASK - emit two values for this field, one hashed and one masked

By default, both the configs hold NONE so the private and internal fields will not be emitted

#### **Metrics Service -**

The service is a consumer for the events emitted by registry into kafka topic.&#x20;

Metric service by default uses clickhouse as the database to store the events. Table Schema for events is as follows

```
create table <table_name> (
    operationType String,
    date Date,
    entityId String,
    entity JSON
)
```

For every distinct type of object, the service creates new table and stores the data related to that object in it.

The service also exposes an API which returns the count of all the events emitted. It aggregates the data from all the tables created

### File Based

The events emitted by registry are logged to a log file which is present at `metrics_log/metrics.log` . The log also has a rolling policy based on the size (30MB).
