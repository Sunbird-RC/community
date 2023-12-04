---
description: >-
  The following document describes about how audit on registry schema can be
  enabled
---

# Audit Configuration

Sunbird-RC enables audit logging of the schemas. Any operation like ADD/READ/UPDATE/DELETE/SEARCH on the schema will be added to the audit log.

Currently, Sunbird-RC supports two types of audit logging:

1. FILE\
   In file-based audit, the logs are appended to a log file which is present at `audit_logs/audit.log`. The log also has a rolling policy based on the size (30MB).
2. DATABASE\
   In database audit, the logs are added to the database and also to elastic search if it's enabled at the registry level. To store the audit logs in the database an audit schema needs to be created. The sample schema for the audit logs should be as per [https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/java/registry/src/main/resources/raw\_audit\_schema.json](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/java/registry/src/main/resources/raw\_audit\_schema.json). The audit schema should be placed in the schema's directory configured for the registry.
