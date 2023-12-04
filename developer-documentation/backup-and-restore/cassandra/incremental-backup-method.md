# Incremental backup method

Enabling incremental backup in Cassandra involves modifying the `incremental_backups`value to `true` in the `cassandra.yaml` file. Once enabled, Cassandra establishes hard links to flushed memtables in SSTables within a backup directory located in the keyspace data directory. Incremental backups in Cassandra exclusively consist of new SSTable files, dependent on the last created snapshot. This approach minimizes disk space usage as it solely includes links to recently generated SSTable files from the previous full snapshot.

## Advantages

* Decreases disk space requirements
* Reduces transfer costs

## Disadvantages

* Cassandra does not automatically clear incremental backup files, necessitating the creation of a custom script for their removal
* Generates numerous small-sized files during backup, complicating file management and recovery procedures
* It is not possible to selectively choose specific column families for incremental backup.
