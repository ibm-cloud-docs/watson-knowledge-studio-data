---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-20"

subcollection: watson-knowledge-studio

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:beta: .beta}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# {{site.data.keyword.knowledgestudioshort}} model build and deployment paths
{: #model-paths}

In {{site.data.keyword.knowledgestudiofull}}, there are three types of models that can be produced and deployed: machine Learning, rule-based, and advanced rules.
{: shortdesc}

The table below shows supported model deployment paths **From** {{site.data.keyword.knowledgestudioshort}} source **To** target services.

| From: {{site.data.keyword.icp4dfull_notm}} - {{site.data.keyword.knowledgestudioshort}} | To: {{site.data.keyword.icp4dfull_notm}} - {{site.data.keyword.discoveryfull}} | To: Public Cloud - {{site.data.keyword.discoveryfull}} | To: Public Cloud - {{site.data.keyword.nlufull}} |
|------------|-------|-----------------|-----------------|
| Machine learning | Upload via {{site.data.keyword.discoveryshort}} UI | N/A | N/A |
| Rule-based | Upload via {{site.data.keyword.discoveryshort}} UI | N/A | N/A |
| Advanced rules | Upload via {{site.data.keyword.discoveryshort}} UI | N/A | Deploy via {{site.data.keyword.knowledgestudioshort}} <sup>1</sup> |
{: caption="Table 1. Model deployment paths" caption-side="top"}

<sup>1</sup> See [Deploy advanced rules model to Natural Language Understanding](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-managing-projects-and-extractors#deploy-adv-rule-model-to-nlu-cloud)
