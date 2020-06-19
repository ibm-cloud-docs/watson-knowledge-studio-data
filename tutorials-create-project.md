---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-19"

subcollection: watson-knowledge-studio-data

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:hide-dashboard: .hide-dashboard}

# Getting started with {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}}
{: #wks_tutintro}

This {{site.data.keyword.knowledgestudiofull}} for {{site.data.keyword.icp4dfull_notm}} tutorial helps you perform prerequisite tasks that must be completed before you can start any of the other tutorials.
{: shortdesc}

## Before you begin
{: #prereq}

- Provision the instance of the {{site.data.keyword.knowledgestudioshort}} for {{site.data.keyword.icp4dfull_notm}} add-on. For more information about provisioning, see [Installing the add-on](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-install).
- Confirm you're using a supported browser. For information, see [Browser requirements](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-system-requirements).

## Launching the {{site.data.keyword.knowledgestudioshort}} application
{: #launching-the-knowledge-studio-application}

1.  From the {{site.data.keyword.icp4dfull_notm}} web client menu, choose **My Instances**.
2.  Click the {{site.data.keyword.knowledgestudioshort}} instance to open the overview page. Click **Launch Tool**. 

    If you're prompted to log in, provide your {{site.data.keyword.icp4dfull_notm}} credentials.

## Lesson 1: Assigning user roles
{: #wks_tutless1}

In this lesson, you will learn about the different roles that you can assign to users in {{site.data.keyword.knowledgestudioshort}}.

### About this task
{: #wks_tutless1_about}

The creation of a machine learning model requires input from subject matter experts, project managers, and users who can understand and interpret statistical models. Administrators assign roles to each user, such that they have appropriate authority for their tasks. For more information about user roles, see [Assembling a team](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-team).

### Procedure
{: #wks_tutless1_procedure}

1. Log in to {{site.data.keyword.knowledgestudioshort}} with an administrator ID.
2. Click the **Settings** icon to open the Service Details page. The page lists all the user IDs that are registered as {{site.data.keyword.knowledgestudioshort}} users. Each user ID has one of the following roles (in decreasing order of included permissions):

    - Admin
    - Project Manager
    - Human Annotator

    For information about user roles, see [User roles in {{site.data.keyword.knowledgestudioshort}}](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-roles).

3. Verify that there is at least one user with the Admin role. A user ID with this role can create workspaces, and act as a project manager or human annotator.
4. If you have access to additional user IDs, verify that there are at least two users with the Human Annotator role.

    Creating a real-life model typically involves multiple human annotators in addition to an administrator or project manager. However, for purposes of the tutorial, you can continue with a single user ID.
    {: tip}

5. Optional: Change the role that is assigned to a user ID. From the **Action** column of the table, click the **Edit** link, an then change the assigned user role.

    You can upgrade a user ID to a role with greater permissions, but you cannot downgrade a user with an Admin or Project Manager role to the Human Annotator role.
    {: note}

## Lesson 2: Creating a workspace
{: #wks_tutless2}

In this lesson, you will learn how to create a workspace within {{site.data.keyword.knowledgestudioshort}}.

### About this task
{: #wks_tutless2_about}

A workspace defines all the resources that are required to create a machine learning model, including training documents, the type system, dictionaries, and annotations that are added by human annotators. For more information about workspace creation, see [Creating a workspace](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-create-project).

### Procedure
{: #wks_tutless2_procedure}

1. {: hide-dashboard} As a {{site.data.keyword.knowledgestudioshort}} administrator, from your {{site.data.keyword.cloud_notm}} [resources ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/resources){:new_window} page, click the {{site.data.keyword.knowledgestudioshort}} service instance under **Services**.
2. Click **Launch tool** from the Manage page.
3. Click **Create Workspace**.
4. Specify the details for the new workspace:

    - In the **Workspace name** field, type `My workspace`.
    - In the **Workspace description** field, type `Watson Knowledge Studio tutorial workspace`.
    - In the **Language of documents** field, use the default value, **English**. The sample files you will be using for this tutorial are in English.
5. Click **Create**.

### Results
{: #wks_tutless2_results}

The workspace is created and opens automatically.

### What to do next
{: #wks_tutless2_next}

You can now start configuring the workspace resources, such as the type system.

## Lesson 3: Creating a type system
{: #wks_tutless3}

In this lesson, you will learn how to upload and modify a type system within {{site.data.keyword.knowledgestudioshort}}. You must create or upload a type system before you begin any annotation tasks.

### About this task
{: #wks_tutless3_about}

For more information about type systems, see [Type systems](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-typesystem#wks_typesystem).

### Procedure
{: #wks_tutless3_procedure}

1. Download the <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/knowledge-studio/en-klue2-types.json" download="en-klue2-types.json">en-klue2-types.json <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a> file to your computer. This file contains an example KLUE type system.
1. Click **Assets**> **Entity Types**.
1. On the Entity Types page, click **Upload**.
1. Upload the `en-klue2-types.json` file from your computer. The uploaded type system is displayed in the table.
1. Browse the type system so you can see the data that was uploaded.
1. Edit an entity type:

    1. Locate the MONEY entity type.
    1. Double-click anywhere in the table row to edit the entity type.
    1. In the **Roles** column, click the **Delete a role** icon ![The "Delete a role" icon.](images/wks_delete.png "The "Delete a role" icon.") next to the AWARD role.

        ![A screen capture of deleting a role from an entity type.](images/wks_tut_delete_role.png "A screen capture that shows the cursor hovering over the "Delete a role" icon.")

    1. Click **Save**.

### What to do next
{: #wks_tutless3_next}

After you finish making changes to the type system, you can begin adding documents to your workspace.

## Lesson 4: Adding a dictionary
{: #wks_tutless4}

In this lesson, you will learn how to add a dictionary to a workspace in {{site.data.keyword.knowledgestudioshort}}. Dictionaries are used for pre-annotating text when creating a machine learning model.

### About this task
{: #wks_tutless4_about}

For more information about dictionaries, see [Adding dictionaries to a workspace](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-dictionaries#wks_projdictionaries).

### Procedure
{: #wks_tutless4_procedure}

1. Download the <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/knowledge-studio/dictionary-items-organization.csv" download="dictionary-items-organization.csv">dictionary-items-organization.csv <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon"></a> file to your computer. This file contains dictionary terms in CSV format, suitable for uploading into a {{site.data.keyword.knowledgestudioshort}} dictionary.
1. Click **Assets** > **Dictionaries**.
1. Click **Create Dictionary** to add a dictionary.

    Do not click **Upload dictionary**, which is used to upload a dictionary that you want to use as-is. For the tutorial, you will create a new editable dictionary and then upload terms into it.
    {: important}

1. In the **Name** field, type `Test dictionary` and click **Save** to create the (empty) dictionary.

    The new dictionary is created and automatically opened for editing.

1. In the dictionary pane, click **Upload**.
1. Upload the `dictionary-items-organization.csv` file from your computer. The terms in the file are uploaded into the dictionary.
1. Click **Add Entry** to create a new term. An editable row is added at the top of the table.
1. In the **Surface Forms** column, type `IBM` and `International Business Machines Corporation` on separate lines. (When you begin to type a new surface form, a space is added below for an additional surface form.) Leave the radio button next to `IBM` selected, which indicates that `IBM` is the lemma.
1. In the **Part of Speech** column, select **Noun**.
1. Click **Save**.

    ![A screen capture of the Dictionaries page. The "IBM" dictionary entry is selected and its surface forms and part of speech are highlighted.](images/wks_tutdict4.png "A screen capture of the Dictionaries page. The "IBM" dictionary entry is selected and its surface forms and part of speech are highlighted.")

### What to do next
{: #wks_tutless4_next}

After you create a dictionary, you can use it to speed up human annotation tasks by pre-annotating the documents.

## Next steps
{: #wks_tutless_next}

Learn how to [create a machine learning model](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-wks_tutml_intro).

## Tutorial summary
{: #wks_tutsum}

While learning about {{site.data.keyword.knowledgestudioshort}}, you created a workspace and added artifacts to it.

### Lessons learned
{: #tcp-ll}

By completing this tutorial, you learned about the following concepts:

- Workspaces
- Type systems
- Dictionaries
