1) ./test.curl




CREATE TABLE outbox_events (id TEXT PRIMARY KEY, name TEXT, age INT);

INSERT INTO outbox_events (id, name, age) VALUES ('5', 'fred', 34);
INSERT INTO outbox_events (id, name, age) VALUES ('7', 'sue', 25);
INSERT INTO outbox_events (id, name, age) VALUES ('2', 'bill', 51);

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
