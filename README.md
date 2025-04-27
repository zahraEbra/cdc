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
