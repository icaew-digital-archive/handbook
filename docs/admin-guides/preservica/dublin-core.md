# Metadata Style Guide

## General

This page covers how ICAEW utilises its custom schema in Preservica.

## Asset XIP

| Custom Schema Terms | Description - ICAEW Specific Use | Examples |
| :---                | ---- | -------- |
| entity.title |  Format: yyyymmdd-File-Name. File name is in Title Case. Replace spaces with hyphens. No special characters (such as &). Acronyms should be all capitals.  The yyyymmdd is to be taken from within the document itself and not the document properties. Use 00 to indicate abesence of days/months/years. A file name can begin with 00000000. Include the document title and the faculty contributor, where applicable. Include unique identifier. Keep concise, the ICAEW digital archive is working towards making file names obsolete for Universal Access.|  - 20120200-Economia-Issue-1, UI is include here, day not found so use 00. <br> - 20101000-International-Consistency, no unique identifier, use title of document in Title case. <br> - 20020117-TECH-02-02. <br>
| entity.description | This field is not used at the asset level. |


## Folder XIP


| Custom Schema Terms | Description - ICAEW Specific Use |
| :---                | ---- |
| entity.title |  Seprarate rules are used at folder level. Uses user friendly names. Folders are created with Preservica and thus creation dates don't make sense. Prioritise access for folder level entity.title. |  
| entity.description | At folder level this is used for internal note taking purposes. For folders used exclusively for deletion, this field explains reason for use and why it is to be deleted. For collections use this field to include information on who requested the ingest and who didthe ingest. This field is to be used to describe the providence of the work.|

## Asset Dublin Core

General usage of Dublin Core can be found [here](https://www.dublincore.org/specifications/dublin-core/usageguide/2001-04-12/generic/).

| DC Terms    | Description - General Use | Description - ICAEW Specific Use | Examples |
| :---        |   ----   | :---- | ---- |
| Title | The name given to the resource. | Match the title as found within the document. Use sentence case, title and subtile divided by a colon (:). After colon do not restart sentence case. Do not use (&) use and. Use question marks. Do not finish with full stops. Use Unique Identifiers (UI), when available. For example an Issue number or date, use commas to break titles and date/issues. Add important information such as if it is revised or how long it is relevant for. This is helpful for removing duplicate titles and for assisting in access.| Examples: - Economia, Issue 1<br> - International consistency: global challenges initiative, providing direction <br> - Preface to international reporting standards, TECH 02/02 |
| Creator | An entity primarily responsible for making the content. | The creator is the author as credited in the document itself, not in its properties. Multiple creators are allowed. Preference for Creator is author FirstName LastName, internal faculty and then institution (most often ICAEW).| - John Doe <br> - Financial Services Faculty <br> - Deloitte <br> - ICAEW <br> 
| Subject | The topic of the content of the resource. | Use multiple dc:subject fields. Subjects are selected from ICAEW’s internal classification system (Semaphore). Each document should be assigned 10 relevant subjects. [Link to Semaphore classification workflow]  |
| Description | An account of the content of the resource. | Writing descriptions is currently resource-intensive. Until scalable AI-assisted solutions are implemented, only provide descriptions when they already exist (e.g., for documents found on the ICAEW website or resources previously described in the ICAEW Library catalogue). A description should briefly summarize the content of the resource. |
| Publisher | The entity responsible for making the resource available. | Preference for external publishers, otherwise use ICAEW. Check ICAEW Library catalogue for support. | - Progressive Content <br> - ICAEW <br> |
| Contributor | An entity responsible for making contributions to the resource.| This field is for external institutes. Use the external institutes full title.| - Deloitte <br> - The Pensions Regulator <br>
| Date | A date associated with an event in the life cycle of the resource. | Use the format: yyyy-mm-dd. If the full date is unknown, month and year (YYYY-MM) or just year (YYYY) may be used. | - 2002-01-17 <br> - 2012-02 <br> - 2022 <br> |
| Type | The nature or genre of the content of the resource. | ICAEW uses two type fields. This first type field contains DC recommended [DCMI Type Vocabulary](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#section-7) such as Text or Moving image etc. Use sentance case. | - Text <br> - Moving Image <br> - Audio <br> |
| Format | The physical or digital manifestation of the resource. | Use the file's extension to indicate its format, using lower case without proceeding ".". Examples: doc, xlsx, csv, pdf, mp4, tiff | - doc <br> - xlsx <br> - csv <br> - pdf <br> - mp4 <br>
| Identifier | An unambiguous reference to the resource within a given context. | Examples of used Identifiers for this field: ISBN, URLs, Issue numbers, UIs. Use all identifiers in one field. | -https://www.icaew.com/technical/tax/tax-faculty/taxline <br> - 9781526516459 <br> - TECH 12/10 AAF <br> |
| Source | A related resource from which the described resource is derived. | Primarily used if an item has been digitised, and has an original source since it is not a born-digital item.| Digitised |
| Language | A language of the intellectual content of the resource. | Will normally be 'en' for English ICAEW publications. | - en <br> - ar <br> - cn <br>
| Relation | A reference to a related resource. | Populate with the name of the parent folder where the document resides. Helps track document relationships within the archive hierarchy. Documents that have been linked into a separate parent folder/collection, would have two Relation fields.| - Economia <br> - Audit Monitoring Report <br> - Faculty Serials <br> |
| Coverage | The spatial or temporal topic of the resource, spatial applicability of the resource, or jurisdiction under which the resource is relevant. | ICAEW does not currently use this field, maybe in future. |
| Rights | Information about rights held in and over the resource. | ICAEW does not currently use this field, maybe in future. Rights information can be found within documents. |

## Folder Dublin Core

Folders do not use the complete suite of Dublin Core metadata fields.

| DC Terms    | Description - General Use | Description - ICAEW Specific Use | Example |
| :---        |   ----   | :---- | ---- |
| Title | The name given to the resource. | Use title case, no special punctuation, use access friendly titles and use commas to break between title and dates. | - Economia <br> - Audit Monitoring Report <br> - Faculty Serials <br> |
| Description | An account of the content of the resource. | This field for folders is used to provide a general description of the collection within the folder, including a date range and relevant title history. Do not use special punctuation.| - Economia is the journal of ICAEW (previously ICAEW updates were published in Accountancy). Economia ran from 2012 to 2019. Quarterly replaced Economia. |
| Date | A date associated with an event in the life cycle of the resource. | Date ranges should be included in the parent folder, using YYYY-MM-DD. If the full date is unknown, month and year (YYYY-MM) or just year (YYYY) may be used. Date ranges may be specified using ISO 8601 period of time specification in which start and end dates are separated by a '/' (slash) character. Either the start or end date may be missing. | - 2012 - 2019 |