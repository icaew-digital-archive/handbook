# Appraisal

> **Purpose:** Appraisal is the decision to ingest into Preservica. During this decision we want to check if:
> - The resource is of value to the Digital Repository
> - The resource will not impact the health of the Digital Repository
> - We have permission to ingest the resource into the Digital Repository

> **Note:** Would be handy to have a collections guide here.

## Virus check

> **Purpose:** It is important that we ensure the ongoing health of the Digital Repository. During the appraisal workflow we achieve this by running Brunnhilde. Brunnhilde also provides an overview of the resources we are planning to ingest, helping to spot any problem files before ingest.

### [Brunnhilde](https://github.com/tw4l/brunnhilde)

Brunnhilde provides the following features:

- Virus check
- File analysis
- Checksums

**Command:**
```bash
brunnhilde.py source destination
```

**Parameters:**
- `source` - Path to source directory or disk image  
- `destination` - Path to destination for reports

> **Tip:** Check for any complications such as 0-sized files and duplicates.

## Check the G:Drive

> **Purpose:** When appraising a collection of documents, it is important to check for similar resources in the G:Drive from the VDI.

**Location:**
```
G:\MD\LIS\LIBRARY AND INFORMATION SERVICE\ICAEW digital repository
```

> **Note:** The G:Drive is a place where documents have already been sorted into hierarchies. They may not always have a full collection but can be useful for laying the groundwork to create a series in Preservica.

**When searching the G:Drive:**
- Look for duplicates and additional resources for the collections you are appraising
- If duplicates are found, remove them from the G:Drive after ingest
- If additional resources are found, transfer them from the VDI onto your local machine using the Digital Archive Sharepoint


## Creating a series

> **Note:** Unfortunately there is no one-size-fits-all approach for creating an archival series. It is important to keep in mind good archival practice.

### Guidelines

1. **Respect Provenance**
   - Maintain original structure where possible, unless it compromises usability or access
   - Group resources that have the same point of origin

2. **Define the Purpose of the Series**
   - Why is this series being collated? Some examples:
     - Faculty Publications
     - Annual Reports
     - Press Releases

3. **Avoid Overly Deep Hierarchies**
   - The deeper a hierarchy, the harder it is to manage and navigate
   - Try to keep the hierarchy manageable. Some examples:
     - `Root/Faculty Serials/Individual Serial/Year`
     - `Root/Press Releases/Year`
     - `Root/Business Confidence Monitor/BCM Sectors/Individual Sector`

4. **Keep Series Intellectually Cohesive**
   - Avoid mixing formats or topics. Only consider doing this if they were originally grouped that way
   - If digital assets span many types, consider separating them into sub-series

5. **Work Collaboratively**
   - Often the Digital Repository won't have the best understanding of the resources we appraise
   - Work collaboratively with information stakeholders to best understand the resources and build the structure with their help

> **Next Step:** After appraisal and creating a series structure for the resources, it is time to begin ingesting the resources.

