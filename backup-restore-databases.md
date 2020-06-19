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

# Backing up and restoring databases
{: #backup-restore-databases}

You can back up and restore databases in {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} version 1.1.2 by running scripts.
{: shortdesc}

The `all-backup-command.sh` script backs up or restores all the databases and deactivates the pods to prevent access. It then reactivates the pods. However, with the individual database scripts, you must run those procedures.

For more information about backing up databases with previous versions, see [v1.1.1](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases-1.1.1), [v1.0.1](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases-1.0.1), or [v1.0.0](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases-1.0.0). For more information about how to back up and restore workspace data, such as type systems and ground truth, see [Backing up and restoring data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore).
{: tip}

## Before you begin
{: #backup-restore-prereqs}

- Download the [scripts](#scripts-location).
- Review information about the script's use of the MinIO client. The client is used for the for MinIO commands.

  - The scripts download the client from the [MinIO website](https://dl.min.io/) if the client isn't installed.
  - If you want the script to use your installed version, verify that you can run the client by issuing the command `mc` on the command line.

## all-backup-command script
{: #all-backup-ref}

The `all-backup-command script.sh` script backs up or restores the MongoDB, PostgreSQL, Minio databases, and PVC.

Unless you need to back up a single database, use the `all-backup-command` script, which deactivates and reactivates {{site.data.keyword.knowledgestudioshort}}.

The script backs up or restores the data in the following order:

1.  MongoDB
1.  PostgreSQL
1.  MinIO
1.  PVC

```sh
all-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```
{: pre}

Use either the `backup` or `restore` command.

<dl>
  <dt>backup</dt>
  <dd>Backs up each database to a subdirectory of the `BACKUP_DIR` directory.</dd>
  <dt>restore</dt>
  <dd>Restores the data from each database in the `BACKUP_DIR` directory.</dd>
</dl>

### Arguments and options
{: #all-backup-ref-options}

<dl>
  <dt>RELEASE_NAME</dt>
  <dd>The release name that was specified when the {{site.data.keyword.knowledgestudioshort}} Helm chart was installed in your cluster. Required.</dd>
  <dd>For version 1.1.2, the value is `wks`.</dd>
  <dt>BACKUP_DIR</dt>
  <dd>The base directory of each database where backups are stored to or restored from. Each database is stored in a subdirectory of the backup directory (`mongodb`, `postgresql`, `minio`, or `pvc`). Required.</dd>
  <dt>-n NAMESPACE</dt>
  <dd>Namespace for the pods.</dd>
  <dd>The default value is `zen`.</dd>
</dl>

### Output
{: #all-backup-ref-output}

The script returns the following output:

```
[SUCCESS] MongoDB,PostgreSQL,Minio,PVC
```
{: screen}

and indicates either the `backup` or `restore` command.

If the process fails, the following message is displayed.

```
[FAIL] MongoDB,PostgreSQL,Minio,PVC
```
{: screen}

and indicates either the `backup` or `restore` command.

If the script fails, the data is corrupted. Do not use the corrupted data to restore.
{: important}

## Scripts location
{: #scripts-location}

The backup and restore scripts for version 1.1.2 are available from the `knowledge-studio/1.1.2` directory of the [`watson-developer-cloud/doc-tutorial-downloads`](https://github.com/watson-developer-cloud/doc-tutorial-downloads/tree/master/knowledge-studio/1.1.2){: external} GitHub project. Download the scripts and the contents of the `lib` directory.

## Backing up and restoring all databases
{: #all-backups}

The `all-backup-restore.sh` script backs up or restores the databases. The script deactivates the pods before it backs up or restores data and then reactivates the pods when the script completes.

### Backing up all databases
{: #all-backups-backup}

Run the `all-backup-restore.sh` script with the `backup` command.

### Restoring all databases
{: #all-backups-restore}

Run the `all-backup-restore.sh` script with the `restore` command.

## Database-specific scripts
{: #db-specific}

If you need to back up or restore a single database, use one of the database-specific scripts. However, make sure that you deactivate the pods before you run the script and reactivate the pods after the script completes successfully.

### MongoDB
{: #db-mongo}

Use this script instead of `all-backup-command.sh` to back up or restore only the MongoDB database.

```sh
mongodb-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```
{: pre}

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-command script](#all-backup-ref-options).

#### Backing up MongoDB
{: #db-mongo-backup}

Back up your MongoDB data. Databases named `WKSDATA`, `ENVDATA`, and `escloud_sbsep` store data for {{site.data.keyword.knowledgestudioshort}}.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `mongodb-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    1.  Creates a remote temporary file under the mongoDB pod and extracts the following data: `WKSDATA`,`ENVDATA`, and `escloud_sbsep`.
    1.  Copies the `WKSDATA`, `ENVDATA`, and `escloud_sbsep` data to the `BACKUP_DIR` that you specify and deletes the temporary file.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring MongoDB data
{: #db-mongo-restore}

Restore the backed-up data to MongoDB.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `mongodb-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    1.  Create a remote temporary file under the mongoDB pod
    1.  Copies the `WKSDATA` `ENVDATA` `escloud_sbsep` data from the `BACKUP_DIR` that you specify to the remote temporary file.
    1.  Restores the data from the temporary file and deletes the temporary file.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### PostgreSQL
{: #db-postgresql}

Use this script instead of `all-backup-command.sh` to back up or restore only the PostgreSQL database.

```sh
postgresql-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```
{: pre}

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-command script](#all-backup-ref-options).

#### Backing up PostgreSQL
{: #db-postgresql-backup}

Back up your PostgreSQL data by getting a data dump. Databases named `awt`, `jobq_RELEASE_NAME_`,`model_management_api`, and `model_management_api_v2` store data for {{site.data.keyword.knowledgestudioshort}}.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `postgresql-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    1.  Creates and sets up a `.pgpass` file.
    1.  Dumps the databases. The filenames are the database names with the `.custom` extension.
    1.  Copies the dump files to the `BACKUP_DIR` that you specify.
    1.  Deletes the `.pgpass` file.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring PostgreSQL data
{: #db-postgresql-restore}

Restores the backed-up data to PostgreSQL.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `postgresql-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    1.  Creates and sets up a `.pgpass` file.
    1.  Restores the databases by loading the `.custom` files from the `BACKUP_DIR` that you specify.
    1.  Deletes the `.pgpass` file.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### MinIO
{: #db-minio}

Use this script instead of `all-backup-command.sh` to back up or restore only the MinIO database.

```sh
minio-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```
{: pre}

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-command script](#all-backup-ref-options).

#### Backing up MinIO
{: #db-minio-backup}

Back up your MinIO database by taking a snapshot of the data. A bucket named `wks-icp` stores data for {{site.data.keyword.knowledgestudioshort}}.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `minio-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    1.  Establishes a connection to the pod `RELEASE_NAME-ibm-minio` by running `kubectl -n NAMESPACE port-forward`.
    1.  Configures a MinIO alias named `wks-minio`.
    1.  Copies data from `wks-minio/wks-icp` to the BACKUP_DIR you specify.
    1.  Closes the `port-forward` connection.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring MinIO data
{: #db-minio-restore}

Restores the snapshot data to MinIO. Deletes the existing data in the MinIO server, and then restores the backup data.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `minio-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    1.  Establishes a connection to the pod `RELEASE_NAME-ibm-minio` by running `kubectl -n NAMESPACE port-forward`.
    1.  Configures a MinIO alias named `wks-minio`.
    1.  Copies data from the BACKUP_DIR you specify to `wks-minio/wks-icp`.
    1.  Closes the `port-forward` connection.
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### PVC
{: #db-pvc}

Use this script instead of `all-backup-command.sh` to back up or restore only the Persistent volume claim (PVC) data.

```sh
pvc-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR DOCKERREGISTRY PVC_USER_ID -n NAMESPACE
```
{: pre}

#### Arguments for PVC
<dl>
  <dt>DOCKERREGISTRY</dt>
  <dd>The same Docker registry as the `RELEASE_NAME-ibm-watson-ks-aql-web-tooling` pod.</dd>
  <dt>PVC_USER_ID</dt>
  <dd>The user ID for the running containers in the `RELEASE_NAME-ibm-watson-ks-aql-web-tooling` pod.</dd>
</dl>

Use either the `backup` or `restore` command. For more information about the other arguments and options, see the [all-backup-command script](#all-backup-ref-options).

#### Backing up PVC
{: #db-pvc-backup}

1.  Identify the name of Docker registry and user ID before you deactivate {{site.data.keyword.knowledgestudioshort}}.
1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `pvc-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    1.  Creates a temporary pod at `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    Compresses `/opt/ibm/watson/aql-web-tooling/target/sandbox` and saves it as `sandbox.tgz` to `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    1.  Copies `sandbox.tgz` to the BACKUP_DIR that you specify.
    1.  Deletes the temporary pod
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring PVC data
{: #db-pvc-restore}

Restores data to the PVC. Deletes the existing data in the sandbox, and then restores the backup data.

1.  [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1.  Run the `pvc-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    1.  Copies `sandbox.tgz` from the BACKUP_DIR that you specify to a temporary pod at `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    1.  Deletes the data in `/opt/ibm/watson/aql-web-tooling/target/sandbox`.
    1.  Decompresses `sandbox.tgz` to `/opt/ibm/watson/aql-web-tooling/target/sandbox`.
        1.  Deletes the temporary pod
1.  [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

## Deactivate {{site.data.keyword.knowledgestudioshort}}
{: #deactivate-watson-ks}

You don't need to deactivate when you run the `all-backup-restore.sh` script because the script handles the process.
{: tip}

To ensure that users don't have access to {{site.data.keyword.knowledgestudioshort}} when you back up or restore a single database, stop the {{site.data.keyword.knowledgestudioshort}} front-end pods before you back up or restore data.

1.  Make sure that no training and evaluation processes are running. You can check job status with the following command:

    ```sh
    kubectl -n NAMESPACE get jobs
    ```
    {: pre}

    Training jobs of {{site.data.keyword.knowledgestudioshort}} are named in the format `wks-train-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx`, and evaluation jobs are named in the format `wks-batch-apply-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. If the `COMPLETIONS` column of a training job reads `0/1`, that job is still running. Wait until all of the training jobs finish.
1.  List information about the deployment:

    ```sh
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-ks
    kubectl -n NAMESPACE get deployment RELEASE_NAME-sire-training-jobq
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-mma-prod-model-management-api
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-ks-servicebroker
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-ks-aql-web-tooling
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-ks-glimpse-builder
    kubectl -n NAMESPACE get deployment RELEASE_NAME-ibm-watson-ks-glimpse-query
    ```
    {:pre}

    where `NAMESPACE` is the namespace where {{site.data.keyword.knowledgestudioshort}} is deployed and `RELEASE_NAME` is the name that was specified when the {{site.data.keyword.knowledgestudioshort}} Helm chart was installed in your cluster.

    Make sure to note the number of pods in the `DESIRED` column so you can restore the same number later.
    {: important}

1.  Temporarily stop the pods by issuing the following commands. Make sure you know the number of pods from the previous step.

    ```sh
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-sire-training-jobq --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-mma-prod-model-management-api --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-servicebroker --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-aql-web-tooling --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-glimpse-builder --replicas=0
    kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-glimpse-query --replicas=0
    ```
    {: pre}

## Reactivate {{site.data.keyword.knowledgestudioshort}}
{: #reactivate-watson-ks}

You don't need to reactivate when you run the `all-backup-restore.sh` script because the script handles the process.
{: tip}

To activate {{site.data.keyword.knowledgestudioshort}}, start that pods that you stopped before you began backing up or restoring data. If you stopped pods with the `kubectl scale` command, you can start the pods with the following commands:

```sh
kubectl -n NAMESPACE scale deployment RELEASE_NAME-sire-training-jobq --replicas=JOBQ_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-mma-prod-model-management-api --replicas=MMA_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks --replicas=FRONTEND_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-servicebroker --replicas=BROKER_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-aql-web-tooling --replicas=AWT_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-glimpse-builder --replicas=GLIMPSE_BUILDER_PODS
kubectl -n NAMESPACE scale deployment RELEASE_NAME-ibm-watson-ks-glimpse-query --replicas=GLIMPSE_QUERY_PODS
```
{: pre}

where the values refer to the number of pods that you [deactivated](#deactivate-watson-ks) earlier:

- `FRONTEND_PODS`: The number of **front-end** pods that you stopped.
- `JOBQ_PODS`: The number of **SIRE** job queue pods that you stopped.
- `MMA_PODS`: The number of **MMA** pods that you stopped.
- `BROKER_PODS`: The number of **Service Broker** pods that you stopped.
- `AWT_PODS`: The number of **AQL Web Tooling** pods that you stopped.
- `GLIMPSE_BUILDER_PODS`: The number of **Glimpse builder** pods that you stopped.
- `GLIMPSE_QUERY_PODS`: The number of **Glimpse query** pods that you stopped.
