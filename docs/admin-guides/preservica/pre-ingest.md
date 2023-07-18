# Appraisal and pre-ingest workflow

This page covers the appraisal and pre-ingest workflows before an asset is ingested into Preservica. Quarterly magazine is given as a working example of the workflows. However, the workflows should be applicable to all assets.

* Example: Quarterly
    * 4 files to be preserved.
    * Four paper issues of Quarterly were published, all in the year 2020. From March 2021, longer form articles that previously would have appeared in the Quarterly magazine have been incorporated into the Specials section of Insights on ICAEW's website, where they are arranged according to theme.

## Appraisal
### Run Brunnhilde:

Brunnhilde provides the three following features.

* virus check
* file analysis
* checksums

Run Brunnhilde using the following in cmd:

    brunnhilde.py source destination

**source** being the path to source directory or disk image

**destination** being the path to destination for reports


* Example: Quarterly.

        brunnhilde.py Desktop/Quarterly_example Desktop/brunnhilde_output

    Result

    * No viruses
    * 2 file types: separate version of PDFs.
    * No error.
    * No duplication.
    * Checksums created.

    No complications.

### Run Exiftool:
* Exiftool lists the properties of a document in a csv to be human readable. The following properties will be included: title, author, subject, keywords, producer, CreateDate, ModifyDate, FileName.
* Check the title properties, if the titles are misleading or completely incorrect. Strip the metadata from the file.

* Run Exiftool using the following in cmd:

        exiftool -csv -r -T -Title -Author -Subject -Keywords -Producer -CreateDate -ModifyDate -FileName "input" > "output.csv"

* Example: Quarterly

        exiftool -csv -r -T -Title -Author -Subject -Keywords -Producer -CreateDate -ModifyDate -FileName /home/digital-archivist/Desktop/Quarterly_example > Desktop/exiftool_output.csv
* Quarterly titles were either correct or blank. No need to strip the metadata. 

## Pre-Ingest
### Prepare a Submission Information Package (SIP)
* Creating a SIP requires: Fixity, Dublin Core descriptive metadata and the digital artefacts.
* For Fixity ICAEW uses the SHA-1 algorithim. 
* Dublin Core Metadata:


    | DC Terms    | Description |
    | :---        |    :----:   |
    | Title | The name given to the resource. |
    | Creator | An entity primarily responsible for making the content. |
    | Subject | The topic of the content of the resource. |
    | Subject | Often a resource has multiple topics. |
    | Description | An account of the content of the resource. |
    | Publisher | The entity responsible for making the resource available. |
    | Date | A date associated with an event in the life cycle of the resource. |
    | Type | The nature or genre of the content of the resource. |
    | Format | The physical or digital manifestation of the resource. |
    | Language | A language of the intellectual content of the resource. |
    | Relation | A reference to a related resource. |

* Each of these elements are placed in a folder, the fixity and descriptive metadata are nested in an .opex metadata template beside the digital artefact they represent.
* Each artefact including the folder must have a seperate .opex metadata document.
* The folder level .opex metadata document is unique to the other .opex files, its primary role is to include a manifest of the folder: both content and metadata.

### Python SIP creation tools

We have three seperate fit for purpose tools that achieve the objective of creating a Preservica SIP. The tools can be found [here](https://github.com/icaew-digital-archive/digital-archiving-scripts/tree/main/opex-scripts).

* Tool A: creates a .csv file with seperate SHA-1 hash's for each artefact. The csv file includes relevant dublin core fields to populate for the .opex metadata file. 

        a_files_to_csv.py [-h] [--hash_type {sha1,md5}] directory output

    After populating the .csv with the relevant dublin core metadata, move on to Tool B.

* Tool B: uses the completed .csv to create unique .opex metadata files for each digital artefact in the folder. Including fixity and dublin core metadata. 

        b_csv_to_opex_xml.py csv_file output_dir

    After this tool has successfully created a .opex metadata file for each digital artefact, move on to Tool C.

* Tool C: creates a folder level .opex metadata document. It scans the parent folder and creates an authenicated folder level .opex metadata document that serves as a manifest for the ingest.

        c_folders_of_files_to_folder_opex_xml.py folder_path

    When all three tools have completed there job the tree of the SIP should look like this Quarterly Example:

    * Folder: Quarterly-Example

        * Quarterly-Example.opex
        * Quarterly-1.pdf
        * Quarterly-1.pdf.opex
        * Quarterly-2.pdf
        * Quarterly-2.pdf.opex
        * Quarterly-3.pdf
        * Quarterly-3.pdf.opex
        * Quarterly-4.pdf
        * Quarterly-4.pdf.opex
