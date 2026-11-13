# Reproducible local environment

Source: SQLGlot v30.31.0, upstream commit `447a8a8929835a82ead76d7c6d7e104df344ab36`.

The fixed execution image is Linux ARM64 with Python 3.11.2:
`adminfather/benzhi-claude-code@sha256:159e0b167850ddc1993271972df001dcb9815b1bd25b4fa55b9c69aedae6b922`.
The installed source runs in pure Python. SQLGlot runtime has no external service dependency.

```bash
curl -fsSL https://bootstrap.pypa.io/get-pip.py -o /tmp/sqlglot-get-pip.py
python3 /tmp/sqlglot-get-pip.py --user --break-system-packages pip==24.3.1
python3 -m pip install --user --break-system-packages -r requirements-cc-solo.txt
SETUPTOOLS_SCM_PRETEND_VERSION=30.31.0 python3 -m pip install --user --break-system-packages --no-build-isolation --no-deps -e .
SKIP_INTEGRATION=1 python3 -m unittest tests.test_lineage tests.test_schema tests.test_expressions
```

`python3 -m unittest tests.test_optimizer` checks optimizer rewrites using the local DuckDB dependency. Select the relevant test modules for the changed code. Run format checks with `python3 -m ruff check` and `python3 -m ruff format --check` on changed files.

The lockfile records the resolved local verification dependencies, including build tools. The upstream `dev` extra, pre-commit, mypy, optional compiled `sqlglotc`, and remote integration-test submodule are not installed by this minimal environment. `make check` includes those additional tools and is broader than the local baseline above; report unavailable checks accurately. No remote database, private submodule, or credential is needed for the listed local checks.
