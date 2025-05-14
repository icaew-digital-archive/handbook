# Pre-Ingest

## Prerequisites
- Python 3.x environment with required packages
- Access to Preservica API credentials
- Understanding of Dublin Core metadata standards
- Familiarity with OPEX package structure

## File Preparation
1. Organize files in a clean directory structure
2. Ensure files follow naming conventions
3. Remove any temporary or system files
4. Verify file integrity and readability

## Metadata Creation
The pre-ingest process uses a series of Python scripts to create Submission Information Packages (SIPs):

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

- Creating a SIP requires: Fixity, Dublin Core descriptive metadata and the digital artefacts.
- For Fixity ICAEW uses the SHA-1 algorithim.
- Refer to the [Dublin Core](./dublin-core.md) page to understand how ICAEW uses it.

## SIP

- Each of these elements are placed in a folder, the fixity and descriptive metadata are nested in an .opex metadata template beside the digital artefact they represent.
- Each artefact including the folder must have a seperate .opex metadata document.
- The folder level .opex metadata document is unique to the other .opex files, its primary role is to include a manifest of the folder: both content and metadata.

### Python SIP creation tools

We have multiple tools that achieve the objective of creating a Preservica SIP. The tools can be found on the [icaew-digital-archive](https://github.com/icaew-digital-archive/digital-archiving-scripts/tree/main/opex-scripts) GitHub page.

- **Tool A**: creates a .csv file with seperate SHA-1 hashes for each artefact and includes relevant dublin core fields to populate for the .opex metadata file.

          a_files_to_csv.py [-h] [--hash_type {sha1,md5}] directory output
Populate the .csv with the relevant Dublin Core metadata - leaving the Subject fields for the next stage of the process.

- **Semaphore auto-classification (via semaphore-helper.py)**

          usage: semaphore-helper.py [-h] [--preservica_ref PRESERVICA_REF] directory
Make use of Semaphore's Classification Server via semaphore-helper.py. This script acts as a wrapper around the Semamphore CLS client and outputs a maximum of 10 subject topics, sorted by scoring. The script accepts a local directory or a preservica folder reference can be supplied directly to the script. Further information regarding the Semaphore CLS client can be found [here](./auto-classification.md). After populating the .csv with the relevant dublin core metadata, move on to the rename tool.

- **Rename tool**: renames the original filenames (found in the 'filename' column) to filenames given in the 'Title' column from the csv produced by a_files_to_csv.py. Use the output CSV file from semaphore-subject-import.py (unless the step was skipped for whatever reason).

          csv_file_rename.py file_directory csv_file_path
After using the script to rename the files, move on to Tool B.  
  **Note: after running the script you should also copy the contents of the 'Title' field over to 'filename' in the csv file as Tool B uses this column to name the .opex files. You will also need to re-add the file extensions if following this method.**

- **Tool B**: uses the completed .csv to create unique .opex metadata files for each digital artefact in the folder. Including fixity and dublin core metadata.

          b_csv_to_opex_xml.py csv_file output_dir
After this tool has successfully created a .opex metadata file for each digital artefact, move on to Tool C.

- **Tool C**: creates a folder level .opex metadata document. It scans the parent folder and creates an authenicated folder level .opex metadata document that serves as a manifest for the ingest. The folder level opex file can then be edited before ingest if needed, i.e. adding/changing the Title, Description, SecurityDescriptor and other Dublin Core metadata.

        c_folders_of_files_to_folder_opex_xml.py folder_path