# Ingest Workflow

The following document outlines the ingest workflow. 

This workflow is for smaller ingests and not for bulk uploads utilising the API.
This workflow happens after the appraisal workflow.

## This workflow utilises the following tools:

* Preservica New Gen UI - Drag and drop tool
* a_get_metadata.py
* semaphore-helper.py
* b_delete_metadata.py
* c_add_metadata_from_csv.py
* d_update_xip_from_csv.py

## Step 1: Create an ingest bucket in Preservica.

Location of bucket should be Root/ADMIN/Working Area/Ingest

Bucket should include: 
* name of folder in following style. yyyymmdd-ingest-name-bucket
* hierarchy of ingest should be decided during appraisal and represented within the bucket.

Take note of the asset ID for the bucket, this is the preservica reference number that we use for our custom tools. It will be necessary for later steps in the workflow.

## Step 2: Upload resources to Preservica

Using the drag and drop feature populate the bucket in accordance with the hierarchy decided on during appraisal.

## Step 3: Add metadata 

The following workflow will explain the process of creating a csv that will then populate each uploaded resource with relevant metadata. Our style guide for metadata can be located [here]

## 3a1: a_get_metadata.py

Run the following script:

```python3 a_get_metadata.py [preservica reference number] [output.csv]```

This first step will create a custom csv in accordance with the ICAEW Digital Archive metadata style guide. This csv will be used to add metadata to ingested resources.

The csv should have the following naming conventions, remaining consistent with the bucket naming convention: yyyymmdd-ingest-name-metadata.csv.

# 3a2: semaphore-helper.py

Run the following script:

```python3 semaphore-helper.py [preservica ref] [asset download location] [output location > .csv]```

The semaphore-helper script is a multi-part tool that firstly downloads a copy of each required asset from Preservica and then using the ICAEW taxonomy classifies up to ten subject terms for the Dublin Core subject fields.

Semaphore requires a preservica reference number, an area for each asset to be downloaded to and a output preferably CSV.

The csv should have the following naming conventions, remaining consistent with the bucket naming and metadata naming convention: yyyymmdd-ingest-name-classification.csv.

These subject terms will be added to the custom csv.

## 3b: b_delete_metadata.py

Run the following script:

```python3 b_delete_metadata.py [preservica_ref]```

This script is run to ensure there is no doubling of metadata within Preservica.

The b_delete_metadata.py permanently deletes metadata templates connected to the asset. It uses a preservica reference number to locate which assets metadata it will delete. Using a folders preservica reference number will delete each of its child files metadata templates.

WARNING: Each times this script is used, a Y/N prompt occurs. After Y the metadata is permanently deleted.

# 3c: c_add_metadata_from_csv.py

Run the following script:

```python3 c_add_metadata_from_csv.py [csv_location]```

the c_add_metadata.py tool adds the newly complete Dublin Core metadata CSV to the original assets within Preservica.

This tool uses the preservica reference numbers in the CSV to locate where to add the newly completed Dublin Core metadata template.

# 3d: d_update_xip_from_csv.py

Run the following script:

```python3 d_update_xip_from_csv.py [csv_location]```

The d_update_xip_from_csv.py tool adds the newly complete xip metadata CSV to the original assets within Preservica. XIP metadata are the entity.title (file name) and entity.description. The asset security tag can also be changed using this tool. But it is recommended to do that within the Preservica GUI.

This tool uses the preservica reference numbers in the CSV to locate where to add the newly completed Dublin Core metadata template.

# Suggested Checklist:

◻ Confirm folder hierarchy matches appraisal output
◻ Ensure all relevant metadata fields are filled in CSV
◻ Review fields for misspelling, or misalignment. i.e fields filled out wrong