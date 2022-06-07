# PostgreSQL Tools

## barman-wal-restore-reverse.py

Script to restore WAL files in reverse manner.
It may be used together with [barman-wal-restore](https://github.com/2ndquadrant-it/barman/blob/616c7765c2052b29542e458dc3a4d54dc7724873/doc/barman-wal-restore.1.md).

Script barman-wal-restore downloads multiple WAL files from Barman server to local file system and then returns them one by one to PostgreSQL Server. It's a good point to request multiple files by time to speed up recovery process but still not good enough when you have a lot of WAL files to download.

To help improve this situation barman-wal-restore-reverse.py was made. It can be used to download WAL files from Barman in reverse order, beginning from last available down to WAL processed by barman-wal-restore script.

Example of use:

1. Switch to postgresql user

```sh
sudo su - postgres
```

2. Find default spool directory of barman-wal-restore script

```sh
barman-wal-restore --help
...
  --spool-dir SPOOL_DIR Specifies spool directory for WAL files. Defaults to '/var/tmp/walrestore'.
...
```

3. Go to the barman and find most recent backup

```sh
barman list-backup myserver
myserver 20220516T125248 - Mon May 16 15:14:17 2022 - Size: 6.4 TiB - WAL Size: 1.6 TiB
```

4. Switch to tmux, because it can take some time

```sh
tmux
```

5. Run the script, but do not stop it. Because after restart it will download only new logs after last stop.

```sh
barman-wal-restore-reverse.py \
  -d /var/tmp/walrestore \
  -U barman \
  barman.server myserver 20220516T125248
```
