---

copyright:
  years: 2019, 2021
lastupdated: "2022-03-08"

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

You can back up and restore databases in {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} version 4.0.x by running scripts.
{: shortdesc}

The `all-backup-restore.sh` script backs up or restores all the databases and deactivates the pods to prevent access. It then reactivates the pods. However, with the individual database scripts, you must run individual procedures.

For more information about backing up databases with previous versions, see [v1.1.2](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases-1.1.2) or [v1.2.0](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases-1.2.0). For more information about how to back up and restore workspace data, such as type systems and ground truth, see [Backing up and restoring data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore).
{: tip}

Using a backup from a different instance will cause errors. You should only restore a backup which is created from the same {{site.data.keyword.knowledgestudiofull}} instance.
{: important}

## Before you begin
{: #backup-restore-prereqs}

- Download the [scripts](https://github.com/watson-developer-cloud/doc-tutorial-downloads/tree/master/knowledge-studio).
- Review information about the script's use of the MinIO client. The client is required for the MinIO commands.

  - The scripts download the client from the [MinIO website](https://dl.min.io/) if the client isn't installed.
  - If you want the script to use your installed version, verify that you can run the client by issuing the command `mc` on the command line.

## all-backup-restore script
{: #all-backup-ref}

The `all-backup-restore.sh` script backs up and restores the MongoDB, PostgreSQL, and Minio databases, and PVC.

Unless you only need to back up a single database, it is recommended that you use the `all-backup-restore` script, which deactivates and reactivates {{site.data.keyword.knowledgestudioshort}}.

The script backs up or restores the data in the following order:

1. MongoDB
1. PostgreSQL
1. MinIO
1. PVC

```sh
all-backup-restore.sh [backup|restore] [RELEASE_NAME] [BACKUP_DIR] [-n NAMESPACE]
```

Use either the `backup` or `restore` command.

- **backup**

  Backs up each database to a subdirectory of the `BACKUP_DIR` directory.

- **restore**

  Restores the data from each database in the `BACKUP_DIR` directory.

### Arguments and options
{: #all-backup-ref-options}

- **RELEASE_NAME**

  Required. The release name that was specified when the {{site.data.keyword.knowledgestudioshort}} Helm chart was installed in your cluster.

  You can find the release name as the prefix of the pod name, for example, {release_name}-ibm-watson-ks-yyy-xxx. For version 1.2.0, the value is always `wks`.

- **BACKUP_DIR**

  Required. The base directory of each database where backups are stored to or restored from. Each database is stored in a subdirectory of the backup directory. A new folder with timestamp `wks-backup-yyyymmdd_hhmmss` will be created under [backupDir], for example: [backupDir]/wks-backup-yyyymmdd_hhmmss/mongodb

  For restore: please set [backupDir] with `wks-backup-yyyymmdd_hhmmss`, for example: [backupDir]/mongodb

- **-n NAMESPACE**

  Namespace for the pods. The default value is `zen`.

### Output
{: #all-backup-ref-output}

The script returns the following output, indicating either the `backup` or `restore` command:

```sh
[SUCCESS] MongoDB,PostgreSQL,Minio,PVC (backup|restore)
```

If the process fails, the following message is displayed, indicating either the `backup` or `restore` command:

```sh
[FAIL] MongoDB,PostgreSQL,Minio,PVC (backup|restore)
```

If the script fails, the data is corrupted. Do not use the corrupted data to restore.
{: important}

## Database-specific scripts
{: #db-specific}

If you need to back up or restore a single database, use one of the database-specific scripts. However, make sure that you deactivate the pods before you run the script, and reactivate the pods after the script completes successfully.

### MongoDB
{: #db-mongo}

Use this script instead of `all-backup-restore.sh` to back up or restore only the MongoDB database.

```sh
mongodb-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-restore script](#all-backup-ref-options).

#### Backing up MongoDB
{: #db-mongo-backup}

Back up your MongoDB data. Databases named `WKSDATA`, `ENVDATA`, and `escloud_sbsep` store data for {{site.data.keyword.knowledgestudioshort}}.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `mongodb-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    - Creates a remote temporary file under the mongoDB pod and extracts the following data: `WKSDATA`,`ENVDATA`, and `escloud_sbsep`.
    - Copies the `WKSDATA`, `ENVDATA`, and `escloud_sbsep` data to the `BACKUP_DIR` that you specify and deletes the temporary file.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring MongoDB data
{: #db-mongo-restore}

Restore the backed-up data to MongoDB.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `mongodb-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    - Create a remote temporary file under the mongoDB pod
    - Copies the `WKSDATA` `ENVDATA` `escloud_sbsep` data from the `BACKUP_DIR` that you specify to the remote temporary file.
    - Restores the data from the temporary file and deletes the temporary file.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### PostgreSQL
{: #db-postgresql}

Use this script instead of `all-backup-restore.sh` to back up or restore only the PostgreSQL database.

```sh
postgresql-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-restore script](#all-backup-ref-options).

#### Backing up PostgreSQL
{: #db-postgresql-backup}
Back up your PostgreSQL data by getting a data dump.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `postgresql-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    - Creates a job for the `postgresql` backup.
    - Dumps the databases. The filenames are the database names, such as `jobq_{release_name_underscore}`, `model_management_api` , `model_management_api_v2` and `awt`, with the `.custom` extension appended.
    - Copies the dump files to the local `[backupDir]` that you specify.
    - Deletes the `.pgpass` file.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring PostgreSQL data
{: #db-postgresql-restore}
Restores the backed-up data to PostgreSQL.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `postgresql-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    - Creates a job for `postgresql` restore.
    - Restores the databases (`jobq_{release_name_underscore}`, `model_management_api` , `model_management_api_v2` and `awt`) by loading the `.custom` files from the `[backupDir[` that you specify. 
    - Restores the databases (`jobq_{release_name_underscore}`, `model_management_api` , `model_management_api_v2` and `awt`) by loading the `.custom` files from the `[backupDir]` that you specify. 
    - Deletes the `.pgpass` file.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### MinIO
{: #db-minio}

Use this script instead of `all-backup-restore.sh` to back up or restore only the MinIO database.

```sh
minio-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR -n NAMESPACE
```

Use either the `backup` or `restore` command. For more information about the arguments and options, see the [all-backup-restore script](#all-backup-ref-options).

#### Backing up MinIO
{: #db-minio-backup}

Back up your MinIO database by taking a snapshot of the data. A bucket named `wks-icp` stores data for {{site.data.keyword.knowledgestudioshort}}.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `minio-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    - Establishes a connection to the pod `RELEASE_NAME-ibm-minio` by running `kubectl -n NAMESPACE port-forward`.
    - Configures a MinIO alias named `wks-minio`.
    - Copies data from `wks-minio/wks-icp` to the BACKUP_DIR you specify.
    - Closes the `port-forward` connection.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring MinIO data
{: #db-minio-restore}

Restores the snapshot data to MinIO. Deletes the existing data in the MinIO server, and then restores the backup data.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `minio-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    - Establishes a connection to the pod `RELEASE_NAME-ibm-minio` by running `kubectl -n NAMESPACE port-forward`.
    - Configures a MinIO alias named `wks-minio`.
    - Copies data from the BACKUP_DIR you specify to `wks-minio/wks-icp`.
    - Closes the `port-forward` connection.
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

### PVC
{: #db-pvc}

Use this script instead of `all-backup-restore.sh` to back up or restore only the Persistent volume claim (PVC) data.

```sh
pvc-backup-restore.sh backup|restore RELEASE_NAME BACKUP_DIR DOCKERREGISTRY PVC_USER_ID -n NAMESPACE
```

#### Arguments for PVC

- **DOCKERREGISTRY**

  The same Docker registry as the `RELEASE_NAME-ibm-watson-ks-aql-web-tooling` pod.

- **PVC_USER_ID**

  The user ID for the running containers in the `RELEASE_NAME-ibm-watson-ks-aql-web-tooling` pod.

Use either the `backup` or `restore` command. For more information about the other arguments and options, see the [all-backup-restore script](#all-backup-ref-options).

#### Backing up PVC
{: #db-pvc-backup}

1. Identify the name of Docker registry and user ID before you deactivate {{site.data.keyword.knowledgestudioshort}}.
1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `pvc-backup-restore.sh` script with the `backup` command. The script runs the following operations:
    - Creates a temporary pod at `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    Compresses `/opt/ibm/watson/aql-web-tooling/target/sandbox` and saves it as `sandbox.tgz` to `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    - Copies `sandbox.tgz` to the BACKUP_DIR that you specify.
    - Deletes the temporary pod
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

#### Restoring PVC data
{: #db-pvc-restore}

Restores data to the PVC. Deletes the existing data in the sandbox, and then restores the backup data.

1. [Deactivate](#deactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.
1. Run the `pvc-backup-restore.sh` script with the `restore` command. The script runs the following operations:
    - Copies `sandbox.tgz` from the BACKUP_DIR that you specify to a temporary pod at `RELEASE_NAME-ibm-watson-ks-aql-web-tooling-backup`.
    - Deletes the data in `/opt/ibm/watson/aql-web-tooling/target/sandbox`.
    - Decompresses `sandbox.tgz` to `/opt/ibm/watson/aql-web-tooling/target/sandbox`.
    - Deletes the temporary pod
1. [Reactivate](#reactivate-watson-ks) {{site.data.keyword.knowledgestudioshort}}.

## Deactivate {{site.data.keyword.knowledgestudioshort}}
{: #deactivate-watson-ks}

You don't need to deactivate when you run the `all-backup-restore.sh` script because the script handles the process.
{: note}

To ensure that users don't have access to {{site.data.keyword.knowledgestudioshort}} when you back up or restore a single database, stop the {{site.data.keyword.knowledgestudioshort}} front-end pods before you back up or restore data.

1. Make sure that no training and evaluation processes are running. You can check job status with the following command:

    ```sh
    kubectl -n NAMESPACE get jobs
    ```
    {: pre}

    Training jobs of {{site.data.keyword.knowledgestudioshort}} are named in the format `wks-train-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx`, and evaluation jobs are named in the format `wks-batch-apply-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. If the `COMPLETIONS` column of a training job reads `0/1`, that job is still running. Wait until all of the training jobs finish.
1. Deactivate {{site.data.keyword.knowledgestudioshort}} using the folowing commands:

    ```sh
    kubectl -n NAMESPACE patch --type=merge wks wks -p '{"spec":{"global":{"quiesceMode":true}}}'
    kubectl -n NAMESPACE patch --type=merge wks wks -p '{"spec":{"mma":{"replicas":0}}}'
    ```
    {:pre}
1. Ensure that no {{site.data.keyword.knowledgestudioshort}} pods exist, except datastore pods, by running the following command (this may takes few minutes):

    ```sh
    kubectl -n NAMESPACE get pod | grep -Ev 'minio|etcd|mongo|postgresql|gw-instance|Completed' | grep wks
    ```
    {: pre}

## Reactivate {{site.data.keyword.knowledgestudioshort}}
{: #reactivate-watson-ks}

You don't need to reactivate when you run the `all-backup-restore.sh` script because the script handles the process.
{: tip}

To reactivate {{site.data.keyword.knowledgestudioshort}}, use the following command:

```sh
kubectl -n NAMESPACE patch --type=merge wks wks -p '{"spec":{"global":{"quiesceMode":false}}}'
kubectl -n NAMESPACE patch --type=merge wks wks -p '{"spec":{"mma":{"replicas":2}}}'
```
{: pre}
