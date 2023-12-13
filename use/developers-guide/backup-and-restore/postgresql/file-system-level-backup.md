# File System Level Backup

Instead of using the pg\_dump method, another backup strategy is to directly copy the files used by PostgreSQL to store database data. These files can be backed up using file system backup methods. For example:

```
tar -cf backup.tar /usr/local/pgsql/data
```

However, there are limitations to this approach that make it less practical compared to pg\_dump:

1. The database server must be shut down to obtain a usable backup. Simply disallowing connections is not sufficient. Stopping the server is necessary for both backup and restoration.
2. It is not possible to selectively back up or restore individual tables or databases from their respective files or directories. The commit log files (pg\_xact/\*) are required along with the table files to make the backup usable. Therefore, file system backups only work for complete backup and restoration of the entire database cluster.

An alternative file system backup approach is to create a "consistent snapshot" of the data directory, if supported by the file system. The typical procedure involves taking a frozen snapshot of the volume, copying the entire data directory from the snapshot to a backup device, and then releasing the frozen snapshot. This method allows backup while the database server is running. However, starting the database server using this backup will trigger WAL log replay, as if the previous server instance crashed. Including the WAL files in the backup is important, and performing a CHECKPOINT before taking the snapshot can reduce recovery time.

If the database is spread across multiple file systems, simultaneous frozen snapshots of all volumes may not be possible. In such cases, shutting down the database server to establish the frozen snapshots or using continuous archiving base backup can be options.

Another approach is to use rsync for file system backup. This involves running rsync while the database server is running and then temporarily shutting down the server to perform an rsync --checksum. This method allows a file system backup to be performed with minimal downtime.

Note that file system backups will generally be larger in size compared to SQL dumps. While pg\_dump excludes index contents and only includes commands for recreation, file system backups may include more data. However, file system backups may offer faster backup speeds.
