# Documentation

Root URL of the Doc. [InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/)

### CLI

主要关于启动CLI：[Using influx - InfluxDB command line interface | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/tools/shell/)

## InfluxQL

This section introduces InfluxQL, the InfluxDB SQL-like query language for working with data in InfluxDB databases.

[Influx Query Language (InfluxQL) | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/)

#### Line Protocol

[InfluxDB line protocol tutorial | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/write_protocols/line_protocol_tutorial/)

* `insert into <retention policy> <line protocol>`
* line protocol = `<measurement>,[<tag set>] <field set> [timestamp]`
* field set is a must
* tag set and timestamp are optional

#### SELECT and FROM clause

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/)

* `FROM <database_name>.<retention_policy_name>.<measurement_name>`

* `FROM <database_name>..<measurement_name>`

* `SELECT *::field`

* > A query requires at least one [field key](https://docs.influxdata.com/influxdb/v1.8/concepts/glossary/#field-key) in the `SELECT` clause to return data. If the `SELECT` clause only includes a single [tag key](https://docs.influxdata.com/influxdb/v1.8/concepts/glossary/#tag-key) or several tag keys, the query returns an empty response. This behavior is a result of how the system stores data.



#### GROUP BY clause

> **Note:** You cannot use `GROUP BY` to group fields.

##### GROUP BY tags

##### GROUP BY time()

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#group-by-time-intervals)

#### INTO clause

[Explore data using InfluxQL | InfluxDB OSS 1.8 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#examples-4)

##### Rename a database

```sql
SELECT * INTO "copy_NOAA_water_database"."autogen".:MEASUREMENT FROM "NOAA_water_database"."autogen"./.*/ GROUP BY *
```

* `GROUP BY *` here preserves the tags from automatically transforming into fields

### Other Usage

###### specify a tag with None value

#### [Use a regular expression to specify a tag with no value in the WHERE clause](https://docs.influxdata.com/influxdb/v1.8/query_language/explore-data/#use-a-regular-expression-to-specify-a-tag-with-no-value-in-the-where-clause)

```sql
SELECT * FROM "h2o_feet" WHERE "location" !~ /./
```

###### show series

```sql
SHOW series
```

###### show all keys given a tag key

```sql
SHOW tag values from <measurements> with KEY=<key_name>
```



# Gossip

`service influxdb start` to start influxdb daemon

`influx -precision rfc3339` to start influxdb CLI

`0.0.0.0` 比 `127.0.0.1` 好用... 可以连接到外网，TODO: 请查明原因

---

