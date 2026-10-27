PostgreSQL DBA Documentation by Kiran Kambale
============================================

Welcome to the **PostgreSQL DBA Tutorial**, a practical documentation library
maintained by **Kiran Kambale**. It brings together PostgreSQL administration,
SQL, backup and recovery, upgrades, performance topics, and database incident
investigation in one searchable place.

The content is organized by task so that beginners can follow a learning path
and experienced engineers can jump directly to a reference topic.

How to use this documentation
-----------------------------

* **New to PostgreSQL?** Start with the PostgreSQL reference and SQL sections.
* **Preparing for DBA work?** Review administration, upgrades, backup/recovery,
  performance, and troubleshooting topics.
* **Investigating a real issue?** Read the DB Incident Replay guide and use its
  evidence-based investigation checklist.
* **Looking for a specific command or concept?** Use the search box or index.

.. tip::

   For production work, verify commands and settings against your PostgreSQL
   major version and environment. Always test potentially disruptive changes
   before applying them to production.

Documentation library
---------------------

.. toctree::
   :maxdepth: 2
   :caption: Start here

   db_incident_replay

.. toctree::
   :maxdepth: 2
   :caption: PostgreSQL administration and operations

   postgresql
   postgresql_upgrade_guide
   pgBackRest_Document

.. toctree::
   :maxdepth: 2
   :caption: SQL and database fundamentals

   sql
   index2

.. toctree::
   :maxdepth: 2
   :caption: Other database platforms

   mongodb

About the author
----------------

**Kiran Kambale** is a PostgreSQL database professional focused on production
database support, troubleshooting, backup and recovery, performance, and
operational reliability. This documentation is intended to make practical
knowledge easier to find, learn, and apply.

Contributions and source
------------------------

The documentation source is maintained in the
`pgsql-Sphinx-docs GitHub repository <https://github.com/Kiran-Kambale/pgsql-Sphinx-docs>`_.
Corrections and improvements should be made in the source files and then
published through the configured Read the Docs build.
