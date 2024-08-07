---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-04"

subcollection: watson-knowledge-studio-data

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

# Creating the rule-based model
{: #wks_rule_train}

After defining rules, you can create a rule-based model.
{: shortdesc}

## Procedure
{: #wks_rule_train_procedure}

To create a rule-based model. complete the following steps:

1. Log in as a {{site.data.keyword.knowledgestudioshort}} administrator or project manager, and select your workspace.
2. Select the **Rule-based Model** > **Versions** > **Rule-based Model** tab.
3. Click **Map entity types and classes**.
4. Map the entity types from your type system to one or more classes that you used to define rules.

  After the entity types are mapped, you can [deploy the rule-based model](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-wks_rule_publish) or use it to [pre-annotate documents](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-preannotation#wks_preannotrule) in the process of creating a machine learning model.

## Deleting a rule-based model
{: #wks_rule_delete_model}

You cannot delete a rule-based model.

You can delete the workspace that was used to develop the rule-based model, but you cannot delete the model itself. Deleting a model is not the best approach. Instead, recreate the rules that are defined for it. Editing the rules directly changes the behavior of the rule-based model.
