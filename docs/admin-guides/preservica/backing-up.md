# Backing Up

[NOTE: Add a "Overview" section]
[NOTE: Add a "Backup Types" section]
[NOTE: Add a "Schedule" section]
[NOTE: Add a "Storage" section]
[NOTE: Add a "Verification" section]
[NOTE: Add a "Best Practices" section]

## Running the report and downloading the files

Local backups of the entire contents of Preservica should be made at least on a yearly basis.

There is already a workflow setup for this purpose called _Full Export (v6)_ which is accessible from the Access and then Manage tab. To run the full export the workflow needs to be made active by clicking the tickbox.

The download will be made available at: `com.preservica.icaew.export`. Access and downloading will be [aws-cli](aws-cli.md) as usual.

**Note: Preservica should be contacted before running this report as it is apparently process heavy. It should be run over the weekend to minimise any potential disruption to their servers.**

## Unzipping the Preservica download

The Preservica backup download will be contained within .zip files, which in turn contain further .zip files (i.e., nested .zip files). In order to make the files more accessible, to generate local hashes, and for tools such as Brunnhilde/ssdeep to work as intended the contents must be unzipped.

**WARNING: The following script deletes the .zip source files while recursively unzipping them. This script should only be run on a copy of the Preservica download**

A working bash script to recursively unzip nested .zip files (found [here](https://www.ddzheng.cc/?p=337)), the script should be run at the location of the .zip files:

    while [[ -n $(find . -name '*.zip') ]]
    do
    find . -name '*.zip'|awk -F'.zip' '{print "unzip -d "$1" "$0" && rm "$0""}'|sh
    done



