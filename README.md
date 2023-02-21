1) ./test.curl


CREATE TABLE IF NOT EXISTS outbox_events
(
    event_id       UUID NOT NULL CONSTRAINT outbox_events_pkey PRIMARY KEY,
    event_type     VARCHAR,
    trace_id       UUID NOT NULL,
    aggregate_id   UUID,
    aggregate_type VARCHAR,
    topic          VARCHAR,
    payload        JSONB,
    created_by     VARCHAR,
    created_at     TIMESTAMP,
    status         VARCHAR
);
insert into outbox_events (event_id, event_type, trace_id, aggregate_id, aggregate_type, topic, payload, created_by,
                           created_at, status)
values ('6d246cfd-25fd-4b9c-9f2d-dc68fbf15dea','workspace_task_created','83e37e35-a1c9-4632-8c5c-4091f0a0538f','1ab68164-2bbf-401d-bd36-6169c2e305f8'
,'Workspace','Workspace','{"before":null,"after":{"createdAt":"2022-10-03T10:10:30.410170Z","lastModifiedAt":"2022-10-03T10:10:30.410170Z","createdBy":"SYSTEM","lastModifiedBy":"SYSTEM","name":"TITLE_SEARCH","taskId":"1ab68164-2bbf-401d-bd36-6169c2e305f8","status":"COMPLETED"}}',current_timestamp,current_timestamp,'SENT');


CREATE TABLE customers (id TEXT PRIMARY KEY, name TEXT, age INT);

INSERT INTO customers (id, name, age) VALUES ('5', 'fred', 34);
INSERT INTO customers (id, name, age) VALUES ('7', 'sue', 25);
INSERT INTO customers (id, name, age) VALUES ('2', 'bill', 51);

   <!-- docker exec -it postgres /bin/bash -->

<!-- psql -U postgres-user customers -->

curl -X DELETE http://localhost:8083/connectors/s3_connector

<!-- alter system set wal_level to 'replica';

docker exec -ti 4a10f43aad19 bash

apt-get update && apt-get install postgresql-13-wal2json -->

PostgreSQL on Amazon RDS
It is possible to capture changes in a PostgreSQL database that is running in Amazon RDS. To do this:

Set the instance parameter rds.logical_replication to 1.

Verify that the wal_level parameter is set to logical by running the query SHOW wal_level as the database RDS master user. This might not be the case in multi-zone replication setups. You cannot set this option manually. It is automatically changed when the rds.logical_replication parameter is set to 1. If the wal_level is not set to logical after you make the preceding change, it is probably because the instance has to be restarted after the parameter group change. Restarts occur during your maintenance window, or you can initiate a restart manually.

Set the Debezium plugin.name parameter to pgoutput.

Initiate logical replication from an AWS account that has the rds_replication role. The role grants permissions to manage logical slots and to stream data using logical slots. By default, only the master user account on AWS has the rds_replication role on Amazon RDS. To enable a user account other than the master account to initiate logical replication, you must grant the account the rds_replication role. For example, grant rds_replication to <my_user>. You must have superuser access to grant the rds_replication role to a user. To enable accounts other than the master account to create an initial snapshot, you must grant SELECT permission to the accounts on the tables to be captured. For more information about security for PostgreSQL logical replication, see the PostgreSQL documentation.