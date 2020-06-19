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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Managing the cluster
{: #manage}

## Preparing your local machine
{: #manage-prep-local-machine}

Ensure that you have the following prerequisites installed and working correctly on your local machine before performing any cluster-management tasks.

1.  Install and configure the following command-line tools:

    - [`cloudctl`](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.2/manage_cluster/install_cli.html){: external}
    - [`kubectl`](https://docs-icpdata.mybluemix.net/docs/content/SSQNUZ_current/com.ibm.icpdata.doc/zen/install/kubectl-access.html){: external}
    - [`helm`](https://helm.sh){: external}
1.   Start `helm`:

    ```sh
    helm init --client-only
    ```
    {: pre}
1.  Verify that the tools are installed correctly by running the following test commands.

    - Test the IBM Cloud Private CLI (`cloudctl`):

      ```sh
      cloudctl login -a https://{hostname}:8443 -u {admin_user_id} -p {admin_password}
      ```
      {: pre}

      If you are using a load balancer, specify the hostname of the load balancer instead of the hostname of the master node.
      {: note}

    - Test Kubernetes (`kubectl`):

      ```sh
      kubectl get namespaces
      ```
      {: pre}

      If you cannot run the `kubectl` command, see [Enabling access to the Kubernetes command-line interface](https://www.ibm.com/support/knowledgecenter/SSQNUZ_2.1.0/com.ibm.icpdata.doc/zen/install/kubectl-access.html){: external}.
      {: note}

    - Test Helm (`helm`):

      ```sh
      helm version --tls
      ```
      {: pre}

## Managing user access
{: #manage-users}

After you provision an instance, you can share the URL for the product user interface with other users. However, those users can only log in to the product user interface if you give them access.

If you plan to use SAML for single sign-on (SSO), complete [Configuring single sign-on](https://www.ibm.com/support/knowledgecenter/SSQNUZ_2.1.0/com.ibm.icpdata.doc/zen/admin/saml-sso.html#saml-sso) before you add users. If you add users before you configure SSO, you will need to re-add the users with their SAML ID to enable them to use SSO.

1.  From the web client menu, click **Administer > Manage user**.
1.  Click **Add user**, and specify the user's full name, user name, and email address. Set the user's permissions, and then click **Add**.
1.  From the web client menu, select **My Instances**.
1.  Find your {{site.data.keyword.knowledgestudioshort}} instance, click the more (**...**) menu, and then choose **Manage Access**.
1.  Click **Add user**.
1.  Click the user name field to see a list of the people you can add.

    The users you added in the previous steps are listed. Select a name, choose their access role, and then click **Add**. 

    If you aren't connecting to an existing user registry and enabling single sign-on, then temporary passwords are created for the users you add and are sent to them by way of the email addresses you specified.

## Scaling Deployments and StatefulSets
{: #scaling}

### Deployments
{: #deployments}

|     Component     |                      Deployment name                      |                                  Pod name                                  | Default number of replicas |
| :---------------: | --------------------------------------------------------- | -------------------------------------------------------------------------- | :------------------------: |
|   WKS Front-end   | `{release_name}-ibm-watson-ks`                            | `{release_name}-ibm-watson-ks-xxxxxxxxxx-xxxxx`                            |             2              |
|       SIREG       | `{release_name}-sire-training-sireg-{lang}`               | `{release_name}-sire-training-sireg-{lang}-xxxxxxxxxx-xxxxx`               |             2              |
|  SIRE Job queue   | `{release_name}-sire-training-jobq`                       | `{release_name}-sire-training-jobq-xxxxxxxxxx-xxxxx`                       |             2              |
| SIRE Train Facade | `{release_name}-sire-training-facade`                     | `{release_name}-sire-training-facade-xxxxxxxxxx-xxxxx`                     |             2              |
|        MMA        | `{release_name}-ibm-watson-mma-prod-model-management-api` | `{release_name}-ibm-watson-mma-prod-model-management-api-xxxxxxxxxx-xxxxx` |             2              |
|   Watson Add-on   | `{release_name}-wcn-addon-addon`                          | `{release_name}-wcn-addon-addon-xxxxxxxxxx-xxxxx`                          |             2              |
|    PostgreSQL     | `{release_name}-ibm-postgresql-proxy`                     | `{release_name}-ibm-postgresql-proxy-xxxxxxxxxx-xxxxx`                     |             2              |
|    PostgreSQL     | `{release_name}-ibm-postgresql-sentinel`                  | `{release_name}-ibm-postgresql-sentinel-xxxxxxxxxx-xxxxx`                  |             2              |

- `{release name}` is the Helm release name of your installation.
- `{lang}` is the language name (`en`, `ar`, `de` etc..) of the SIREG tokenizer deployment. Deployments are created for each language.

To scale up/down the number of replicas for Deployments, use the `kubectl scale` command.

```sh
kubectl scale deployment/{deployment_name} --replicas={count}
```

- `{deployment_name}` is the name of the Deployment you want to scale, which should be one of the Deployment names listed above.
- `{count}` is the expected number of replicas to be scaled.

For example, suppose that you want to scale up the Deployment of WKS Front-end associated with the release `my-release` from 2 to 3 replicas, the command will be like following.

```sh
kubectl scale deployment/my-release-ibm-watson-ks --replicas=3
```

### StatefulSets
{: #statefulsets}

| Component  |        StatefulSet name         |                                                     Pod name                                                      | Default number of replicas |
| :--------: | ------------------------------- | ----------------------------------------------------------------------------------------------------------------- | :------------------------: |
| PostgreSQL | `{release_name}-ib-xxxx-keeper` | `{release_name}-ib-xxxx-keeper-0`, `{release_name}-ib-xxxx-keeper-1`                                              |             2              |
|  MongoDB   | `{release_name}-ib-xxxx-server` | `{release_name}-ib-xxxx-server-0`, `{release_name}-ib-xxxx-server-1`                                              |             2              |
|   Minio    | `{release_name}-minio`          | `{release_name}-minio-0`, `{release_name}-ib-minio-1`<br>`{release_name}-ib-minio-2`, `{release_name}-ib-minio-3` |             4              |

To scale up/down the number of replicas for StatefulSets, use `kubectl scale` command.

```sh
kubectl scale statefulset/{statefulset_name} --replicas={count}
```

- `{deployment_name}` is the name of the StatefulSet you want to scale, which should be one of the StatefulSet names listed above.
- `{count}` is the expected number of replicas to be scaled.

For example, suppose that you want to scale up the MongoDB server associated with the release `my-release` from 2 to 3 replicas, the command will be like following.

```sh
kubectl scale statefulset/my-release-ib-336f-server --replicas=3
```

Additional persistent volumes are required for scaling up StatefulSets. One persistent volume is consumed per replica.
{: note}

## Identifying which nodes the product is deployed to
{: #identifying-nodes}

To identify the nodes to which the release is deployed, run the following `kubectl` command.

```sh
kubectl get pods -l release={release_name} -o wide
```

- `{release name}` is the Helm release name of your installation.

The `NODE` column of the command output shows the node to which each pod is deployed. Use the [Deployments](#deployments) and [StatefulSets](#statefulsets) tables to determine the pod names of the components.

You can use the following command to check the status of each node.

```sh
kubectl get nodes -l node-role.kubernetes.io/worker=true
```

## Viewing logs from the {{site.data.keyword.icp4dfull_notm}} Logging dashboard
{: #viewing-logs}

1.  Make sure an ELK stack is deployed to your cluster. See [IBM Cloud Private Logging](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.2/manage_metrics/logging_elk.html) for more details.
1.  Login to the ICP console of your cluster by accessing `https://{cluster_CA_domain}:8443` using your Web browser.

    - `{cluster_CA_domain}`: your cluster CA domain name. e.g., `mycluster.icp`.
1.  Open Kibana by accessing *Side Menu -> Platform -> Logging*
1.  Viewing and querying logs. See [Viewing and querying logs](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.2/manage_metrics/logging_elk.html#viewing-and-querying-logs) for more general information.

    - To see all logs of your installation, query logs by `kubernetes.pod:"{release_name}-*`. `{release name}` is the Helm release name of your installation. To identify which component output a log, check `kubernetes.pod` field of the log to see the pod name, and look up the component on the table in [Deployments](#deployments) and [StatefulSets](#statefulsets) sections with the pod name.

    - To see the logs of specific Deployment, query logs with `kubernetes.pod:"{deployment_name}-*"`. `{deployment_name}` is the Kubernetes Deployment name of the component you want to see logs. See [Deployments](#deployments) for the Deployment name of each component.

    - To see the logs of specific StatefulSet, query logs with `kubernetes.pod:"{statefulset_name}-*"`. `{statefulset_name}` is the Kubernetes StatefulSet name of the component you want to see logs. See [StatefulSets](#statefulsets) for the StatefulSet names of each component.
