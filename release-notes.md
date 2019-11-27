---

copyright:
  years: 2019
lastupdated: "2019-11-27"

subcollection: watson-knowledge-studio-data

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}


# Release notes
{: #release-notes}

The following new features and changes to {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} are available.
{: shortdesc}

## Version 1.1.0 (27 November 2019)
{: #27-november-2019}

- {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} 1.1.0 can be installed on {{site.data.keyword.icp4dfull_notm}} 2.5

- **Dictionary suggestions**:
Create dictionaries faster. With only a few terms specified, {{site.data.keyword.knowledgestudioshort}} will analyze your uploaded documents and suggest other terms possibly in the same domain. These dictionaries can be used as machine learning preannotators or within rules to create models.

- **Advanced rules workspace (Beta)**:
Create rules-based text extractors with an advanced rules workspace. Advanced rules expand well beyond the standard rules engine with a graphical interface that creates Annotation Query Language (AQL) rules, allowing users to build and manage complex rule sets.

## Version 1.0.1 (30 August 2019)
{: #30-august-2019}

- Added the ability to create multiple instances per installation.
- Added the ability to install on {{site.data.keyword.openshiftlong_notm}}.
- Federal Information Security Management Act (FISMA) support is available for {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} offerings purchased on or after August 30, 2019. FISMA support is also available to those who purchased the June 28, 2019 version and upgrade to the August 30, 2019 version. {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} is FISMA High Ready.

**Annotation tasks are no longer required to create ground truth.**

- Admins and Project Managers can annotate document sets directly from the **Ground Truth** tab on the **Annotations** page.
- Annotation tasks are still available. You can manage them from the **Annotation Tasks** tab on the **Annotations** page.
- Annotations applied directly to ground truth are not applied to related active annotation tasks. It is recommended that you annotate document sets directly only when they are not associated with any active annotation tasks.
- The **Annotation Sets** tab has been removed from the **Documents** page. You can manage annotation sets from the **Annotation Tasks** tab of the **Annotations** page.
  - To create new annotation sets, click **Add task**. Then, click **Create Annotation Sets**.
  - To manage existing annotation sets, click an existing annotation task, then click **Edit**.


## Version 1.0.0 (30 June 2019)
{: #28-june-2019}

{{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} is now available as an add-on to the {{site.data.keyword.icpfull_notm}} platform.

