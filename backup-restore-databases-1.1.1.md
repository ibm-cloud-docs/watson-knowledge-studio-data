---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-19"

subcollection: watson-knowledge-studio-data

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Backing up and restoring databases  (version 1.1.1)
{: #backup-restore-databases-1.1.1}

This document describes how to backup and restore databases in {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} version 1.1.1. To backup and restore workspace data such as type systems and ground truth, see [Backing up and restoring data](https://cloud.ibm.com/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore).
{: important}

## Overview
{: #overview-databases}

- Deactivate {{site.data.keyword.knowledgestudioshort}}
- Follow backup or restore procedures for each storage system
  - MongoDB
  - PostgreSQL
  - MinIO
- Reactivate {{site.data.keyword.knowledgestudioshort}}

These instructions assume that administrators replace data in the databases with backup data instead of appending backup data.
{: note}

## Step 1: Deactivate {{site.data.keyword.knowledgestudioshort}}
{: #deactivate-knowledge-studio}

1. Deactivate the {{site.data.keyword.knowledgestudioshort}} front-end.

    To ensure that users do not have access to {{site.data.keyword.knowledgestudioshort}} during backup, stop the {{site.data.keyword.knowledgestudioshort}} front-end pods.
    Before stopping the pods, note the number of pods and make sure to restore the same number after backup.

    You can temporarily stop pods with the following commands.

    ```bash
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-ks 
    ```
    {: pre}

    Make sure to note the number of pods in the DESIRED column so you can restore the same number later.
    {: note}

    ```bash 
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks --replicas=0
    ```
    {: pre}

    - `{namespace}`: The namespace where {{site.data.keyword.knowledgestudioshort}} is deployed
    - `{release_name}`: Helm release name of {{site.data.keyword.knowledgestudioshort}} in your cluster

2. Make sure that no training and evaluation processes are running.

    Confirm that there are no running jobs. For example, you can check job status with the following command.

    ```bash
    kubectl -n {namespace} get jobs
    ```
    {: pre}

    Training jobs of {{site.data.keyword.knowledgestudioshort}} are named in the format `wks-train-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx`, and evaluation jobs are named in the format `wks-batch-apply-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.
    If the `COMPLETIONS` column of a training job reads `0/1`, that job is still running. Wait until all of the training jobs finish.

3. Deactivate pods.

    To avoid data corruption, stop MMA, SIRE job queue, and Service Broker, AQL Web Tooling, and Glimpse pods.
    Before stopping the pods, memorize the number of pods to restore the same number of pods after backup.

    For example, you can stop the pods temporarily with the following commands.

    ```bash
    kubectl -n {namespace} get deployment {release_name}-sire-training-jobq 
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-mma-prod-model-management-api
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-ks-servicebroker
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-ks-aql-web-tooling
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-ks-glimpse-builder
    kubectl -n {namespace} get deployment {release_name}-ibm-watson-ks-glimpse-query
    ```
    {: pre}

    Make sure to note the number of pods in the DESIRED column for the deployments so you can restore the same number later.
    {: note}

    ```bash
    kubectl -n {namespace} scale deployment {release_name}-sire-training-jobq --replicas=0
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-mma-prod-model-management-api --replicas=0
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-servicebroker --replicas=0
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-aql-web-tooling --replicas=0
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-glimpse-builder --replicas=0
    kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-glimpse-query --replicas=0
    ```
    {: pre}

## Step 2: Follow the backup or restore procedure for MongoDB
{: #backup-restore-mongodb}

In MongoDB, databases named `WKSDATA`, `ENVDATA`, and `escloud_sbsep` store data for {{site.data.keyword.knowledgestudioshort}}.

### Obtain authentication information for MongoDB
{: #obtain-mongodb-authentication}

The MongoDB username and password are base64 encoded and stored into kubernetes secret named `{release_name}-ibm-mongodb-auth-secret`. You can retrieve the username and password with the following commands.

```bash
# username
kubectl -n {namespace} get secret {release_name}-ibm-mongodb-auth-secret -o json | jq .data.user | tr -d '"' | base64 --decode

# password
kubectl -n {namespace} get secret {release_name}-ibm-mongodb-auth-secret -o json | jq .data.password | tr -d '"' | base64 --decode
```
{: pre}

After you obtain the username and password, you are ready to [backup](#backup-mongodb) or [restore](#restore-mongodb) MongoDB data.

### Backing up MongoDB data
{: #backup-mongodb}

If you are performing a backup, you can use `mongodump` (See <https://docs.mongodb.com/v3.4/tutorial/backup-and-restore-tools/>). You can retrieve the backup data with the following commands.

```bash
# In client machine
kubectl -n {namespace} exec -it {release_name}-ibm-mongodb-server-0 -- bash

# In pod
mkdir /home/mongodb/{output_dir}
mongodump --ssl --sslAllowInvalidCertificates -u {username} -p {password} --authenticationDatabase admin --db=WKSDATA -o /home/mongodb/{output_dir}
mongodump --ssl --sslAllowInvalidCertificates -u {username} -p {password} --authenticationDatabase admin --db=ENVDATA -o /home/mongodb/{output_dir}
mongodump --ssl --sslAllowInvalidCertificates -u {username} -p {password} --authenticationDatabase admin --db=escloud_sbsep -o /home/mongodb/{output_dir}

exit

# In client machine
kubectl -n {namespace} cp {release_name}-ibm-mongodb-server-0:/home/mongodb/{output_dir} {local_dir}
```
{: pre}


You should not backup MongoDB user data (you should not use `--dumpDbUsersAndRoles` and `--restoreDbUsersAndRoles` options of `mongodump` and `mongorestore` respectively) because the username and password internally used in {{site.data.keyword.knowledgestudioshort}} is automatically generated during the deployment.
If you restore and overwrite the MongoDB username and password manually, the connection between the components will be refused.
{: note}

### Restoring MongoDB data
{: #restore-mongodb}

If you are restoring data, you can use `mongorestore` (See <https://docs.mongodb.com/v3.4/tutorial/backup-and-restore-tools/>). For example, you can restore backed up data by executing the following commands.

```bash
# In client machine
kubectl -n {namespace} cp {local_dir} {release_name}-ibm-mongodb-server-0:/home/mongodb/{remote_dir}
kubectl -n {namespace} exec -it {release_name}-ibm-mongodb-server-0 -- bash

# In pod
mongorestore --drop --ssl --sslAllowInvalidCertificates -u {username} -p {password} --authenticationDatabase admin --dir /home/mongodb/{remote_dir}
exit
```
{: pre}

You should not backup MongoDB user data (you should not use `--dumpDbUsersAndRoles` and `--restoreDbUsersAndRoles` option of `mongodb` and `mongorestore`) because the username and password internally used in {{site.data.keyword.knowledgestudioshort}} are automatically generated during the deployment. If you restore and overwrite the MongoDB username and password manually, the connection between the components will be refused.
{: note}

## Step 3: Follow the backup or restore procedure for PostgreSQL 
{: #backup-restore-postgresql}

In PostgreSQL, databases named `awt`, `jobq_{release_name_underscore}`,`model_management_api`, and `model_management_api_v2` store data for {{site.data.keyword.knowledgestudioshort}}.

### Obtain PostgreSQL authentication information
{: #obtain-postgresql-authentication}

The username of administrator is `stolon`, and the password is base64 encoded and stored into kubernetes secret named `{release_name}-ibm-postgresql-auth-secret`.

You can retrieve the password with the following command.
```bash
kubectl -n {namespace} get secret -o json {release_name}-ibm-postgresql-auth-secret | jq .data.pg_su_password | tr -d '"' | base64 --decode
```
{: pre}

After you obtain the username and password, you are ready to [backup](#backup-postgresql) or [restore](#restore-postgresql) PostgreSQL data.

### Backing up PostgreSQL data
{: #backup-postgresql}

If you are performing a backup, You can use [pg_dump](https://www.postgresql.org/docs/9.6/app-pgdump.html) and [pg_dumpall](https://www.postgresql.org/docs/9.6/app-pg-dumpall.html). For example, you can retrieve backup data by executing the following commands.

```bash
# In client machine
kubectl -n {namespace} get pods | grep {release_name}-ibm-postgresql-proxy # Choose a pod
kubectl -n {namespace} exec -it {release_name}-ibm-postgresql-proxy-xxxxxxxxxx-yyyyy bash

# In pod
mkdir /home/stolon/{output_dir}

## Each pg_dump command requires your password
pg_dump -h localhost -p 5432 -U stolon --clean jobq_{release_name_underscore} > /home/stolon/{output_dir}/jobq_wks_{release_name_underscore}.sql
pg_dump -h localhost -p 5432 -U stolon --clean model_management_api > /home/stolon/{output_dir}/model_management_api.sql
pg_dump -h localhost -p 5432 -U stolon --clean model_management_api_v2 > /home/stolon/{output_dir}/model_management_api_v2.sql
pg_dump -h localhost -p 5432 -U stolon --clean awt > /home/stolon/{output_dir}/awt.sql

exit

# In client machine
kubectl -n {namespace} cp {release_name}-ibm-postgresql-proxy-xxxxxxxxxx-yyyyy:/home/stolon/{output_dir}/ {local_dir}
```
{: pre}

- `{release_name_underscore}`: `{release_name}` where `-` is replaced with  `_`. For example, if `{release_name}` is `my-release`, `{release_name_underscore}` is `my_release`.

You should not backup the PostgreSQL username and password because the username and password internally used in {{site.data.keyword.knowledgestudioshort}} are automatically generated during the deployment.
If you restore and overwrite the PostgreSQL username and password manually, the connection between the components will be refused.
{: note}

### Restoring PostgreSQL data
{: #restore-postgresql}

If you are restoring data, you can use [psql](https://www.postgresql.org/docs/9.6/app-psql.html) and [pg_restore](https://www.postgresql.org/docs/9.6/app-pgrestore.html). For example, you can retrieve backup data by executing the following commands.

```bash
# In client machine
kubectl -n {namespace} get pods | grep {release_name}-ibm-postgresql-proxy # Choose a pod
kubectl -n {namespace} cp {local_dir} {release_name}-ibm-postgresql-proxy-xxxxxxxxxx-yyyyy:/home/stolon/{remote_dir}
kubectl -n {namespace} exec -it {release_name}-ibm-postgresql-proxy-xxxxxxxxxx-yyyyy bash

# In pod
## Each psql command requires your password
psql jobq_wks_{release_name_underscore} -h localhost -p 5432 -U stolon < /home/stolon/{remote_dir}/jobq_{release_name_underscore}.sql
psql model_management_api -h localhost -p 5432 -U stolon < /home/stolon/{remote_dir}/model_management_api.sql
psql model_management_api_v2 -h localhost -p 5432 -U stolon < /home/stolon/{remote_dir}/model_management_api_v2.sql
psql awt -h localhost -p 5432 -U stolon < /home/stolon/{remote_dir}/awt.sql

exit
```
{: pre}

You should not backup the PostgreSQL username and password because the username and password internally used in {{site.data.keyword.knowledgestudioshort}} are automatically generated during the deployment.
If you restore and overwrite the PostgreSQL username and password manually, the connection between the components will be refused.
{: note}

## Step 4: Follow the backup or restore procedure for MinIO
{: #backup-restore-minio-1.0.0-2}

In MinIO, a bucket named `wks-icp` stores data for {{site.data.keyword.knowledgestudioshort}}.

### Obtain authentication information
{: #obtain-minio-authentication}

The MinIO access key and secret key, which are needed to connect the MinIO server, are base64 encoded and stored into kubernetes secret named `minio-access-secret-{release_name}`. You can retrieve the username and password with the following commands.

```bash
# access key
kubectl -n {namespace} get secret -o json minio-access-secret-{release_name} | jq .data.accesskey | tr -d '"' | base64 --decode

# secret key
kubectl -n {namespace} get secret -o json minio-access-secret-{release_name} | jq .data.secretkey | tr -d '"' | base64 --decode
```
{: pre}

### Backing up MinIO data
{: #backup-minio}

MinIO allows data to be copied from the server through several client applications such as [MinIO Client](https://docs.min.io/docs/minio-client-quickstart-guide.html), [S3cmd](https://docs.min.io/docs/s3cmd-with-minio.html), [AWS CLI](https://docs.min.io/docs/aws-cli-with-minio.html), and [restic](https://docs.min.io/docs/restic-with-minio.html). For example, you can retrieve backup data by executing the following MinIO Client commands.
    
```bash
# In client machine
kubectl -n {namespace} port-forward {release_name}-ibm-minio-1 9000:9000
mc --insecure config host add {alias} https://localhost:9000 {access_key} {secret_key}
mc --insecure cp -r {alias}/wks-icp {local_dir}
```
{: pre}

- `{alias}`: Choose a name for the alias of your minio server (for example, "wks-minio").

### Restoring MinIO data
{: #restore-minio}

You can restore backup data by executing the following commands.

The following commands delete existing data in the MinIO server, then restore backup data.
{: note}

```bash
# In client machine
kubectl -n {namespace} port-forward {release_name}-ibm-minio-1 9000:9000
mc --insecure config host add {alias} https://localhost:9000 {access_key} {secret_key}
mc --insecure cp -r {local_dir} {alias}/wks-icp
```
{: pre}

- `{alias}`: Choose a name for the alias of your minio server (for example, "wks-minio")


## Step 5: Reactivate {{site.data.keyword.knowledgestudioshort}}
{: #reactivate-the-add-on}

To activate {{site.data.keyword.knowledgestudioshort}}, start that pods that you stopped before you began backing up or restoring data.
If you stopped pods with the `kubectl scale` command, you can start the pods with the following commands.

``` bash
kubectl -n {namespace} scale deployment {release_name}-sire-training-jobq --replicas={num_jobq_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-mma-prod-model-management-api --replicas={num_mma_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks --replicas={num_frontend_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-servicebroker --replicas={num_broker_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-aql-web-tooling --replicas={num_awt_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-glimpse-builder --replicas={num_glimpse_builder_pods}
kubectl -n {namespace} scale deployment {release_name}-ibm-watson-ks-glimpse-query --replicas={num_glimpse_query_pods}
```
{: pre}

- `{num_frontend_pods}`: The number of front-end pods that you stopped in step 1.1.
- `{num_jobq_pods}`: The number of SIRE job queue pods that you stopped in step 1.3.
- `{num_mma_pods}`: The number of MMA pods that you stopped in step 1.3.
- `{num_broker_pods}`: The number of Service Broker pods that you stopped in step 1.3.
- `{num_awt_pods}`: The number of AQL Web Tooling pods that you stopped in step 1.3.
- `{num_glimpse_builder_pods}`: The number of Glimpse builder pods that you stopped in step 1.3.
- `{num_glimpse_query_pods}`: The number of Glimpse query pods that you stopped in step 1.3.

