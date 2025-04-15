# Editing the ICAEW Digital Archive Handbook

[NOTE: Add a "Style Guide" section]
[NOTE: Add a "Markdown Guidelines" section]
[NOTE: Add a "Review Process" section]
[NOTE: Add a "Version Control" section]
[NOTE: Add a "Testing" section]
[NOTE: Add a "Deployment" section]

This ICAEW Digital Archive Handbook is produced using:

* [MkDocs](https://www.mkdocs.org)  
_A static site generator that's geared towards building project documentation_

* [Material](https://squidfunk.github.io/mkdocs-material/)  
_Material theme for MkDocs_

MkDocs and Material require a Python installation. Good practice is to initialize a Python virtual environment before installing the libraries. This is done via:  

        python3 -m venv venv
    
and then activating the virtual environment using:  
    
        . ./venv/bin/activate

MkDocs is installed via:  

        pip install mkdocs

Material is installed via:  

        pip install mkdocs-material

The following command allows the mkdocs.yml and the files in /docs/ to be edited with changes seen in realtime:  

        mkdocs serve

In order to create a static site (which can then be copied over for offline viewing in the ICAEW VDI), the following command is run:  

        mkdocs build

This will create a static website located in the project directory under /site/. The contents of this folder can be transferred anywhere where the documentation needs to be viewed, i.e. inside the ICAEW VDI.  

The contents of /site/ should be copied over to G:\Apps\Passport\ICAEW Digital Archive Handbook.