# Standard Ingest Workflow

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "Required Tools" section]
[NOTE: Add a "Step-by-Step Guide" section]
[NOTE: Add a "Quality Checks" section]
[NOTE: Add a "Troubleshooting" section]
[NOTE: Add a "Best Practices" section]

This page outlines the critical steps of appraisal and pre-ingest processes that are followed before an asset is ingested into **Preservica**. These workflows ensure thorough evaluation, metadata assignment, and asset preparation to maintain integrity and compliance throughout the digital preservation lifecycle.

```mermaid
flowchart LR
    id1[Appraisal \n\n Brunnhilde \n Exiftool] --> id2
    id2[Pre-ingest \n\n SIP creation via custom Python scripts \n Fixity \n Dublin Core metadata creation \n File renaming] --> id3[Post-ingest \n\n Miscellaneous tasks: \n Update catalogue entries \n Migration and checking quality]
```

### Workflow Breakdown:

- **Appraisal**: Tools like **Brunnhilde** and **Exiftool** are used to evaluate and assess the digital assets for preservation.
- **Pre-Ingest**: Preparation steps include creating **SIP (Submission Information Packages)** through custom Python scripts, performing fixity checks, adding **Dublin Core** metadata, and ensuring proper file renaming.
- **Post-Ingest**: After ingestion, miscellaneous tasks such as updating catalogue entries, migration, and quality checking are performed to ensure asset integrity and accessibility.

```