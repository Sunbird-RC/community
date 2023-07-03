# Snapshot-based backup method

Cassandra offers the `nodetool` utility, a command-line interface for managing clusters, including the ability to create data snapshots. By using the `nodetool` snapshot command, you can create snapshots by flushing `memtables` to disk and creating immutable hard links to SSTables.

Snapshots can be taken per node or across the entire cluster. For cluster-wide snapshots, parallel ssh utilities like `pssh` can be used, or you can take snapshots of each node individually. It's possible to capture snapshots of all keyspaces, specific keyspaces, or even individual tables within a keyspace. Keep in mind that sufficient free disk space is required on the node to accommodate the snapshot.

It's important to note that this snapshot method does not back up the schema, which must be backed up manually and separately. Here are a few examples of snapshot commands:

**Snapshot of all keyspaces:**

To capture a snapshot of all keyspaces, run the command:

```bash
nodetool snapshot
```

**Snapshot of a single keyspace:**

Assuming you have a keyspace named "organization" and you want to name the snapshot, use this command:

```bash
nodetool snapshot -t 2023.06.27 organization
```

**Snapshot of a single table:**

If you wish to snapshot only the "employee" table within the "organization" keyspace, execute the following command:

```
nodetool snapshot --table employee organization
```

After completing the snapshot, you can transfer the snapshot files to other storage locations like AWS S3, Google Cloud, or MS Azure. Remember to back up the schema separately because Cassandra can restore data from a snapshot only when the corresponding table schema exists.

## Advantages:

* Easy to manage and straightforward
* The nodetool utility in Cassandra offers the nodetool clearsnapshot command for removing snapshot files.

## Disadvantages:

* Taking daily backups of an entire keyspace can be challenging, especially for large datasets.
* Transferring large snapshot data to secure locations like AWS S3 can be costly.
