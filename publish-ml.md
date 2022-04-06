---

copyright:
  years: 2019, 2022
lastupdated: "2022-04-06"

subcollection: watson-knowledge-studio-data

---

{{{site.data.keyword.attribute-definition-list}}

# Using the machine learning model
{: #publish-ml}

Leverage a machine learning model that you trained with {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} by making it available to other {{site.data.keyword.watson}} applications.
{: shortdesc}

You can deploy or export a machine learning model. A dictionary can only be used to pre-annotate documents within {{site.data.keyword.knowledgestudioshort}}.

You can also pre-annotate new documents with the machine learning model. See [Pre-annotating documents with the machine learning model](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_preannotsire) for details.

## Exporting a machine learning model
{: #exporting-a-machine-learning-model}

To export a machine learning model as a .zip file, complete the following steps:

1. Log in as a {{site.data.keyword.knowledgestudioshort}} administrator or project manager, and select your workspace.
2. Select **Machine Learning Model** > **Versions**.
3. Choose the version of the model that you want to export, or select **Export current model**.

    If there is only one working version of the model, create a snapshot of the current model. This versions the model, which enables you to deploy one version, while you continue to improve the current version. The option to deploy does not appear until you create at least one version.

4. Click **Export**, and then click **Export** again to confirm.

## Deploying a machine learning model to IBM Watson Discovery for IBM Cloud Pak for Data
{: #wks_madiscovery}

When you are satisfied with the performance of the model, you can export a version to {{site.data.keyword.discoveryfull}} for {{site.data.keyword.icp4dfull_notm}}. This feature enables your applications to use the deployed machine learning model to enrich the insights that you get from your data to include the recognition of entities and relations that are relevant to your domain.

### Before you begin
{: #wks_madiscovery_prereqs}

You must have administrative access to a [{{site.data.keyword.discoveryshort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/discovery-data) deployment.

### Procedure
{: #wks_madiscovery_procedure}

1. [Export a machine learning model](#exporting-a-machine-learning-model).
2. From the {{site.data.keyword.discoveryshort}} service, follow the steps to create a Machine Learning enrichment, which include uploading the ZIP file. For more details, see [Machine Learning models](/docs/discovery-data?topic=discovery-data-domain#machinelearning){: external} in the {{site.data.keyword.discoveryshort}} v2 documentation.

## Deploying a machine learning model to IBM Watson Natural Language Understanding for IBM Cloud Pak for Data
{: #wks_manlu}

When you are satisfied with the performance of the model, you can deploy a version of it to {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.nlushort}}. This feature enables your applications to use the deployed machine learning model to analyze custom entities and relations.

### Before you begin
{: #wks_manlu_prereqs}

You must have a [{{site.data.keyword.nlushort}} for {{site.data.keyword.icp4dfull_notm}}](/docs/natural-language-understanding-data) deployment.

### Procedure
{: #wks_manlu_procedure}

1. [Export a machine learning model](#exporting-a-machine-learning-model).
2. Follow the [Customizing](/docs/natural-language-understanding-data?topic=natural-language-understanding-data-customizing) instructions in the {{site.data.keyword.nlushort}} for {{site.data.keyword.icp4dfull_notm}} documentation to create an entities model with the .zip file that you downloaded.

## Deleting a version
{: #wks_delete_model_version}

If you wish to delete a specific version a same machine learning model, navigate to the **Versions** page and click the **Delete** link on the row of the version that you want to delete.
**Note:** The **Delete** model version link is only active if there are no deployed models associated with it. **Undeploy** all associated models before deleting the a version.

## Leveraging a machine learning model in IBM Watson Explorer
{: #wks_maexport}

Export the trained machine learning model so it can be used in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.

### Before you begin
{: #wks_maexport_prereqs}

If you choose to identify relation types and annotate them, then you must define at least two relation types, and annotate instances of the relationships in the ground truth before you export the model. Defining and annotating only one relation type can cause subsequent issues in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer, release 11.0.1.0.

### About this task
{: #wks_maexport_about}

Now that the machine learning model is trained to recognize entities and relationships for a specific domain, you can leverage it in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.

Click [this link ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=1VoS-xczBow&amp;feature=youtu.be){: new_window} to watch a less than 2 minute video that illustrates how to export a model and use it in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer.

### Procedure
{: #wks_maexport_procedure}

1. [Export a machine learning model](#exporting-a-machine-learning-model).
1. From the {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer application, import the model.

    You can then map the model to a machine learning model in {{site.data.keyword.watson}} Explorer Content Analytics. After you perform the mapping step, when you crawl documents, the model finds instances of the entities and relations that your model understands. To learn how to import and configure the model in {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} Explorer, see the technical document that describes the integration: [http://www.ibm.com/support/docview.wss?uid=swg27048147 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/support/docview.wss?uid=swg27048147){: new_window}.

#### Related tasks
{: #wks_maexport_related}

[Exporting analyzed documents from {{site.data.keyword.watson}} Explorer Content Analytics](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_uimawexca)
