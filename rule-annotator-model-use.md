---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-19"

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

# Using the rule-based model
{: #wks_rule_publish}

Leverage a rule-based model that you created with {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} by making it available to other {{site.data.keyword.watson}} applications.
{: shortdesc}

**Attention**: You can deploy a rule-based model to make it available for use in these services as an [experimental](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-troubleshooting#experimental) feature.

You can also pre-annotate new documents with the rule-based model. See [Pre-annotating documents with the rule-based model](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_preannotrule) for details.

## Deploying a rule-based model to IBM Watson Discovery
{: #wks_rule_discovery}

Deploy the model to enable an application that uses the {{site.data.keyword.discoveryshort}} service to use the rule-based model to find and extract entities during document enrichment.

**Attention**: This is currently an [experimental](/docs/services/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-troubleshooting#experimental) feature of the service.

### Before you begin
{: #wks_rule_discovery_prereqs}

You must have administrative access to a {{site.data.keyword.watson}} {{site.data.keyword.discoveryshort}} for {{site.data.keyword.icp4dfull_notm}} deployment.

### Procedure
{: #wks_rule_discovery_procedure}

To deploy a rule-based model to {{site.data.keyword.watson}} {{site.data.keyword.discoveryshort}} for {{site.data.keyword.icp4dfull_notm}}, complete the following steps:

1. Log in as a {{site.data.keyword.knowledgestudioshort}} administrator or project manager, and select your workspace.
2. Select the **Rule-based Model** > **Versions** > **Rule-based Model** tab.
3. Choose the version of the model that you want to deploy, or click **Export current model**.

    If there is only one working version of the model, save the current model for deployment by clicking **Save for Deployment**. This versions the model, which enables you to deploy one version, while you continue to improve the current version. Saving the version might take a few minutes. The option to deploy does not appear until after the version is created.

4. Click **Export**, and click **Export** again to confirm.

    The model is saved as a PEAR file, and you are prompted to download the file. A PEAR (Processing Engine ARchive) file is the UIMA standard packaging format for UIMA components. The model is saved in PEAR format so it can be distributed and reused within UIMA applications.

### What to do next
{: #wks_rule_discovery_next}

To use the deployed model, you must provide the model ID when it is requested during the {{site.data.keyword.discoveryshort}} service enrichment configuration process. For more details, see the [{{site.data.keyword.discoveryshort}} service documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/discovery-data?topic=discovery-data-machinelearning-enrichment#machinelearning-enrichment){: new_window}.

## Leveraging a rule-based model in IBM Watson Explorer
{: #wks_rule_export}

Export the PEAR file that is produced when the rule-based model is created so it can be used in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.

### Procedure
{: #wks_rule_export_procedure}

To leverage a rule-based model in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer, complete the following steps.

1. Log in as a {{site.data.keyword.knowledgestudioshort}} administrator or project manager, and select your workspace.
2. Select the **Rule-based Model** > **Versions** > **Rule-based Model** tab.
3. Click **Export current model**.

    The model is saved as a PEAR file, and you are prompted to download the file. A PEAR (Processing Engine ARchive) file is the UIMA standard packaging format for UIMA components. The model is saved in PEAR format so it can be distributed and reused within UIMA applications.

4. Download the file to your local system.
5. From the {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer application, import the PEAR file.
