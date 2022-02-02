---
layout: default
title: PostgreSQL Backups Tester
nav_order: 95
has_children: true
---

What is it
==========

This is a container which tests your Postgresql backups for corruption.
Making backups of the database is very important.
But testing the backups if they are valid is also very important part of development.

Postgres Backup collects all dumps in the specified directory and tries to restore them one-by-one to Postgresql instance.
Then writes result of restoration to the database and stdout.
Restoration is done via `psql` command.
Backups must be done with `pg_dump` command, Postgres Backup works only with `.sql.gz` files currently.

Results
-------

Refer to [installation](/images/postgres-backup/install) part, how to configure your setup.

If you set up all correctly, then in your database there is a table `pbackup_check`.
This table consists of a number of columns:

- file: path to dump file.
- status: status of restoration, 0 - success, 1 - fail.
- error: if failed, then this field will contain error message.
- created_at: date of check.
