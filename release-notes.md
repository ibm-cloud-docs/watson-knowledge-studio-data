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

# Release notes
{: #release-notes}

The following new features and changes to {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull}} are available.
{: shortdesc}

For details about what's new, see the {{site.data.keyword.icp4dfull_notm}} documentation.

-  [4.5.x](https://www.ibm.com/docs/SSQNUZ_4.5.x/fixlist/wks-fixlist.html){: external}

## Version 1.2.0 (9 December 2020)
{: #v120}

- **Security and stability fixes**
    - Several security fixes and stability enhancements have been introduced.

## Version 1.1.3 (31 August 2020)
{: #v113}

- **Security and stability fixes**
    - Several security fixes and stability enhancements have been introduced.

## Version 1.1.2 (19 June 2020)
{: #v112}

- **Enhanced pre-annotation workflow.**
    - Run multiple pre-annotators at once and configure the order of pre-annotators to resolve annotation conflicts between them. Existing pre-annotator results are preserved unless you remove them with the *Wipe* option.
    - Human annotations are preserved when running pre-annotators, even when using the *Wipe* option.

## Version 1.1.1 (28 February 2020)
{: #28-february-2020}

- **New advanced rules tokenizer** Advanced rules has introduced a new tokenizer which is not compatible with the previous one. The following extractor categories are affected:
  - **Parts of Speech** extractor names are renamed automatically
    - **Cardinal** is renamed to **Numeral**
    - **Preposition** is renamed to **Adposition**
  - **Machine Data Analytics** Splitter extractors have been removed and will disappear from the canvas. There is no substitute for these extractors that is provided by the new tokenizer.
    - Generic Splitters:
      - **Date Time Splitter**
      - **Day First Splitter**
      - **Month First Splitter**
      - **Year First Splitter**
    - Syslog Splitter:
      - **Date Time Splitter**
- **Cross-sentence relations (Experimental)**: English entities and relations workspaces can now support annotating relations between entities within spans of six sentences. To get started, see [Enabling cross-sentence relations](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-enabling-cross-sentence-relations).

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

