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

# Installing {{site.data.keyword.knowledgestudioshort}}
{: #install}

To install the {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} add-on, see the [installation documentation](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-install.html){: external} in the {{site.data.keyword.icp4dfull_notm}} documentation.

## Upgrading to the latest version
{: #upgrading}

To upgrade the {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} service in the same cluster, complete the following steps.

Only [Backing up and restoring data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) is supported when upgrading. Note that [Backing up and restoring databases](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases) is not supported.
{: note}

1.  [Backup the workspaces data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore).
1.  Deprovision the existing {{site.data.keyword.knowledgestudioshort}} instance on the {{site.data.keyword.cpd_full_notm}} console.
1.  Uninstall the existing installation. For more information, see [Uninstalling {{site.data.keyword.knowledgestudioshort}}](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-uninstall.html){: external}.
1.  Install the latest version. For more information, see [Installing {{site.data.keyword.knowledgestudioshort}}](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/watson/knowledge-studio-install.html){: external}.
1.  Provision a new {{site.data.keyword.knowledgestudioshort}} instance in the {{site.data.keyword.icp4dfull_notm}} console.
1.  [Restore the workspaces data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) into the new instance.
