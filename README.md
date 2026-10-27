# PostgreSQL DBA Documentation — Kiran Kambale

A Sphinx-powered documentation library covering PostgreSQL administration,
SQL, upgrades, backup/recovery, and practical database incident investigation.

- **Hosted documentation:** https://pgsql-sphinx-docs.readthedocs.io/en/latest/
- **DB Incident Replay topic:** https://pgsql-sphinx-docs.readthedocs.io/en/latest/db_incident_replay.html
- **Source repository:** https://github.com/Kiran-Kambale/pgsql-Sphinx-docs

## Build locally

From the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r docs/requirements.txt
sphinx-build -b html docs/source docs/build/html
```

Open `docs/build/html/index.html` to review the generated documentation.

## Publishing updates

Commit and push the updated source files to the GitHub repository connected to
Read the Docs. After the build completes successfully, the hosted site will
publish the new `db_incident_replay.html` page and the revised navigation.

The page documents the evidence-analysis workflow and clearly distinguishes
current scope from capabilities that require additional implementation or
configuration. Always verify product behavior for the specific release.
