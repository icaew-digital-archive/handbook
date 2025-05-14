# Metadata Style Guide

## General

This page covers how ICAEW utilises its custom schema in Preservica.

| Custom Schema Terms | Description - ICAEW Specific Use |
| :---                | ---- |
| entity.title |  Format: yyyymmdd-File-name. Begin the file name with a capital letter. Replace spaces with hyphens. The yyyymmdd should match the date found in the document's properties. Include the document title and the faculty contributor, where applicable. |  
| entity.description | This should be a copy of the dc:description field. At the folder level, include a brief history of the series, including any name changes. |

General usage of Dublin Core can be found [here](https://www.dublincore.org/specifications/dublin-core/usageguide/2001-04-12/generic/).

| DC Terms    | Description - General Use | Description - ICAEW Specific Use |
| :---        |   ----   | :---- |
| Title | The name given to the resource. | Match the file name. Use sentence case: Capitalize the first word only (unless a proper noun or acronym). Replace hyphens with colons. |
| Creator | An entity primarily responsible for making the content. | The creator is the author as credited in the document itself, not in its properties.Multiple creators are allowed. Preference should be given to individual creators over organizational ones. Provide full names. ICAEW may also be credited as a creator, where applicable. |
| Subject | The topic of the content of the resource. | Use multiple dc:subject fields. Subjects are selected from ICAEW’s internal classification system (Semaphore). Each document should be assigned 10 relevant subjects. [Link to Semaphore classification workflow]  |
| Description | An account of the content of the resource. | Writing descriptions is currently resource-intensive. Until scalable AI-assisted solutions are implemented, only provide descriptions when they already exist (e.g., for documents found on the ICAEW website). A description should briefly summarize the content of the resource. |
| Publisher | The entity responsible for making the resource available. | The publisher is the entity responsible for making the resource available. For most ICAEW documents, use ICAEW as the publisher. |
| Contributor | An entity responsible for making contributions to the resource.| Use for faculties or departments contributing to ICAEW publications. Examples: Tax Faculty, Information Technology Faculty, Professional Standards Department.|
| Date | A date associated with an event in the life cycle of the resource. | Use the format: yyyy-mm-dd. If the document contains a visible date, use it (even if it's partial, e.g., March 2020 becomes 2020-03-01). If no visible date exists, use the date found in the document’s properties. | 
| Type | The nature or genre of the content of the resource. | ICAEW uses two type fields. This first type field contains DC recommended [DCMI Type Vocabulary](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#section-7) such as text or movingimage etc. No capitals should be used. Note: A secondary field is currently used for ICAEW-specific content types (e.g., Annual Report, Helpsheets). This is expected to move to a custom metadata field in the future. |
| Format | The physical or digital manifestation of the resource. | Use the file's extension to indicate its format. Examples: .doc, .xlsx, .csv, .pdf, .mp4, .tiff |
| Identifier | An unambiguous reference to the resource within a given context. | ICAEW does not use this field |
| Source | A related resource from which the described resource is derived. | Used for resources retrieved from the ICAEW website. Add the URL as the value.|
| Language | A language of the intellectual content of the resource. | Will normally be 'en' for English ICAEW publications. |
| Relation | A reference to a related resource. | Populate with the name of the parent folder where the document resides. Helps track document relationships within the archive hierarchy. |
| Coverage | The spatial or temporal topic of the resource, spatial applicability of the resource, or jurisdiction under which the resource is relevant. | ICAEW does not use this field |
| Rights | Information about rights held in and over the resource. | Extract rights information from the bottom of the document, usually on the last page. Here is an example: Copyright © ICAEW November 2013. All rights reserved. If you want to reproduce or redistribute any of the material in this publication, you should first get ICAEW’s permission in writing. ICAEW will not be liable for any reliance you place on information in this publication. |
