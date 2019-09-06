---

copyright:
  years: 2019
lastupdated: "2019-06-28"

subcollection: watson-knowledge-studio-data

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:note: .note}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}


# Installing the add-on
{: #install}

To install the {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} add-on, see the [installation documentation](https://docs-icpdata.mybluemix.net/docs/content/SSQNUZ_current/com.ibm.icpdata.doc/watson/knowledge-studio-install.html) in the {{site.data.keyword.icp4dfull_notm}} documentation.

## Upgrading from v1.0.0 to v1.0.1
{: #upgrading}

To upgrade the {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} add-on from version 1.0.0 to version 1.0.1 in the same cluster, complete the following steps.

Only [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) is supported when upgrading. Note that [Backing up and restoring databases](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases) is not supported.
{: note}

1. Backup the workspaces data. See [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) for more details.

1. Deprovision an existing {{site.data.keyword.knowledgestuioshort}} instance on IBM Cloud Pak for Data console.

1. Uninstall the existing version 1.0.0 installation. See `Uninstalling the chart` section of [Installing the Watson Knowledge Studio add-on](https://docs-icpdata.mybluemix.net/docs/content/SSQNUZ_current/com.ibm.icpdata.doc/watson/knowledge-studio-install.html) for details.

1. Install version 1.0.1. See [Installing the Watson Knowledge Studio add-on](https://docs-icpdata.mybluemix.net/docs/content/SSQNUZ_current/com.ibm.icpdata.doc/watson/knowledge-studio-install.html) for details.

1. Provision a new {{site.data.keyword.knowledgestuioshort}} instance in the {{site.data.keyword.icp4dfull_notm}} console.

1. Restore the workspaces data into the new instance. See [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) for details.
