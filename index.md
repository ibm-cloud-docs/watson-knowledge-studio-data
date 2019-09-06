---

copyright:
  years: 2015, 2019
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


# About
{: #wks_overview_full}

Use {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} to create a machine learning model that understands the linguistic nuances, meaning, and relationships specific to your industry or to create a rule-based model that finds entities in documents based on rules that you define.
{: shortdesc}

## Identify custom entities and relations
{: #entities-and-relations}

An entities and relations workspace enables you to create your own entity type system and train a custom model that can recognize custom entities in text. With machine learning models, you can also define custom relation types and train the model to recognize when two entities are related. Consider the following example sentence.

```
ABC Motors has received great reviews for its new 2020 Lightning.
```

A custom entities and relations model could be trained to recognize "2020 Lightning" as a `Vehicle` entity, and "ABC Motors" as a `Manufacturer` entity. The model could also be trained to recognize that the two entities are connected by an `isManufacturedBy` relation.


### Build a machine learning model
{: #ibml}

{{site.data.keyword.knowledgestudioshort}} provides easy-to-use tools for annotating unstructured domain literature, and uses those annotations to create a custom machine learning model that understands the language of the domain. The accuracy of the model improves through iterative testing, ultimately resulting in an algorithm that can learn from the patterns that it sees and recognize those patterns in large collections of new documents. You can deploy the finished machine learning model to other {{site.data.keyword.watson}} cloud-based offerings and cognitive solutions to find and extract mentions of relations and entities, including entity coreferences.

![Overview of the process to build a machine learning model](images/wks-ovw-anno.svg "Shows the process of building a machine learning model that can find entities and relations in new documents.") Figure 1. Overview of the process to build a machine learning model

1. Based on a set of domain-specific source documents, the team creates a type system that defines entity types and relation types for the information of interest to the application that will use the model.
2. A group of two or more human annotators annotates a small set of source documents to label words that represent entity types, to identify relation types where the text identifies relationships between entity mentions, and to define coreferences, which identify different mentions that refer to the same thing, that is, the same entity. Any inconsistencies in annotation are resolved, and one set of optimally annotated documents is built, which forms the ground truth.
3. {{site.data.keyword.knowledgestudioshort}} uses the ground truth to train a model.
4. The trained model is used to find entities, relations, and coreferences in new, never-seen-before documents.

See [Creating a machine learning model](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-ml_annotator) for more details.

### Build a rule-based model
{: #ibrb}

{{site.data.keyword.knowledgestudioshort}} provides a rules editor that simplifies the process of finding and capturing common patterns in your documents as rules. You can then create a model that recognizes the rule patterns, and deploy it for use in other services.

See [Creating a rule-based model](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-rule-annotator) for more details.

## FISMA support
{: #fisma-support}

Federal Information Security Management Act (FISMA) support is available for {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} offerings purchased on or after August 30, 2019. {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} is FISMA High Ready.

## Watson services integration
{: #wks_watsoninteg}

Share domain artifacts and models between {{site.data.keyword.knowledgestudioshort}} and other {{site.data.keyword.watson}} services available on {{site.data.keyword.icp4dfull_notm}}.

Use {{site.data.keyword.knowledgestudioshort}} to perform the following tasks:

- Upload analyzed documents that are in [UIMA CAS XMI format](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_uimaweximport). For example, you can upload UIMA CAS XMI files that were exported from {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer content analytics collections or [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer Content Analytics Studio](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_uimawexstudio).
- Export a [machine learning](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-publish-ml#wks_madiscovery) or [rule-based](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-wks_rule_publish#wks_rule_discovery) model to use with {{site.data.keyword.watson}} {{site.data.keyword.discoveryshort}} for {{site.data.keyword.icp4dfull_notm}}.
- Export a [machine learning](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-publish-ml#wks_manlu) to use with {{site.data.keyword.nlushort}} for {{site.data.keyword.icp4dfull_notm}}.
- [Export a machine learning model](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-publish-ml#wks_maexport) to use in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.
- [Export a rule-based model PEAR file](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-wks_rule_publish#wks_rule_export) to use in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.
