# DBT Copilot Python

![](https://codebuild.eu-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiTG41bUNVdHN1b3NRS0hTYUlmMktLbnNNQzEyTlpMRDBlYlZiV1ZjNnl4b3dyMXl0R3VIUEVIbGVnYVJWbHd0OVZndVhURFpnckp5dWx0R0llMVpHUktzPSIsIml2UGFyYW1ldGVyU3BlYyI6ImthS3RRRUtOYkljSUVVUHMiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=main)

A set of utility functions for running Django & Flask apps in AWS ECS via AWS Copilot.

## Using `dbt-copilot-python`

### Installation

```
pip install dbt-copilot-python
```

### Usage

In `settings.py`...

#### ALLOWED_HOSTS

Add the ECS container IP to `ALLOWED_HOSTS` so that the Application Load Balancer (ALB) healthcheck will succeed:

```
from dbt_copilot_python.network import setup_allowed_hosts

ALLOWED_HOSTS = [...]

ALLOWED_HOSTS = setup_allowed_hosts(ALLOWED_HOSTS)
```

#### DATABASES

To configure the `DATABASES` setting from an RDS JSON object stored in AWS Secrets Manager, there are two options.

1. Configure the `DATABASES` setting to use a database URL (recommended):

    Note: This is dependent on the [`dj-database-url`](https://pypi.org/project/dj-database-url/) package which can be installed via `pip install dj-database-url`.

    ```
    import dj_database_url

    from dbt_copilot_python.database import database_url_from_env
   
    DATABASES = {
        "default": dj_database_url.config(
            default=database_url_from_env("DATABASE_ENV_VAR_KEY")
        )
    }
    ```

2. Configure the `DATABASES` setting to use a dictionary containing the settings:

    ```
    from dbt-copilot-python.database import database_from_env

    DATABASES = database_from_env("DATABASE_ENV_VAR_KEY")
    ```

## Contributing to `dbt-copilot-python`

### Requirements

- [Poetry](https://python-poetry.org/); `pip install poetry`

### Install dependencies & pre-commit hooks

```
poetry install && poetry run pre-commit install
```

### Run the tests

```
poetry run pytest
```
