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

To install the {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} add-on, see the [installation documentation](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-install.html) in the {{site.data.keyword.icp4dfull_notm}} documentation.

## Upgrading to the latest version
{: #upgrading}

To upgrade the {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} service in the same cluster, complete the following steps.

Only [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) is supported when upgrading. Note that [Backing up and restoring databases](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases) is not supported.
{: note}

1. Backup the workspaces data. See [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) for more details.

1. Deprovision the existing {{site.data.keyword.knowledgestuioshort}} instance on the IBM Cloud Pak for Data console.

1. Uninstall the existing installation. See `Uninstalling the chart` section of [Installing the Watson Knowledge Studio add-on](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-uninstall.html) for details.

1. Install the latest version. See [Installing Watson Knowledge Studio](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-install.html) for details.

1. Provision a new {{site.data.keyword.knowledgestudioshort}} instance in the {{site.data.keyword.icp4dfull_notm}} console.

1. Restore the workspaces data into the new instance. See [Backing up and restoring data](https://cloud.ibm.com/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) for details.
