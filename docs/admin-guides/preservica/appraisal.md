# Appraisal

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "Assessment Criteria" section]
[NOTE: Add a "Tools and Methods" section]
[NOTE: Add a "Documentation" section]
[NOTE: Add a "Decision Making" section]
[NOTE: Add a "Best Practices" section]

### [Brunnhilde](https://github.com/tw4l/brunnhilde)

- Brunnhilde provides the three following features.

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
