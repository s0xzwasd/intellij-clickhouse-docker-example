![GitHub](https://img.shields.io/github/license/s0xzwasd/intellij-clickhouse-docker-example?style=flat-square) 

An example of [ClickHouse OLAP DBMS](https://clickhouse.com/) database
using [yandex/clickhouse-server](https://registry.hub.docker.com/r/yandex/clickhouse-server) image in IntelliJ-based
IDEs.

# Getting Started

1. Run Docker services or Docker Desktop.
2. Open `docker-compose.yaml` in IntelliJ-based IDEs and click on the gutter icon over `services` declaration.
   Alternatively, execute `sudo docker-compose up -d` in CLI.
3. Select _View | Tool Windows | Database_, select _New_ and configure the following options:
    1. Host: `localhost`
    2. Port: `8123`
    3. User: `default`
    4. Password: empty
    5. Database: empty or `default`
4. Press _Test Connection_, save changes.
5. Initialize a sample table by executing the code below
   in [Query Console](https://www.jetbrains.com/help/idea/working-with-database-consoles.html).

```clickhouse
CREATE TABLE test_tbl
(
    id          UInt16,
    create_time Date,
    comment Nullable(String)
) ENGINE = MergeTree()
      PARTITION BY create_time
      ORDER BY (id, create_time)
      PRIMARY KEY (id, create_time)
      TTL create_time + INTERVAL 1 MONTH
      SETTINGS index_granularity = 8192;
```

```clickhouse
Insert into test_tbl
values (0, '2019-12-12', null);
Insert into test_tbl
values (0, '2019-12-12', null);
Insert into test_tbl
values (1, '2019-12-13', null);
Insert into test_tbl
values (1, '2019-12-13', null);
Insert into test_tbl
values (2, '2019-12-14', null);
```
