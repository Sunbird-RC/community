# Cassandra

\
Apache Cassandra is an open-source distributed database management system that offers exceptional scalability, fault tolerance, and cost-effectiveness, making it highly desirable for enterprises. One of its standout features is the ability to ensure uninterrupted service even in the face of node failures.

Cassandra achieves this by replicating data across multiple nodes and data centers. The data is stored in SSTable files, which reside in the `keyspace` directory within the data directory path specified in the `Cassandra.yaml` file.

By default, the SSTable files are stored in the following directory path: `/var/lib/cassandra/data/`

With its robust data replication and fault-tolerant architecture, Cassandra guarantees continuous availability, making it a reliable choice for enterprise-level applications.

Cassandra offers two distinct approaches for backups:

1. [Snapshot-based backup:](snapshot-based-backup-method.md) This method involves capturing point-in-time snapshots of the entire database. Snapshots provide a consistent view of the data at a specific moment, facilitating recovery from various issues or accidental data loss.
2. [Incremental backup:](incremental-backup-method.md) With incremental backup, only the changes made since the last backup are stored. This approach reduces storage requirements and backup duration by capturing and preserving only the modified data. It complements snapshot-based backups by providing more frequent and efficient backup options.

By leveraging these backup methods, Cassandra users can ensure data resiliency and efficiently restore their databases in case of unexpected events or data inconsistencies.
