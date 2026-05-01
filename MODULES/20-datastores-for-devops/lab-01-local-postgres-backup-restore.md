---
title: 'Local Postgres Backup and Restore'
tags:
  - lab
  - postgres
  - backups
module: "20"
---

# Lab 01 — Local Postgres Backup and Restore (pg_dump + Restore)

## Goal

Practice database recovery basics locally:

- run Postgres in a container
- create a dataset
- take a logical backup (`pg_dump`)
- simulate data loss
- restore and verify with explicit signals

## Prereqs

- Docker installed and working: `docker version`
- `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod20-postgres
cd ~/work/devops-labs/mod20-postgres
```

Start Postgres:

```bash
docker run -d --name mod20-pg \
  -e POSTGRES_PASSWORD=postgres \
  -p 15432:5432 \
  postgres:16-alpine
```

Wait for readiness:

```bash
until docker exec mod20-pg pg_isready -U postgres >/dev/null 2>&1; do sleep 1; done
```

## Steps

### 1) Create a Dataset

```bash
docker exec -i mod20-pg psql -U postgres <<'SQL'
create table if not exists demo_items (
  id serial primary key,
  name text not null
);
insert into demo_items(name) values ('a'), ('b'), ('c');
SQL
```

Verify row count:

```bash
docker exec -i mod20-pg psql -U postgres -tAc "select count(*) from demo_items;"
```

Expected signal:

- output is `3`

### 2) Take a Logical Backup (pg_dump)

```bash
docker exec mod20-pg pg_dump -U postgres -Fc -f /tmp/backup.dump
docker cp mod20-pg:/tmp/backup.dump backup.dump
sha256sum backup.dump | tee backup.sha256
ls -la backup.dump backup.sha256
```

Expected:

- backup files exist and are non-empty

### 3) Simulate Data Loss

```bash
docker exec -i mod20-pg psql -U postgres -c "drop table demo_items;"
docker exec -i mod20-pg psql -U postgres -tAc "select count(*) from pg_tables where tablename='demo_items';"
```

Expected:

- output is `0` (table gone)

### 4) Restore and Verify

Copy backup back into container and restore:

```bash
docker cp backup.dump mod20-pg:/tmp/backup.dump
docker exec mod20-pg pg_restore -U postgres -d postgres --clean --if-exists /tmp/backup.dump
```

Verify row count:

```bash
docker exec -i mod20-pg psql -U postgres -tAc "select count(*) from demo_items;"
```

Expected:

- output is `3`

## Verify

```bash
sha256sum -c backup.sha256
docker exec -i mod20-pg psql -U postgres -tAc "select count(*) from demo_items;"
```

Expected signals:

- checksum validation prints `OK`
- row count is `3`

## Cleanup

```bash
docker rm -f mod20-pg 2>/dev/null || true
cd ~
rm -rf ~/work/devops-labs/mod20-postgres
```

## Troubleshooting

### Symptom: cannot connect to Postgres

Diagnosis:

```bash
docker logs --tail 50 mod20-pg
docker exec mod20-pg pg_isready -U postgres || true
```

Fix:

- wait for readiness before running psql/pg_dump

### Symptom: restore fails due to ownership/roles

Fix:

- use `pg_restore` flags appropriate for your environment; for this local lab, use the default `postgres` superuser
