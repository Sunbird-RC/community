# Data Restore

Restoring data from backups is crucial for ensuring data availability, especially in scenarios such as keyspace deletion, launching a new cluster from backup data, or replacing a node. The restoration process typically involves utilizing snapshots and incremental backup files.

There are two primary methods for restoring data from backups:

### Using `nodetool` refresh

The `nodetool` refresh command enables the loading of newly placed SSTables onto the system without requiring a restart. This method is useful when a new node replaces an unrecoverable node. To restore data from a snapshot using this method, follow these steps:

* Create the necessary schema if it doesn't already exist.
* Truncate the table if needed.
* Locate the snapshot folder (e.g., /var/lib/keyspace\_name/table\_name-UUID/snapshots/snapshot\_name) and copy the snapshot SSTable directory to the /var/lib/keyspace/table\_name-UUID directory.
* Execute the `nodetool refresh` command.

### Using `sstableloader`:

The `sstableloader` is a tool for loading a set of SSTable files into a Cassandra cluster. It offers options for loading external data, existing SSTables, and restoring snapshots. To restore data using `sstableloader`, follow these steps:

* Create the required schema if it's not already present.
* Truncate the table if necessary.
* Bring the backup data to a node from a storage service like AWS S3, Google Cloud, or MS Azure (e.g., download the backup data to /home/data).
*   Run the following command:

    ```javascript
    sstableloader -d <ip> /home/data
    ```

Note: Replace `<ip>` with the appropriate IP address.

By following these methods, data can be effectively restored from backups, ensuring data availability and recovery in various scenarios.
