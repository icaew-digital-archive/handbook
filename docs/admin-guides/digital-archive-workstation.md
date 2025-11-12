# Digital Archive Workstation

## Overview

> **Purpose:** This document provides comprehensive information about the digital archiving workstation setup, including system configuration, installed software, and maintenance procedures.

## System Configuration

### Dual-Boot Setup

> **Configuration:** The workstation is configured as a dual-boot system running both Windows and Ubuntu/BitCurator, with Ubuntu/BitCurator serving as the primary operating system. To switch between the two at boot, press `F12`.

> **Note:** For login details to both Windows and Ubuntu/BitCurator, refer to the [Logins page](../logins.md).

## Installation Guide

### Prerequisites
- Windows operating system installed
- BitLocker disabled (if enabled)
- Minimum 8GB USB drive for Ubuntu installation
- Internet connection for downloads

### Installing Ubuntu/BitCurator
Follow these steps to set up a new laptop:

1. **Disable BitLocker (Mandatory)**  
   - If your system has BitLocker encryption enabled, you **must** disable it before proceeding
   - Boot into Windows, go to Control Panel → BitLocker → Turn off BitLocker
   - For detailed instructions, refer to [Ubuntu's guide](https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338)
  
2. **Download Ubuntu**  
   - Obtain the Ubuntu image from [ubuntu.com](https://ubuntu.com/tutorials/install-ubuntu-desktop)

3. **Create Bootable USB**  
   - Use Ubuntu's "Startup Disk Creator" or [balenaEtcher](https://www.balena.io/etcher/)
   - Follow the tool's instructions to create a bootable USB

4. **Install Ubuntu**  
   - Restart and press `F12` to boot from USB
   - Select "Install Ubuntu alongside Windows Boot Manager"
   - Follow the installation wizard

5. **Install BitCurator**  
   - Use the [SaltStack repository](https://github.com/bitcurator/bitcurator-salt)
   - Follow the installation instructions provided

## Installed Software

### Operating System and Core Tools
- **[Ubuntu](https://ubuntu.com/download/desktop)**  
  Core operating system optimized for web archiving tools

- **[BitCurator](https://bitcurator.net/)**  
  Digital archiving suite installed on Ubuntu

- **[Citrix Workspace](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)**  
  Version 23.3.0.32 (tested working) for VDI access via:
  
    - [MKPortal](https://www.mkportal.icaew.com)
    - [LonPortal](https://www.lonportal.icaew.com/)

### Development Tools
- **[Python](https://www.python.org/downloads/)**  
  Pre-installed with BitCurator
  ```bash
  # Install virtual environment support
  sudo apt install python3.10-venv
  
  # Create and activate virtual environment
  python3 -m venv venv
  . ./venv/bin/activate
  ```

- **[Git](https://git-scm.com/download/linux)**  
  Pre-installed with BitCurator
  ```bash
  # Configure Git
  git config --global user.name "FIRST_NAME LAST_NAME"
  git config --global user.email "MY_NAME@example.com"
  ```

- **[Visual Studio Code](https://code.visualstudio.com/download)**  
  Recommended extensions:
  - Python
  - Prettier
  - Markdown All in One

- **[Docker](https://docs.docker.com/engine/install/ubuntu/)**  
  Pre-installed with BitCurator
  ```bash
  # Pull browsertrix-crawler image
  docker pull webrecorder/browsertrix-crawler
  ```

### Web Archiving Tools
- **[Google Chrome/Chromium](https://www.google.com/chrome/)**  
  Required for browsertrix-crawler

- **[AWS CLI](https://aws.amazon.com/cli/)**  
  For large file ingestion into Preservica

- **[Node.js Tools]**  
  ```bash
  # Install Node Version Manager
  nvm install node
  
  # Install Yarn
  npm install -g yarn
  yarn install
  
  # Fix compilation errors
  export NODE_OPTIONS=--openssl-legacy-provider
  
  # Install Puppeteer
  npm i puppeteer
  ```

- **[Get cookies.txt](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)**  
  Chrome extension for cookie export

- **[Wget](https://www.gnu.org/software/wget/)**  
  Pre-installed file retrieval tool

- **[Thunar](https://docs.xfce.org/xfce/thunar/start)**  
  Bulk file rename utility
  ```bash
  sudo apt install thunar
  ```

### Digital Archiving Tools
- **[archiveweb.page](https://github.com/webrecorder/archiveweb.page)**  
  Chrome extension for web archiving

- **[replayweb.page](https://github.com/webrecorder/replayweb.page)**  
  Web archive replay tool

- **[OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF)**  
  PDF OCR tool

- **[ExifTool](https://github.com/exiftool/exiftool)**  
  Metadata management tool

- **[Heritrix](https://github.com/internetarchive/heritrix3)**  
  Web crawler

- **[youtube-dl](https://github.com/ytdl-org/youtube-dl)**  
  Video download tool
  ```bash
  pip install youtube-dl
  ```

- **[py-wacz](https://github.com/webrecorder/py-wacz)**  
  WACZ format handler

- **[py-wasapi-client](https://github.com/unt-libraries/py-wasapi-client)**  
  WARC file download client

### Documentation Tools
- **[MkDocs](https://www.mkdocs.org)**  
  Documentation generator
  ```bash
  pip install mkdocs
  ```

- **[Material](https://squidfunk.github.io/mkdocs-material/)**  
  MkDocs theme
  ```bash
  pip install mkdocs-material
  ```

## Additional Resources
- [BitCurator Documentation](https://wiki.bitcurator.net/)
- [Ubuntu Documentation](https://help.ubuntu.com/)
- [Docker Documentation](https://docs.docker.com/)
- [Python Documentation](https://docs.python.org/)
- [Node.js Documentation](https://nodejs.org/en/docs/)
