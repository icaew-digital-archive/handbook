# Archived Pages

> **Purpose:** This document contains reference information for various Preservica processes and workflows. For current workflows, see the dedicated pages in this section.

> **Note:** This page contains legacy and reference information. Some sections may be duplicated or outdated. Please refer to the main workflow pages for current processes.

## Appraisal Tools

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "Assessment Criteria" section]
[NOTE: Add a "Tools and Methods" section]
[NOTE: Add a "Documentation" section]
[NOTE: Add a "Decision Making" section]
[NOTE: Add a "Best Practices" section]

### [Brunnhilde](https://github.com/tw4l/brunnhilde)

Brunnhilde provides the three following features:

  - virus check
  - file analysis
  - checksums

- Run Brunnhilde using the following in cmd:

            brunnhilde.py source destination

- `source` being the path to source directory or disk image  
  `destination` being the path to destination for reports  
  Check for any complications such as 0 sized files and duplicates.

### [ExifTool](https://exiftool.org/#running)

- Exiftool lists the properties of a document in a csv to be human readable. The following properties will be included: title, author, subject, keywords, producer, CreateDate, ModifyDate, FileName.
- Check the title properties, if the titles are misleading or completely incorrect. Strip the metadata from the file if any are found to be misleading or completely incorrect.

- Run Exiftool using the following in cmd:

        exiftool -csv -r -T -Title -Author -Subject -Keywords -Producer -CreateDate -ModifyDate -FileName "input file/folder" > "output.csv"

**TODO: Cover how to strip metadata from PDFs, doc, docx etc. here.**

# Archived Pages

> **Purpose:** This document contains reference information for various Preservica processes and workflows. For current workflows, see the dedicated pages in this section.

> **Note:** This page contains legacy and reference information. Some sections may be duplicated or outdated. Please refer to the main workflow pages for current processes.

## Appraisal Tools

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "Assessment Criteria" section]
[NOTE: Add a "Tools and Methods" section]
[NOTE: Add a "Documentation" section]
[NOTE: Add a "Decision Making" section]
[NOTE: Add a "Best Practices" section]

### [Brunnhilde](https://github.com/tw4l/brunnhilde)

Brunnhilde provides the three following features:

  - virus check
  - file analysis
  - checksums

- Run Brunnhilde using the following in cmd:

            brunnhilde.py source destination

- `source` being the path to source directory or disk image  
  `destination` being the path to destination for reports  
  Check for any complications such as 0 sized files and duplicates.

### [ExifTool](https://exiftool.org/#running)

- Exiftool lists the properties of a document in a csv to be human readable. The following properties will be included: title, author, subject, keywords, producer, CreateDate, ModifyDate, FileName.
- Check the title properties, if the titles are misleading or completely incorrect. Strip the metadata from the file if any are found to be misleading or completely incorrect.

- Run Exiftool using the following in cmd:

        exiftool -csv -r -T -Title -Author -Subject -Keywords -Producer -CreateDate -ModifyDate -FileName "input file/folder" > "output.csv"

**TODO: Cover how to strip metadata from PDFs, doc, docx etc. here.**

## Auto Classification for DC Metadata

> **Purpose:** ICAEW uses [Smartlogic Semaphore](https://cloud.smartlogic.com/spa/) to auto-classify material using a custom made taxonomy. The _Classification & Language Service Client_ tool provides subject classification for documents stored locally.

> **Note:** This section is under development. Future additions will include:
> - Overview section
> - Configuration section
> - Rules Setup section
> - Process Flow section
> - Validation section
> - Best Practices section

> **Configuration:** We have decided to include only subjects with a classification threshold of 48% and above - this matches what is happening on ICAEW.com. Topics can be removed at the discretion of the digital archivist if they feel the document has been assigned too many subjects.

> **Usage:** This tool will mostly be used in conjunction with the **semaphore-helper.py** script.

> **Credentials:** `--cloud-api-key` can be found on the [Logins page](../../logins.md).

**CLI example:**
```bash
java -jar Semaphore-CLSClient-5.6.3.jar --cloud-api-key= --url=https://cloud.smartlogic.com/svc/138b5bab-8ac4-45e0-b36f-815008f9921d/ --threshold=48 --csv-output-file subject-terms-output.csv input
```

> **Things to note:**
> - `threshold=48` is the 48% classification threshold
> - The output must be a CSV
> - We use the terms from the Generic_UPWARD column

### The Classification & Language Service Client

> **Reference:** The _Classification & Language Service Client_ tool can be downloaded from the Smartlogic portal, along with the documentation: [CLS-Client](https://portal.smartlogic.com/docs/5.6/classification_server_-_developers_guide/the_command_line_client)

## Post-Ingest

> **Reference:** For current post-ingest processes, see [Post Ingest](../preservica/post-ingest.md).

> **Note:** This section is under development. Future additions will include:
> - Verification Steps section
> - Quality Checks section
> - Metadata Review section
> - Access Testing section
> - Documentation section
> - Troubleshooting section

### Update Catalogue entries

> **Purpose:** Post-ingest, the ICAEW catalogue needs updating to include the new access point in Preservica. This is achieved by adding a new 856 field in Symphony.

**Formatting for adding an 856 field in Symphony:**
```
|aAvailable at the ICAEW Digital Repository: |u*hyperlink*
```

> **Note:** (To be discussed) If we add unique identifiers alongside Dublin Core metadata, these should be added into the catalogue as well.

> **TODO:** Write post-ingest processes

### Appendix: Legacy Processes

> **Note:** Processes that are no longer used.

- **Semaphore CLS Client and semaphore-subject-import.py**

  > **Purpose:** The semaphore-subject-import.py script combines the CSV output from the Semaphore CLS Client, negating the need for copy and pasting from one CSV to the other.

  **Usage:**
  ```bash
  semaphore-subject-import.py semaphore_csv dublin_core_csv csv_output
  ```

## Pre-Ingest

> **Reference:** For current pre-ingest processes, see [Ingest Workflow](../preservica/ingest-workflow.md) and [Standard Ingest Workflow](../preservica/standard-ingest-workflow.md).

### Prerequisites

**Required:**
- Python 3.x environment with required packages
- Access to Preservica API credentials
- Understanding of Dublin Core metadata standards
- Familiarity with OPEX package structure

## File Preparation
1. Organize files in a clean directory structure
2. Ensure files follow naming conventions
3. Remove any temporary or system files
4. Verify file integrity and readability

### Metadata Creation

> **Purpose:** The pre-ingest process uses a series of Python scripts to create Submission Information Packages (SIPs):

### 1. Generate Initial CSV (`a_files_to_csv.py`)
```bash
python3 a_files_to_csv.py [-h] [--hash_type {sha1,md5}] directory output
```
This script:
- Creates a CSV file with SHA-1/MD5 hashes for each artifact
- Includes Dublin Core fields for .opex metadata
- Preserves original file information

### 2. Auto-classification (`semaphore-helper.py`)
```bash
python3 semaphore-helper.py [-h] [--preservica_ref PRESERVICA_REF] directory
```
This script:
- Uses Semaphore's Classification Server
- Outputs up to 10 subject topics
- Sorts by topic score
- Accepts local directory or Preservica folder reference

### 3. File Renaming (`csv_file_rename.py`)
```bash
python3 csv_file_rename.py file_directory csv_file_path
```
This script:
- Renames files based on Title field from CSV
- Preserves file extensions
- Updates CSV with new filenames

### 4. Generate OPEX Files (`b_csv_to_opex_xml.py`)
```bash
python3 b_csv_to_opex_xml.py csv_file output_dir
```
This script:
- Creates unique .opex metadata files
- Includes fixity and Dublin Core metadata
- Validates XML structure

### 5. Create Folder OPEX (`c_folders_of_files_to_folder_opex_xml.py`)
```bash
python3 c_folders_of_files_to_folder_opex_xml.py folder_path
```
This script:
- Creates folder-level .opex metadata
- Generates manifest for ingest
- Allows manual editing of metadata

## Quality Checks
1. Verify all files have corresponding .opex files
2. Check metadata completeness
3. Validate XML structure
4. Confirm file integrity
5. Review folder structure

## Validation
- Run XML validation on all .opex files
- Verify checksums match
- Check Dublin Core compliance
- Test folder structure integrity

## Best Practices
1. Always work in a clean directory
2. Keep original files backed up
3. Document any manual changes
4. Use consistent naming conventions
5. Validate at each step
6. Test with a small batch first

## Troubleshooting
Common issues and solutions:
1. Missing metadata fields
2. Invalid XML structure
3. Checksum mismatches
4. File permission issues
5. API connection problems

## Support
For issues or questions, contact the Digital Archive team.

### Prepare a Submission Information Package (SIP)

> **Purpose:** Creating a SIP requires: Fixity, Dublin Core descriptive metadata and the digital artefacts.

> **Note:** 
> - For Fixity, ICAEW uses the SHA-1 algorithm
> - Refer to the [Dublin Core](./dublin-core.md) page to understand how ICAEW uses it

**SIP Structure:**
- Each of these elements are placed in a folder, the fixity and descriptive metadata are nested in an .opex metadata template beside the digital artefact they represent
- Each artefact including the folder must have a separate .opex metadata document
- The folder level .opex metadata document is unique to the other .opex files, its primary role is to include a manifest of the folder: both content and metadata

### Python SIP creation tools

> **Purpose:** We have multiple tools that achieve the objective of creating a Preservica SIP. The tools can be found on the [icaew-digital-archive](https://github.com/icaew-digital-archive/digital-archiving-scripts/tree/main/opex-scripts) GitHub page.

**Tool A:** Creates a .csv file with separate SHA-1 hashes for each artefact and includes relevant Dublin Core fields to populate for the .opex metadata file.

**Command:**
```bash
a_files_to_csv.py [-h] [--hash_type {sha1,md5}] directory output
```

> **Note:** Populate the .csv with the relevant Dublin Core metadata - leaving the Subject fields for the next stage of the process.

**Semaphore auto-classification (via semaphore-helper.py):**

**Command:**
```bash
semaphore-helper.py [-h] [--preservica_ref PRESERVICA_REF] directory
```

> **Purpose:** Make use of Semaphore's Classification Server via semaphore-helper.py. This script acts as a wrapper around the Semaphore CLS client and outputs a maximum of 10 subject topics, sorted by scoring. The script accepts a local directory or a Preservica folder reference can be supplied directly to the script. Further information regarding the Semaphore CLS client can be found [here](./auto-classification.md). After populating the .csv with the relevant Dublin Core metadata, move on to the rename tool.

**Rename tool:** Renames the original filenames (found in the 'filename' column) to filenames given in the 'Title' column from the csv produced by a_files_to_csv.py. Use the output CSV file from semaphore-subject-import.py (unless the step was skipped for whatever reason).

**Command:**
```bash
csv_file_rename.py file_directory csv_file_path
```

> **Important:** After running the script, you should also copy the contents of the 'Title' field over to 'filename' in the csv file as Tool B uses this column to name the .opex files. You will also need to re-add the file extensions if following this method.

**Tool B:** Uses the completed .csv to create unique .opex metadata files for each digital artefact in the folder. Including fixity and Dublin Core metadata.

**Command:**
```bash
b_csv_to_opex_xml.py csv_file output_dir
```

> **Next Step:** After this tool has successfully created a .opex metadata file for each digital artefact, move on to Tool C.

**Tool C:** Creates a folder level .opex metadata document. It scans the parent folder and creates an authenticated folder level .opex metadata document that serves as a manifest for the ingest. The folder level opex file can then be edited before ingest if needed, i.e. adding/changing the Title, Description, SecurityDescriptor and other Dublin Core metadata.

**Command:**
```bash
c_folders_of_files_to_folder_opex_xml.py folder_path
```

## Reclassification

> **Purpose:** The following document outlines the reclassification workflow.

> **Note:** This section is under development. Future additions will include:
> - Prerequisites section
> - Classification Rules section
> - Process Steps section
> - Validation section
> - Error Handling section
> - Best Practices section

### Overview

> **Purpose:** The reclassification workflow downloads assets from the ICAEW Preservica digital archive. Then using Python tools and the Semaphore taxonomy, each asset is re-classified with Dublin Core metadata. Upon completing the Dublin Core metadata CSV, the existing metadata connected to the asset will be deleted. The new metadata will then be connected to the asset. Four tools are used in the workflow, the workflow has two stages.

> **Prerequisites:** Each of these tools require that the operator is within a virtual environment in the relevant directory.

**Activate virtual environment:**
```bash
. ./venv/bin/activate
```

### Download and Classify

**Tools used:**
- `a_get_metadata.py`
- `semaphore-helper.py`

> **Purpose:** During this stage, assets and existing metadata will be downloaded from Preservica. Using semaphore, each of these assets will be re-classified and any additional metadata will be added/updated.

#### a_get_metadata.py

> **Purpose:** The first step is to download the existing metadata for the assets that are to be reclassified. First step is to grab the asset or folder ID from Preservica. Secondly, create a CSV. This will be where the relevant Dublin Core metadata is added/updated.

**Command:**
```bash
python3 a_get_metadata.py [preservica reference number] [output.csv]
```

> **Warning:** The CSV output will require manual column realignment if there are repeating Dublin Core elements.

#### semaphore-helper.py

> **Purpose:** The semaphore-helper script is a multi-part tool that firstly downloads a copy of each required asset from Preservica and then using the ICAEW taxonomy classifies up to ten subject terms for the Dublin Core subject fields.

**Requirements:**
- Preservica reference number
- Area for each asset to be downloaded to
- Output preferably CSV

**Command:**
```bash
python3 semaphore-helper.py [preservica ref] [asset download location] [output location > .csv]
```

### Delete and Upload

**Tools used:**
- `b_delete_metadata.py`
- `c_add_metadata_from_csv.py`

> **Purpose:** During this stage, existing metadata will be deleted from the enclosed Preservica environment. Afterwards the new completed Dublin Core metadata will be added to the assets.

#### b_delete_metadata.py

> **Purpose:** The `b_delete_metadata.py` permanently deletes metadata templates connected to the asset. It uses a Preservica reference number to locate which assets' metadata it will delete. Using a folder's Preservica reference number will delete each of its child files' metadata templates.

**Command:**
```bash
python3 b_delete_metadata.py [preservica_ref]
```

> **Warning:** Each time this script is used, a Y/N prompt occurs. After Y, the metadata is permanently deleted.

#### c_add_metadata_from_csv.py

> **Purpose:** Lastly, the `c_add_metadata_from_csv.py` tool adds the newly complete Dublin Core metadata CSV to the original assets within Preservica.

**Command:**
```bash
python3 c_add_metadata_from_csv.py [csv_location]
```

> **Note:** This tool uses the Preservica reference numbers in the CSV to locate where to add the newly completed Dublin Core metadata template.




