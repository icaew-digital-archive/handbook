# Post-Ingest

[NOTE: Add a "Verification Steps" section]
[NOTE: Add a "Quality Checks" section]
[NOTE: Add a "Metadata Review" section]
[NOTE: Add a "Access Testing" section]
[NOTE: Add a "Documentation" section]
[NOTE: Add a "Troubleshooting" section]

## Update Catalogue entries

Post-ingest the ICAEW catalogue needs updating to include the new access point in Preservica. This is achieved by adding a new 856 field in Symphony.

The formatting for adding an 856 field in Symphony is as follows:

|aAvailable at the ICAEW Digital Repository: |u*hyperlink*

(To be discussed)
If we add unique identifiers alongside Dublin Core metadata, these should be added into the catalogue as well.

**TODO: Write post-ingest processes**

## Appendix:

Processes that are no longer used.

- **Semaphore CLS Client and semaphore-subject-import.py**

  The semaphore-subject-import.py script combines the CSV output from the Semaphore CLS Client, negating the need for copy and pasting from one CSV to the other. Usage is as follows:

        semaphore-subject-import.py semaphore_csv dublin_core_csv csv_output
