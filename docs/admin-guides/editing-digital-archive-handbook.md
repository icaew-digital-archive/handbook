# Editing the ICAEW Digital Archive Handbook

## Overview
This guide provides comprehensive instructions for editing and maintaining the ICAEW Digital Archive Handbook, which serves as the central resource for digital preservation and web archiving activities at ICAEW.

## Tools and Technologies

### Core Documentation Tools
* [MkDocs](https://www.mkdocs.org)  
_A static site generator that's geared towards building project documentation_

* [Material](https://squidfunk.github.io/mkdocs-material/)  
_Material theme for MkDocs_

### Development Environment Setup

#### Python Environment
MkDocs and Material require a Python installation. Good practice is to initialize a Python virtual environment before installing the libraries:

```bash
python3 -m venv venv
```

Activate the virtual environment:
```bash
. ./venv/bin/activate
```

#### Package Installation
Install MkDocs:
```bash
pip install mkdocs
```

Install Material theme:
```bash
pip install mkdocs-material
```

### Additional Tools
* [Visual Studio Code](https://code.visualstudio.com/)  
  Recommended code editor with Markdown support
  - Install the following extensions:
    - Markdown All in One
    - Markdown Preview Enhanced
    - Python
    - Prettier

* [Git](https://git-scm.com/)  
  Version control system for tracking changes

* [Python Virtual Environment](https://docs.python.org/3/library/venv.html)  
  For managing Python dependencies

## Development Workflow

### GitHub Workflow
When contributing to the handbook, follow these steps to manage your changes:

1. **Create a branch from `master`:**
   ```bash
   git checkout -b craig
   ```

2. **Make your changes (edit, stage, commit):**

    Note: This section will usually be done in the GUI of VSCode rather than via these commands.

    ```bash
    git add .
    git commit -m "Add awesome feature"
    ```

3. **Push your branch to GitHub:**
   ```bash
   git push -u origin craig
   ```

4. **Keep your branch up to date with `master`:**
   ```bash
   git fetch origin && git merge origin/master && git push
   ```

### Local Development
To edit and preview changes in real-time:
```bash
mkdocs serve
```

### Building Static Site
To create a static site for offline viewing (e.g., in the ICAEW VDI):
```bash
mkdocs build
```

This creates a static website in the `/site/` directory.

### Deployment
The contents of `/site/` should be copied to:
```
G:\Apps\Passport\ICAEW Digital Archive Handbook
```

## Documentation Structure

### File Organization
- Main documentation files are located in the `/docs/` directory
- Configuration is managed in `mkdocs.yml`
- Custom CSS can be added in `/assets/css/custom.css`

### Navigation
The navigation structure is defined in `mkdocs.yml`. Key sections include:

- User Guides
- Admin Guides
- Useful Resources
- Logins

