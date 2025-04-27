# Change Data Capture (PostgreSQL to ClickHouse)

## Step 1: Setup PostgreSQL Config: 
- Edit postgresql.conf :
```
  wal_level = logical
  max_replication_slots = 4
  max_wal_senders = 4
```

- In order to allow replication to Debezium, edit pg_hba.conf and add :
```
host    replication     all             <debezium_ip>/32         md5
```
- restart PostgreSQL service:
```
docker restart <postgres_container>
```

## Step 2: Start CDC Pipeline:
```
docker-compose up -d
```

## Step 3: Configure Debezium connector
- [Run Postman Register API](Debezium.postman_collection.json) 

## Step 4: Configure ClickHouse
Per each table on PostgreSQL there is need to do these tree steps on ClickHouse side:
- Create Kafka engine Table:
  
CREATE TABLE mydb.table_queue
(
    id UInt64,
    values Nullable(String),
    created_at Nullable(String),
    created_by_id Nullable(String),
    is_deleted Bool,
    is_archived Bool
)
ENGINE = Kafka
SETTINGS kafka_broker_list = 'kafka:9092',
 kafka_topic_list = 'etl_table',
 kafka_group_name = 'clickhouse-queue',
 kafka_format = 'JSONEachRow',
 kafka_num_consumers = 1,
 kafka_handle_error_mode = 'stream';
  

- Create a Materialized View:
  ```

  ```

- Create a destination Table:
  ```
  
  ```
