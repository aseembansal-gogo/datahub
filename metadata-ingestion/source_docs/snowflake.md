# Snowflake

For context on getting started with ingestion, check out our [metadata ingestion guide](../README.md).

## Setup

To install this plugin, run `pip install 'acryl-datahub[snowflake]'`.

## Capabilities

This plugin extracts the following:

- Metadata for databases, schemas, views and tables
- Column types associated with each table

:::tip

You can also get fine-grained usage statistics for Snowflake using the `snowflake-usage` source described below.

:::

## Quickstart recipe

Check out the following recipe to get started with ingestion! See [below](#config-details) for full configuration options.

For general pointers on writing and running a recipe, see our [main recipe guide](../README.md#recipes).

```yml
source:
  type: snowflake
  config:
    # Coordinates
    host_port: account_name
    warehouse: "COMPUTE_WH"

    # Credentials
    username: user
    password: pass
    role: "sysadmin"

sink:
  # sink configs
```

## Config details

Note that a `.` is used to denote nested fields in the YAML recipe.

| Field                    | Required | Default                                                              | Description                                                                                                                                                                             |
| ------------------------ | -------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `username`               |          |                                                                      | Snowflake username.                                                                                                                                                                     |
| `password`               |          |                                                                      | Snowflake password.                                                                                                                                                                     |
| `host_port`              | ✅       |                                                                      | Snowflake host URL.                                                                                                                                                                     |
| `warehouse`              |          |                                                                      | Snowflake warehouse.                                                                                                                                                                    |
| `role`                   |          |                                                                      | Snowflake role.                                                                                                                                                                         |
| `env`                    |          | `"PROD"`                                                             | Environment to use in namespace when constructing URNs.                                                                                                                                 |
| `options.<option>`       |          |                                                                      | Any options specified here will be passed to SQLAlchemy's `create_engine` as kwargs.<br />See https://docs.sqlalchemy.org/en/14/core/engines.html#sqlalchemy.create_engine for details. |
| `database_pattern.allow` |          |                                                                      | Regex pattern for databases to include in ingestion.                                                                                                                                    |
| `database_pattern.deny`  |          | `"^UTIL_DB$" `<br />`"^SNOWFLAKE$"`<br />`"^SNOWFLAKE_SAMPLE_DATA$"` | Regex pattern for databases to exclude from ingestion.                                                                                                                                  |
| `table_pattern.allow`    |          |                                                                      | Regex pattern for tables to include in ingestion.                                                                                                                                       |
| `table_pattern.deny`     |          |                                                                      | Regex pattern for tables to exclude from ingestion.                                                                                                                                     |
| `schema_pattern.allow`   |          |                                                                      | Regex pattern for schemas to include in ingestion.                                                                                                                                      |
| `schema_pattern.deny`    |          |                                                                      | Regex pattern for schemas to exclude from ingestion.                                                                                                                                    |
| `view_pattern.allow`     |          |                                                                      | Regex pattern for views to include in ingestion.                                                                                                                                        |
| `view_pattern.deny`      |          |                                                                      | Regex pattern for views to exclude from ingestion.                                                                                                                                      |
| `include_tables`         |          | `True`                                                               | Whether tables should be ingested.                                                                                                                                                      |
| `include_views`          |          | `True`                                                               | Whether views should be ingested.                                                                                                                                                       |

## Compatibility

Coming soon!

## Snowflake Usage Stats

For context on getting started with ingestion, check out our [metadata ingestion guide](../README.md).

### Setup

To install this plugin, run `pip install 'acryl-datahub[snowflake-usage]'`.

### Capabilities

This plugin extracts the following:

- Statistics on queries issued and tables and columns accessed (excludes views)
- Aggregation of these statistics into buckets, by day or hour granularity

Note: the user/role must have access to the account usage table. The "accountadmin" role has this by default, and other roles can be [granted this permission](https://docs.snowflake.com/en/sql-reference/account-usage.html#enabling-account-usage-for-other-roles).

Note: the underlying access history views that we use are only available in Snowflake's enterprise edition or higher.

:::note

This source only does usage statistics. To get the tables, views, and schemas in your Snowflake warehouse, ingest using the `snowflake` source described above.

:::

### Quickstart recipe

Check out the following recipe to get started with ingestion! See [below](#config-details) for full configuration options.

For general pointers on writing and running a recipe, see our [main recipe guide](../README.md#recipes).

```yml
source:
  type: snowflake-usage
  config:
    # Coordinates
    host_port: account_name
    warehouse: "COMPUTE_WH"

    # Credentials
    username: user
    password: pass
    role: "sysadmin"

    # Options
    top_n_queries: 10

sink:
  # sink configs
```

### Config details

Note that a `.` is used to denote nested fields in the YAML recipe.

| Field             | Required | Default                                                        | Description                                                     |
| ----------------- | -------- | -------------------------------------------------------------- | --------------------------------------------------------------- |
| `username`        |          |                                                                | Snowflake username.                                             |
| `password`        |          |                                                                | Snowflake password.                                             |
| `host_port`       | ✅       |                                                                | Snowflake host URL.                                             |
| `warehouse`       |          |                                                                | Snowflake warehouse.                                            |
| `role`            |          |                                                                | Snowflake role.                                                 |
| `env`             |          | `"PROD"`                                                       | Environment to use in namespace when constructing URNs.         |
| `bucket_duration` |          | `"DAY"`                                                        | Duration to bucket usage events by. Can be `"DAY"` or `"HOUR"`. |
| `start_time`      |          | Last full day in UTC (or hour, depending on `bucket_duration`) | Earliest date of usage logs to consider.                        |
| `end_time`        |          | Last full day in UTC (or hour, depending on `bucket_duration`) | Latest date of usage logs to consider.                          |
| `top_n_queries`   |          | `10`                                                           | Number of top queries to save to each table.                    |

### Compatibility

Coming soon!

## Questions

If you've got any questions on configuring this source, feel free to ping us on [our Slack](https://slack.datahubproject.io/)!
