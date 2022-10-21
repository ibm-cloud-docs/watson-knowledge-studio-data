---

copyright:
  years: 2019, 2022
lastupdated: "2022-10-21"

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

To install {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}}, see the [installation procedure](https://www.ibm.com/docs/SSQNUZ_4.5.x/svc-wks/knowledge-studio-install-overview.html){: external} in the {{site.data.keyword.icp4dfull_notm}} documentation.

## Upgrading to the latest version
{: #upgrading}

You can do in-place upgrades starting with the {{site.data.keyword.icp4dfull_notm}} 4.x releases. For more information, see [Upgrading](https://www.ibm.com/docs/SSQNUZ_4.5.x/svc-wks/knowledge-studio-upgrade.html){: external}.

For releases earlier than 4.0.x, you must backup data, uninstall, install the new version, and then restore the data.

To upgrade the {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} service in the same cluster, complete the following steps.

Only [Backing up and restoring data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) is supported when upgrading. Note that [Backing up and restoring databases](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore-databases) is not supported.
{: note}

1.  [Backup the workspaces data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore).
1.  Deprovision the existing {{site.data.keyword.knowledgestudioshort}} instance on the {{site.data.keyword.cpd_full_notm}} console.
1.  Uninstall the existing installation. 
1.  Install the latest version.
1.  Provision a new {{site.data.keyword.knowledgestudioshort}} instance in the {{site.data.keyword.icp4dfull_notm}} console.
1.  [Restore the workspaces data](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-backup-restore) into the new instance.
