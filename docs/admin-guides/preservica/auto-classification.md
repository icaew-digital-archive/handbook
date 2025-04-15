# Auto Classification for DC Metadata

[NOTE: Add a "Overview" section]
[NOTE: Add a "Configuration" section]
[NOTE: Add a "Rules Setup" section]
[NOTE: Add a "Process Flow" section]
[NOTE: Add a "Validation" section]
[NOTE: Add a "Best Practices" section]

ICAEW uses [Smartlogic Semaphore](https://cloud.smartlogic.com/spa/) to auto-classify material using a custom made taxonomy. The _Classification & Language Service Client_ tool provides subject classification for documents stored locally. We have decided to include only subjects with a classification threshold of 48% and above - this matches what is happening on ICAEW.com. Topics can be removed at the descretion of the digital archivist if they feel the document has been assigned too many subjects.

This tool will mostly be used in conjunction with the **semaphore-helper.py** script.

`--cloud-api-key` can be found on the [Logins page](../../logins.md).

CLI example:

    java -jar Semaphore-CLSClient-5.6.3.jar --cloud-api-key= --url=https://cloud.smartlogic.com/svc/138b5bab-8ac4-45e0-b36f-815008f9921d/ --threshold=48 --csv-output-file subject-terms-output.csv input


## Things to note:

- threshold=48 is the 48% classification threshold
- The output must be a csv
- We use the terms from the Generic_UPWARD column

## The Classification & Language Service Client

The _Classification & Language Service Client_ tool can be downloaded from the Smartlogic portal, along with the documentation: [CLS-Client](https://portal.smartlogic.com/docs/5.6/classification_server_-_developers_guide/the_command_line_client)
