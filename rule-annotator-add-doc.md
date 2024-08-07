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

# Adding documents for defining rules
{: #wks_rule_anno_add}

Add documents with linguistic patterns that illustrate the types of rules you want to define.
{: shortdesc}

Documents that you add for defining rules are kept separate from documents that you add for annotation. Their sole purpose is to be used for mining examples of patterns. They are not used for training nor are they ever downloaded.

## Ways you can add documents to the rule editor

- **Author a text document in UTF-8 format**

    You can add text that illustrates a pattern directly into the rule editor. The name that you specify for the document cannot be longer than 256 characters.

- **Upload a CSV file**

    Upload a file in CSV format that contains the document text.

- **Copy a document that was imported for annotation**

    If a document that is part of a set that was imported for annotation purposes contains examples of patterns that you want to capture as rules, you can copy the document over to the rule editor. Any changes that you make to the document do not impact the original version of the document that is being used for annotation. For more information about uploading documents, see [Adding documents to a workspace](/docs/watson-knowledge-studio-data?topic=watson-knowledge-studio-data-documents-for-annotation#wks_projadd).

For documents that you copy or upload, choose documents that contain distinct patterns that help to identify entity types in documents.
