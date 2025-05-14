# Appraisal

Appraisal is the decision to ingest into Preservica. 

During this decision we want to check if:

* The resource is of value to the Digital Repository,
* The resource will not impact the health of the Digital Repository or,
* We have permission to ingest the resource into the Digital Repository.

Note: Would be handy to have a collections guide here.

## Virus check

It is important that we ensure the ongoing health of the Digital Repository. During the appraisal workflow we achieve this by running Brunnhilde.
Brunnhilde also provides an overview of the resources we are planning to ingest, it helps spot any problem files before ingest.

### [Brunnhilde](https://github.com/tw4l/brunnhilde)

Brunnhilde provides the three following features:

  - virus check
  - file analysis
  - checksums

- Run Brunnhilde using the following in cmd:

```brunnhilde.py source destination```

- `source` being the path to source directory or disk image  
  `destination` being the path to destination for reports  
  Check for any complications such as 0 sized files and duplicates.

## Creating a series

Unfortunately there is no one size fits all for creating an archival series.

It is important to keep in mind good archival practice.

1. Respect Provenance
    * Maintain origianl structure where possible, unless it compromises usability or access
    * Group resources that have the same point of origin.
  
2. Define the Purpose of the Series.
    Why is this series being collated? Some examples
   * Faculty Publications
   * Annual Reports
   * Press Releases

3. Avoid overly deep hierarchies.
    The deeper a hierarchy the harder it is to manage and navigate.
    Try to keep the hierarchy manageable, some examples:
    * Root/Faculty Serials/Individual Serial/Year
    * Root/Press Releases/Year
    * Root/Business Confidence Monitor/BCM Sectors/Individual Sector

4. Keep Series Intellectually Cohesive
    * Avoid mixing formats or topics. Only consider to do this is they were originally grouped that way.
    * If digital assets span many types, consider seperating them into sub-series.

5. Work Collabratively
    Often the Digital Repository wont have the best understanding of the resources we appraise.
    Work collabratively with information stakeholders to best understand the resources, build the structure with their help.

After appraisal and creating a series structure for the resources it is time to being ingesting the resources.

