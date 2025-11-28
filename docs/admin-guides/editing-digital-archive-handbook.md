# Editing the ICAEW Digital Archive Handbook

## Overview

> **Purpose:** This guide provides comprehensive instructions for editing and maintaining the ICAEW Digital Archive Handbook, which serves as the central resource for digital preservation and web archiving activities at ICAEW.

## Tools and Technologies

### Core Documentation Tools
* [MkDocs](https://www.mkdocs.org)  
_A static site generator that's geared towards building project documentation_

* [Material](https://squidfunk.github.io/mkdocs-material/)  
_Material theme for MkDocs_

### Development Environment Setup

#### Python Environment

> **Tip:** MkDocs and Material require a Python installation. Good practice is to initialize a Python virtual environment before installing the libraries:

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

---

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

This starts a local development server (typically at `http://127.0.0.1:8000`) that automatically reloads when you save changes to your Markdown files.

### Building Static Site
To create a static site for offline viewing (e.g., in the ICAEW VDI):
```bash
mkdocs build
```

This creates a static website in the `/site/` directory. **Note:** The `/site/` directory is excluded from Git (see `.gitignore`) as it's generated content.

---

### Deployment
The contents of `/site/` should be copied to:
```
G:\Apps\Passport\ICAEW Digital Archive Handbook
```

**Deployment steps:**
1. Build the static site: `mkdocs build`
2. Copy the entire contents of the `/site/` directory to the deployment location
3. Ensure all files are copied, including assets, JavaScript, and CSS files

---

## Documentation Structure

### File Organization
- Main documentation files are located in the `/docs/` directory
- Configuration is managed in `mkdocs.yml`
- Custom CSS can be added in `/docs/assets/css/custom.css`
- Images should be placed in `/docs/assets/images/`
- The generated site is output to `/site/` (excluded from Git)

### Navigation
The navigation structure is defined in `mkdocs.yml` under the `nav:` section. Key sections include:

- User Guides
- Admin Guides
- Useful Resources
- Logins

To add a new page:
1. Create the Markdown file in the appropriate directory under `/docs/`
2. Add the entry to the `nav:` section in `mkdocs.yml`
3. Follow the existing indentation and formatting pattern

---

### Adding Images
1. Place image files in `/docs/assets/images/`
2. Reference them in Markdown using:
   ```markdown
   ![Alt text](assets/images/filename.png)
   ```
3. Supported formats: PNG, JPG, JPEG, GIF, SVG

---

## Best Practices

### Markdown Guidelines
- Use clear, descriptive headings
- Keep line length reasonable (80-100 characters)
- Use code blocks with language identifiers for syntax highlighting
- Use blockquotes (`>`) for important notes or warnings
- Use consistent formatting for code snippets, file paths, and commands

### Content Organization
- Keep pages focused on a single topic
- Use consistent terminology throughout
- Cross-reference related pages using relative links
- Update the navigation in `mkdocs.yml` when adding new sections

### Writing Style
- Write in clear, concise language
- Use active voice when possible
- Include step-by-step instructions for procedures
- Add context and purpose for technical procedures

## Troubleshooting

### Common Issues

**MkDocs serve not working:**
- Ensure the virtual environment is activated
- Check that MkDocs is installed: `pip list | grep mkdocs`
- Try rebuilding: `mkdocs build` then `mkdocs serve`

**Changes not appearing:**
- Hard refresh your browser (Ctrl+F5 or Cmd+Shift+R)
- Check that you're editing files in `/docs/`, not `/site/`
- Verify the file is included in `mkdocs.yml` navigation

**Build errors:**
- Check Markdown syntax for errors
- Verify all linked files exist
- Check `mkdocs.yml` for syntax errors (YAML is sensitive to indentation)

**Images not displaying:**
- Verify the image path is correct relative to the Markdown file
- Check that the image file exists in `/docs/assets/images/`
- Ensure the image file is committed to Git

---

## Additional Resources
- [MkDocs Documentation](https://www.mkdocs.org/user-guide/)
- [Material for MkDocs Documentation](https://squidfunk.github.io/mkdocs-material/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Git Documentation](https://git-scm.com/doc)

