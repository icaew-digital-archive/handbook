# Ingest Workflow

> **Purpose:** This document outlines the ingest workflow for smaller ingests (not for bulk uploads utilising the API). This workflow happens after the appraisal workflow.

## Tools Used

This workflow utilises the following tools:

- Preservica New Gen UI - Drag and drop tool
- `a_get_metadata.py`
- `semaphore-helper.py`
- `b_delete_metadata.py`
- `c_add_metadata_from_csv.py`
- `d_update_xip_from_csv.py`

## Step 1: Create an ingest bucket in Preservica

**Location:** `Root/ADMIN/Working Area/Ingest`

**Bucket requirements:**
- Name folder in following style: `yyyymmdd-ingest-name-bucket`
- Hierarchy of ingest should be decided during appraisal and represented within the bucket

> **Important:** Take note of the asset ID for the bucket. This is the Preservica reference number that we use for our custom tools. It will be necessary for later steps in the workflow.

## Step 2: Upload resources to Preservica

Using the drag and drop feature, populate the bucket in accordance with the hierarchy decided on during appraisal.

## Step 3: Add metadata

> **Purpose:** The following workflow explains the process of creating a CSV that will then populate each uploaded resource with relevant metadata. Our style guide for metadata can be located [here](../preservica/dublin-core.md).

## 3a1: a_get_metadata.py

**Command:**
```bash
python3 a_get_metadata.py [preservica reference number] [output.csv]
```

> **Purpose:** This first step creates a custom CSV in accordance with the ICAEW Digital Archive metadata style guide. This CSV will be used to add metadata to ingested resources.

> **Note:** The CSV should have the following naming convention, remaining consistent with the bucket naming convention: `yyyymmdd-ingest-name-metadata.csv`.

## 3a2: semaphore-helper.py

**Command:**
```bash
python3 semaphore-helper.py [preservica ref] [asset download location] [output location > .csv]
```

> **Purpose:** The semaphore-helper script is a multi-part tool that:
> - Downloads a copy of each required asset from Preservica
> - Uses the ICAEW taxonomy to classify up to ten subject terms for the Dublin Core subject fields

**Requirements:**
- Preservica reference number
- Area for each asset to be downloaded to
- Output preferably CSV

> **Note:** The CSV should have the following naming convention, remaining consistent with the bucket naming and metadata naming convention: `yyyymmdd-ingest-name-classification.csv`. These subject terms will be added to the custom CSV.

## 3b: b_delete_metadata.py

**Command:**
```bash
python3 b_delete_metadata.py [preservica_ref]
```

> **Purpose:** This script is run to ensure there is no doubling of metadata within Preservica.

> **Warning:** The `b_delete_metadata.py` permanently deletes metadata templates connected to the asset. It uses a Preservica reference number to locate which assets' metadata it will delete. Using a folder's Preservica reference number will delete each of its child files' metadata templates.
> 
> **Each time this script is used, a Y/N prompt occurs. After Y, the metadata is permanently deleted.**

## 3c: c_add_metadata_from_csv.py

**Command:**
```bash
python3 c_add_metadata_from_csv.py [csv_location]
```

> **Purpose:** The `c_add_metadata_from_csv.py` tool adds the newly complete Dublin Core metadata CSV to the original assets within Preservica.

> **Note:** This tool uses the Preservica reference numbers in the CSV to locate where to add the newly completed Dublin Core metadata template.

## 3d: d_update_xip_from_csv.py

**Command:**
```bash
python3 d_update_xip_from_csv.py [csv_location]
```

> **Purpose:** The `d_update_xip_from_csv.py` tool adds the newly complete XIP metadata CSV to the original assets within Preservica. XIP metadata includes the `entity.title` (file name) and `entity.description`. The asset security tag can also be changed using this tool, but it is recommended to do that within the Preservica GUI.

> **Note:** This tool uses the Preservica reference numbers in the CSV to locate where to add the newly completed XIP metadata template.

## Suggested Checklist

- [ ] Confirm folder hierarchy matches appraisal output  
- [ ] Ensure all relevant metadata fields are filled in CSV  
- [ ] Review fields for misspelling, or misalignment (i.e., fields filled out wrong)  