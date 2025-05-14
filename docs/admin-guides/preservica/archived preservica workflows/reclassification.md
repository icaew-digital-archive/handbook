# Reclassification

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "Classification Rules" section]
[NOTE: Add a "Process Steps" section]
[NOTE: Add a "Validation" section]
[NOTE: Add a "Error Handling" section]
[NOTE: Add a "Best Practices" section]

# Reclassification Workflow

The following document outlines the reclassification workflow.

The reclassification workflow downloads assets from the ICAEW Preservica digital archive. Then using Python tools and the Semaphore taxonomy each asset is re-classified with Dublin Core metadata. Upon completing the Dublin Core metadata CSV, the existing metadata connected to the asset will be deleted. The new metadata will then be connected to the asset. Four tools are used in the workflow, the workflow has two stages.

Each of these tools require that the operator is within a virtual environment in the relevant directory.

```. ./venv/bin/activate```

## Download and Classify

* a_get_metadata.py
* semaphore-helper.py

During this stage, assets and existing metadata will be downloaded from Preservica. Using semaphore, each of these assets will be re-classified and any additional metadata will be added/updated.

## a_get_metadata.py

The first step is to download the existing metadata for the assets that are to be reclassified. First step is to grab the asset or folder ID from Preservica. Secondly, create a CSV. This will be where the relevant Dublin Core metadata is added/updated.

```python3 a_get_metadata.py [preservica reference number] [output.csv] ```

WARNING: The CSV output will require manual column realignment if there are repeating Dublin Core elements.

## semaphore-helper.py

The semaphore-helper script is a multi-part tool that firstly downloads a copy of each required asset from Preservica and then using the ICAEW taxonomy classifies up to ten subject terms for the Dublin Core subject fields.

Semaphore requires a preservica reference number, an area for each asset to be downloaded to and a output preferably CSV.

```python3 semaphore-helper.py [preservica ref] [asset download location] [output location > .csv]```

## Delete and Upload

* b_delete_metadata.py
* c_add_metadata_from_csv.py

During this stage, existing metadata will be deleted from the enclosed Preservica environment. Afterwards the new completed Dublin Core metadata will be added to the assets.

## b_delete_metadata.py

The b_delete_metadata.py permanently deletes metadata templates connected to the asset. It uses a preservica reference number to locate which assets metadata it will delete. Using a folders preservica reference number will delete each of its child files metadata templates.

```python3 b_delete_metadata.py [preservica_ref]```

WARNING: Each times this script is used, a Y/N prompt occurs. After Y the metadata is permanently deleted.

## c_add_metadata_from_csv.py

Lastly, the c_add_metadata.py tool adds the newly complete Dublin Core metadata CSV to the original assets within Preservica.

```python3 c_add_metadata_from_csv.py [csv_location]```

This tool uses the preservica reference numbers in the CSV to locate where to add the newly completed Dublin Core metadata template.

