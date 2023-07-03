# SQL Dump

## **Create a SQL Dump**

**Command:**

```sh
pg_dump dbname > dumpfile
```

`pg_dump` is a PostgreSQL utility used for backing up databases. It generates an SQL file that can be used to recreate the database in its original state. The output can be redirected to a file or used in other formats for more control. `pg_dump` can be executed from a remote host with database access, but it requires read permissions for the tables being backed up. It offers advantages such as compatibility with different PostgreSQL versions and support for transferring databases across different machine architectures. Dumps created by `pg_dump` are internally consistent, representing a snapshot of the database at the time of the dump.

## Restore from a dump file

**Command:**

```sh
psql dbname < dumpfile
```

When using `pg_dump` to generate a database dump, the output is saved in a file called "dumpfile." It's important to create the database specified by "dbname" separately before using the `psql` utility for restoration. Ensure that all relevant users exist before restoring the SQL dump to maintain ownership and permissions. By default, `psql` continues executing even after encountering SQL errors, but you can change this behavior by setting the `ON_ERROR_STOP` variable. Alternatively, you can restore the entire dump as a single transaction using the `--single-transaction` option. The use of pipes enables direct database transfer between servers using pg\_dump and `psql` commands.

You might wish to run `psql` with the `ON_ERROR_STOP` variable set to alter that behavior and have `psql` exit with an exit status of 3 if an SQL error occurs:

```
psql --set ON_ERROR_STOP=on dbname < dumpfile
```

The ability of pg\_dump and psql to write to or read from pipes makes it possible to dump a database directly from one server to another, for example:

```
pg_dump -h host1 dbname | psql -h host2 dbname
```

## Important Note

When dealing with large `pg_dump` output files, certain operating systems may impose file size limits. Thankfully, `pg_dump` provides options to overcome this issue. Here are several methods to consider:

1. Compressed Dumps: Utilize compression tools like gzip to compress the `pg_dump` output file. For example:
   * Create a compressed dump: `pg_dump dbname | gzip > filename.gz`&#x20;
   * Restore the dump: `gunzip -c filename.gz | psql dbname` or `cat filename.gz | gunzip | psql dbname`
2. Splitting Output: Use the split command to divide the output into smaller files that fit within the file system limits. For example:
   * Split into 2 GB chunks: `pg_dump dbname | split -b 2G - filename`
   * Restore the dump: `cat filename* | psql dbname`
3. Custom Dump Format: If PostgreSQL was built with the zlib compression library, you can utilize the custom dump format, which compresses data as it writes to the output file. This format allows selective table restoration. For example:
   * Create a custom-format dump: `pg_dump -Fc dbname > filename`
   * Restore the dump: `pg_restore -d dbname filename`

Note: Custom-format dumps must be restored using `pg_restore`, not `psql`.

For very large databases, combining the split method with other approaches may be necessary.

4. Parallel Dump: To expedite the dump process for large databases, you can use `pg_dump`'s parallel mode, which dumps multiple tables simultaneously. Control the degree of parallelism using the -j parameter. Parallel dumps are supported for the "directory" archive format. For example:
   * Dump in parallel: `pg_dump -j num -F d -f out.dir dbname`
   * Restore in parallel: Use `pg_restore -j` to restore a parallel dump, regardless of the archive mode used.

Refer to the pg\_dump and pg\_restore reference pages for further details on these methods.
