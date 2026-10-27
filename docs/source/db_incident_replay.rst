DB Incident Replay: PostgreSQL Incident Investigation
=====================================================

.. meta::
   :description: A practical guide to DB Incident Replay, an evidence-analysis tool for PostgreSQL incident investigations.
   :keywords: PostgreSQL, DBA, database support, incident response, diagnostics, locks, evidence, root cause

Overview
--------

**DB Incident Replay** is a PostgreSQL incident investigation project created by
Kiran Kambale. It is designed to help database administrators and support
engineers organize collected evidence, understand possible explanations for an
incident, and communicate their findings clearly.

The central idea is simple: investigate from evidence rather than guesswork.
A report that says “the application was slow” describes a symptom. A useful
investigation asks what was observed, when it happened, what evidence supports
a possible explanation, what remains unknown, and what should be checked next.

.. important::

   The evidence-analysis version works with a supplied evidence bundle. Do not
   assume that opening the dashboard means it is continuously connected to or
   collecting data from a live PostgreSQL server. Live collection depends on
   the collector capabilities and configuration of the version being used.

Project links
-------------

* **Project documentation:** `DB Incident Replay guide <https://pgsql-sphinx-docs.readthedocs.io/en/latest/db_incident_replay.html>`_
* **Source repository:** `pgsql-Sphinx-docs on GitHub <https://github.com/Kiran-Kambale/pgsql-Sphinx-docs>`_
* **Documentation home:** `PostgreSQL DBA Tutorial <https://pgsql-sphinx-docs.readthedocs.io/en/latest/>`_

Why this project exists
-----------------------

PostgreSQL incidents can be difficult to investigate because useful information
may be scattered across logs, monitoring systems, database snapshots, ticket
comments, and the memories of people responding under pressure. Some evidence,
such as a live blocking relationship, can disappear when sessions finish.

DB Incident Replay aims to make the investigation more structured by helping an
engineer:

* review the evidence that is available for an incident;
* identify findings that deserve further investigation;
* place recorded events in time order;
* compare possible explanations with supporting evidence;
* identify gaps that limit the conclusion; and
* communicate the investigation in a consistent report.

Who can use it?
---------------

* **PostgreSQL DBAs** investigating database availability or performance issues.
* **Database support engineers** handling tickets and customer escalations.
* **SRE and operations teams** correlating database observations with service incidents.
* **Application developers** who need a clearer explanation of database-side symptoms.
* **Engineering managers and incident coordinators** who need a concise, evidence-backed summary.

How it works
------------

The investigation follows this general workflow:

.. code-block:: text

   Detect or receive a report
            |
            v
   Collect and validate evidence
            |
            v
   Review findings and build a timeline
            |
            v
   Compare possible explanations
            |
            v
   Investigate and validate the leading hypothesis
            |
            v
   Record next steps, resolution, and lessons learned

The exact data available depends on the evidence bundle and the capabilities of
the installed version. The product should distinguish direct observations from
interpretations and hypotheses. Events occurring close together in time are not
necessarily causally related.

Getting started
---------------

1. **Prepare an evidence bundle.** Use a supported bundle collected for the
   incident or the example lab bundle supplied with the project.
2. **Start the application.** Follow the installation and run instructions in
   the product repository. Use the Python environment and dependencies required
   by that version.
3. **Open the web dashboard.** Review the overview before drawing conclusions.
4. **Inspect findings.** Open each important finding and read the underlying
   evidence, timestamp, and source where available.
5. **Review the timeline.** Establish the order of recorded observations and
   note missing time intervals.
6. **Compare hypotheses.** Treat proposed causes as leads to validate, not as
   automatically confirmed root causes.
7. **Document the outcome.** Record confirmed facts, remaining uncertainty,
   next steps, and the resolution when it is known.

.. note::

   Screen labels and available actions can vary by release. Use the controls
   actually present in the running version; do not assume a planned feature is
   already implemented.

Example: investigating a slow application
-----------------------------------------

Imagine that an application team reports that checkout requests became slow
between 14:05 and 14:18. This is a starting point, not a root-cause diagnosis.

**Step 1 — Define the time window**

Record the reported start and end times, the time zone, the affected database,
and the business impact. Include a little time before the symptoms began when
reviewing available evidence.

**Step 2 — Check evidence coverage**

Confirm which logs and snapshots cover the period. If the first snapshot was
recorded at 14:10, the bundle cannot prove what happened at 14:06.

**Step 3 — Investigate blocking evidence**

If the bundle contains blocked-session observations, inspect the blocker, the
blocked sessions, transaction age, and relevant query context. A current lock
snapshot does not reconstruct a historical blocking chain unless that chain was
recorded at the time.

**Step 4 — Review connection and query evidence**

Look for evidence of connection pressure and query-performance changes where
those data sources are available. A high connection count alone does not prove
connection exhaustion, and cumulative query statistics are not the same as the
duration of one specific query execution.

**Step 5 — Write a careful conclusion**

A responsible interim conclusion might say: “Recorded observations show waiting
sessions during part of the reported slowdown. This is consistent with lock
contention contributing to the symptoms, but the available evidence does not
yet prove that it explains the entire incident.”

Evidence versus hypothesis
--------------------------

.. list-table:: How to phrase findings responsibly
   :header-rows: 1
   :widths: 22 38 40

   * - Category
     - Meaning
     - Example
   * - Observed fact
     - Directly recorded in a source.
     - “The 14:08 snapshot recorded three waiting sessions.”
   * - Interpretation
     - An explanation of what an observation may mean.
     - “The waiting sessions may indicate lock contention.”
   * - Hypothesis
     - A possible cause that still needs testing.
     - “A long-running transaction may have contributed to the delay.”
   * - Confirmed finding
     - A conclusion validated by sufficient evidence.
     - “The investigation verified that the transaction held a conflicting lock during the affected period.”
   * - Evidence gap
     - Missing data limits what can be concluded.
     - “No snapshot was collected before 14:08.”

Key PostgreSQL concepts
-----------------------

**Blocking and locks**
   PostgreSQL uses locks to coordinate concurrent work. A session can wait when
   another session holds a conflicting lock. See the PostgreSQL documentation
   on `explicit locking <https://www.postgresql.org/docs/current/explicit-locking.html>`_.

**Activity and sessions**
   ``pg_stat_activity`` provides a view of current backend activity, subject to
   permissions and configuration. It is a live view, not a historical recorder.
   See `Monitoring Database Activity <https://www.postgresql.org/docs/current/monitoring-stats.html>`_.

**Query statistics**
   ``pg_stat_statements`` can provide aggregated statistics about SQL statements
   when the extension is installed and configured. These values are cumulative
   statistics, not a complete record of every execution. See
   `pg_stat_statements <https://www.postgresql.org/docs/current/pgstatstatements.html>`_.

**Vacuum and transaction age**
   Vacuum-related observations and transaction age can help identify maintenance
   pressure, but thresholds must be interpreted in the context of workload,
   table activity, and configuration. See `Routine Vacuuming <https://www.postgresql.org/docs/current/routine-vacuuming.html>`_.

These links explain PostgreSQL concepts; they do not imply that every related
collector is present in every DB Incident Replay release.

Safe use and security
---------------------

Database evidence may include SQL text, usernames, hostnames, addresses, or
customer data. Treat evidence bundles and reports as sensitive.

* Use only authorized evidence and database connections.
* Keep credentials out of source control, screenshots, and reports.
* Use a least-privilege monitoring role for any live integration.
* Keep remote database TLS certificate verification enabled.
* Restrict dashboard access to trusted users.
* Review and redact sensitive query text before sharing evidence.
* Do not automatically terminate sessions, change settings, restart services,
  or trigger failovers based solely on a dashboard recommendation.

Until production authentication, authorization, deployment hardening, and
operational testing have been implemented and verified, use the tool in a
trusted local development environment rather than exposing it publicly.

Current scope and limitations
-----------------------------

The evidence-analysis workflow can only analyze the data that was actually
provided. It cannot recreate a query, lock, error, replication event, or metric
that was never captured. Historical analysis may be limited by missing samples,
permission restrictions, statistics resets, version differences, and incomplete
logs.

Do not assume the following capabilities are available unless they are
implemented and configured in the specific release:

* continuous live PostgreSQL collection;
* historical capture of every lock, query, or deadlock;
* complete replication, cloud, or high-availability integrations;
* automatic confirmation of a root cause; or
* automatic remediation of production systems.

Suggested incident checklist
----------------------------

Before investigating:

* [ ] Record the reported symptom and business impact.
* [ ] Identify the database and affected time window.
* [ ] Confirm the time zone and evidence coverage.
* [ ] Check for gaps or failed collection sources.

During investigation:

* [ ] Inspect findings and their supporting records.
* [ ] Build a timeline from recorded timestamps.
* [ ] Compare more than one plausible explanation.
* [ ] Separate observed facts from hypotheses.
* [ ] Record missing evidence and the next validation step.
* [ ] Assess the risk and authorization for any proposed action.

Before closing:

* [ ] Validate the cause and outcome where possible.
* [ ] Record corrective actions and remaining follow-up work.
* [ ] Sanitize the report before sharing it.

Incident summary template
-------------------------

Use this structure in a ticket or post-incident review:

.. code-block:: text

   Incident:
   Impact:
   Time window and time zone:
   Affected database / service:
   Observed facts:
   Leading hypothesis and supporting evidence:
   Alternative explanations:
   Evidence gaps / limitations:
   Next action and owner:
   Resolution and validation:
   Lessons learned:

Frequently asked questions
--------------------------

**Does DB Incident Replay prevent all database incidents?**

No. It is intended to support investigation and improve how evidence is
organized. Prevention also requires appropriate architecture, monitoring,
backup and recovery practices, testing, and operational processes.

**Does it automatically know the root cause?**

No. Findings and hypotheses help focus investigation. An engineer must validate
whether the evidence supports the final conclusion.

**Can a non-technical person understand the output?**

The summary, timeline, impact, and report can help non-specialists understand
what is known and what remains uncertain. Detailed database diagnosis still
benefits from a PostgreSQL professional.

**Does the dashboard monitor my live database?**

Not simply because the dashboard opens. Confirm the collector implementation,
configuration, permissions, and health status for the installed release.

**What happens when evidence is missing?**

The missing information should be documented as a limitation. Absence of a
record is not proof that an event did not occur.

**Can I use it on a production system?**

Only after reviewing the current release's security controls, supported data
sources, permissions, operational behavior, and deployment configuration.
Start with read-only diagnostics and a least-privilege account.

Further reading
---------------

* `PostgreSQL official documentation <https://www.postgresql.org/docs/current/>`_
* :doc:`PostgreSQL administration guide <postgresql>`
* :doc:`PostgreSQL upgrade guide <postgresql_upgrade_guide>`
* :doc:`pgBackRest documentation <pgBackRest_Document>`
* `DB Incident Replay source and project materials <https://github.com/Kiran-Kambale/pgsql-Sphinx-docs>`_

---

*Documentation maintained by Kiran Kambale. Verify the behavior of the specific
software release before using it in an operational environment.*
