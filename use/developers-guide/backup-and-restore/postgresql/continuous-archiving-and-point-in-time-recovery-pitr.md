# Continuous Archiving and Point-in-Time Recovery (PITR)

PostgreSQL maintains a write-ahead log (WAL) in the `pg_wal/` subdirectory of the data directory to record all changes made to the database's data files. This log ensures crash safety and allows restoring consistency by replaying the log entries since the last checkpoint. Combining a file-system-level backup with a backup of the WAL files provides an alternative backup strategy. To recover, the file system backup is restored, and then the backed-up WAL files are replayed to bring the system to the current state. This approach offers some benefits:

1. A perfectly consistent file system backup is not required as internal inconsistencies can be corrected through log replay.
2. Continuous backup can be achieved by archiving the WAL files, which is useful for large databases where frequent full backups may not be convenient.
3. Point-in-time recovery is supported, allowing the database to be restored to any desired state since the base backup.
4. By continuously feeding the series of WAL files to another machine with the same base backup, a warm standby system can be created for nearly-current database copies.

Note that `pg_dump` and `pg_dumpall` cannot be used as part of continuous archiving since they do not produce file-system-level backups with enough information for WAL replay.

It's important to consider that this method only supports the restoration of an entire database cluster and requires significant archival storage for the base backup and WAL traffic. However, it is a preferred backup technique in situations where high reliability is crucial.

To successfully recover using continuous archiving, a continuous sequence of archived WAL files extending back to the start time of the backup is necessary. Therefore, it's essential to set up and test the procedure for archiving WAL files before taking the initial base backup.

## Limitations

\
Currently, the continuous archiving technique in PostgreSQL has some limitations that may be addressed in future releases. These limitations include:

1. Risk of template database modifications during base backup: If a CREATE DATABASE command is executed while a base backup is in progress and the template database is modified during this time, the modifications may be applied to the created database during recovery. To avoid this issue, it is recommended not to modify any template databases while taking a base backup.
2. Tablespaces replaying with absolute paths: CREATE TABLESPACE commands are logged with absolute paths and will be replayed as tablespace creations with the same absolute path. This can be problematic if the log is replayed on a different machine or even on the same machine into a new data directory, as it will overwrite the contents of the original tablespace. To avoid potential issues, it is best practice to take a new base backup after creating or dropping tablespaces.
3. Bulky default WAL format: The default Write-Ahead Log (WAL) format is relatively large because it includes disk page snapshots to support crash recovery. These page snapshots ensure the recovery process can fix partially-written disk pages. However, depending on the hardware and software setup, the risk of partial writes may be negligible. In such cases, the total volume of archived logs can be significantly reduced by disabling page snapshots using the full\_page\_writes parameter. It's essential to carefully read the notes and warnings in Chapter 30 before making this change. Disabling page snapshots does not prevent the use of logs for point-in-time recovery (PITR). Future development may focus on compressing archived WAL data by removing unnecessary page copies even with full\_page\_writes enabled. Administrators can also reduce the number of page snapshots included in WAL by increasing the checkpoint interval parameters as much as possible.
