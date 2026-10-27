Quick Reference and Learning Paths
==================================

Use these paths to find the most relevant topic for your task.

Learning paths
--------------

**New to database administration**

1. Read :doc:`PostgreSQL concepts and reference <postgresql>`.
2. Learn the fundamentals of :doc:`SQL <sql>`.
3. Review the administration and operational topics linked from the main page.

**Preparing for production DBA work**

1. Review the :doc:`PostgreSQL upgrade guide <postgresql_upgrade_guide>`.
2. Study :doc:`pgBackRest backup and recovery <pgBackRest_Document>`.
3. Use the :doc:`DB Incident Replay investigation workflow <db_incident_replay>` to structure evidence-based troubleshooting.

**Investigating a production incident**

1. Define the symptom, impact, and time window.
2. Check whether the available logs and snapshots cover the period.
3. Inspect evidence before accepting a finding or hypothesis.
4. Record uncertainty and missing data.
5. Validate the cause and document the resolution.

Reference links
---------------

* `Official PostgreSQL documentation <https://www.postgresql.org/docs/current/>`_
* `PostgreSQL monitoring statistics <https://www.postgresql.org/docs/current/monitoring-stats.html>`_
* `PostgreSQL explicit locking <https://www.postgresql.org/docs/current/explicit-locking.html>`_
* `pg_stat_statements <https://www.postgresql.org/docs/current/pgstatstatements.html>`_

Version reminder
----------------

PostgreSQL features, system views, columns, and configuration defaults can vary
between major versions. Confirm commands against the version you operate and
test potentially disruptive changes before production use.
