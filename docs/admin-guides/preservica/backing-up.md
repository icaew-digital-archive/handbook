# Backing Up

> **Purpose:** This document outlines the process for creating local backups of Preservica content.

> **Note:** This page is under development. Future additions will include:
> - Overview section
> - Backup Types section
> - Schedule section
> - Storage section
> - Verification section
> - Best Practices section

## Running the report and downloading the files

> **Purpose:** Local backups of the entire contents of Preservica should be made at least on a yearly basis.

**Process:**
- There is already a workflow setup for this purpose called _Full Export (v6)_ which is accessible from the Access and then Manage tab
- To run the full export, the workflow needs to be made active by clicking the tickbox
- The download will be made available at: `com.preservica.icaew.export`
- Access and downloading will be via [aws-cli](aws-cli.md) as usual

> **Important:** Preservica should be contacted before running this report as it is apparently process heavy. It should be run over the weekend to minimise any potential disruption to their servers.

## Unzipping the Preservica download

> **Purpose:** The Preservica backup download will be contained within .zip files, which in turn contain further .zip files (i.e., nested .zip files). In order to make the files more accessible, to generate local hashes, and for tools such as Brunnhilde/ssdeep to work as intended, the contents must be unzipped.

> **Warning:** The following script deletes the .zip source files while recursively unzipping them. This script should only be run on a copy of the Preservica download.

**Bash script to recursively unzip nested .zip files** (found [here](https://www.ddzheng.cc/?p=337)). The script should be run at the location of the .zip files:

    while [[ -n $(find . -name '*.zip') ]]
    do
    find . -name '*.zip'|awk -F'.zip' '{print "unzip -d "$1" "$0" && rm "$0""}'|sh
    done



