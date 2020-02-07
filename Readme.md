### barman-wal-restore-reverse.py
Script to restore WAL files in reverse manner.
It may be used together with [barman-wal-restore](https://github.com/2ndquadrant-it/barman/blob/616c7765c2052b29542e458dc3a4d54dc7724873/doc/barman-wal-restore.1.md).

Script barman-wal-restore downloads multiple WAL files from Barman server to local file system and then returns them one by one to PostgreSQL Server. It's a good point to request multiple files by time to speed up recovery process but still not good enough when you have a lot of WAL files to download.

To help improve this situation barman-wal-restore-reverse.py was made. It can be used to download WAL files from Barman in reverse order, beginning from last available down to WAL processed by barman-wal-restore script.

Example of use:

    barman-wal-restore-reverse.py -d /var/tmp/barman-wal-restore -U barman barman.server my_postgres 20200207T005824
