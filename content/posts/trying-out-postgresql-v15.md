---
title: "Trying out PostgreSQL v15"
date: 2022-10-18T19:30:30+05:30
---

Recently the PostgreSQL community [released PostgreSQL v15](https://www.postgresql.org/about/news/postgresql-15-released-2526/), which is the latest version as of this writing. I thought I could try out and experiment with the latest and greatest of PostgreSQL so I got my hands on this version with some basic steps

```bash
# Get prebuilt binary links from a trusted source. For example https://www.enterprisedb.com/download-postgresql-binaries

wget https://sbp.enterprisedb.com/getfile.jsp?fileid=1258215 -O pg-15-osx-binaries.zip

unzip pg-15-osx-binaries.zip

sudo xattr -r -d com.apple.quarantine pgsql

./pgsql/bin/postgres --version

./pgsql/bin/psql --version

./pgsql/bin/initdb --version

mkdir test-data-dir

./pgsql/bin/initdb -D test-data-dir

./pgsql/bin/postgres -D test-data-dir

./pgsql/bin/pg_ctl -D test-data-dir -l logfile status

./pgsql/bin/pg_ctl -D test-data-dir -l logfile start

cat logfile

./pgsql/bin/pg_ctl -D test-data-dir -l logfile status

ps aux | rg postgres

ps aux | grep postgres

./pgsql/bin/psql -h localhost -d postgres

ls test-data-dir/postmaster.pid

cat test-data-dir/postmaster.pid

./pgsql/bin/pg_ctl -D test-data-dir -l logfile stop

ls test-data-dir/postmaster.pid

./pgsql/bin/pg_ctl -D test-data-dir -l logfile status

ls test-data-dir/

cat test-data-dir/PG_VERSION
```

Given below is the demo of how I do the above given steps and what output / logs show up when running the commands

{{<asciinema 529793>}}
