# Digital Archive Workstation

[NOTE: Add a "Prerequisites" section]
[NOTE: Add a "System Requirements" section]
[NOTE: Add a "Installation Steps" section]
[NOTE: Add a "Configuration Guide" section]
[NOTE: Add a "Maintenance" section]
[NOTE: Add a "Troubleshooting" section]

## Overview of the installed software on the digital archiving workstation

This document provides an overview of the software currently installed on our digital archiving workstation.

The workstation is configured as a dual-boot system running both Windows and Ubuntu/BitCurator, with Ubuntu/BitCurator serving as the primary operating system. To switch between the two at boot, press `F12`.

For login details to both Windows and Ubuntu/BitCurator, refer to the [Logins page](../logins.md).


## Installing Ubuntu/BitCurator on a New Laptop

If you are setting up a new laptop, ensure Windows is installed first. Ubuntu/BitCurator will be installed alongside Windows in a dual-boot configuration.

1. **Disable BitLocker (Mandatory)**:  
   - If your system has BitLocker encryption enabled, you **must** disable it before proceeding with the installation of Ubuntu.
   - Boot into Windows, go to the Control Panel, and search for "BitLocker." Select "Manage BitLocker" and turn it off. You may need to temporarily turn it on and then off again.
   - For more detailed instructions, refer to [this guide](https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338).
  
2. **Download Ubuntu**: Obtain the Ubuntu image from [this link](https://ubuntu.com/tutorials/install-ubuntu-desktop).

3. **Create a Bootable USB**: Use Ubuntu's "Startup Disk Creator" or [balenaEtcher](https://www.balena.io/etcher/) as a cross-platform alternative to create a bootable USB stick.

4. **Boot from USB**: Restart the laptop and press `F12` at boot to select the USB drive as the boot device.

5. **Install Ubuntu**:  
   - Follow the installation process and select "Install Ubuntu alongside Windows Boot Manager" when prompted.

6. **Install BitCurator**: Once Ubuntu is installed, proceed with installing BitCurator using the [SaltStack repository](https://github.com/bitcurator/bitcurator-salt).


## General Software

Below is a list of general software installed on the workstation:

- **[Ubuntu](https://ubuntu.com/download/desktop)**  
  _Ubuntu is the core operating system, particularly suited for web archiving tools that work best in Linux environments._

- **[BitCurator](https://bitcurator.net/)**  
  _BitCurator is installed on top of a fresh Ubuntu installation. It includes numerous tools tailored for digital archiving._

- **[Citrix Workspace](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)**  
  _Used to connect to the ICAEW VDI via [MKPortal](https://www.mkportal.icaew.com) or [LonPortal](https://www.lonportal.icaew.com/). You may need to use an older version of the app. 23.3.0.32 is tested as working._

- **[Python](https://www.python.org/downloads/)**  
  _Python is pre-installed with BitCurator. However, virtual environments can be installed using:_
  ```bash
  sudo apt install python3.10-venv
  ```
  _Create a virtual environment:_
  ```bash
  python3 -m venv venv
  ```
  _Activate the virtual environment:_
  ```bash
  . ./venv/bin/activate
  ```

- **[Git](https://git-scm.com/download/linux)**  
  _Pre-installed with BitCurator. Git is essential for version control. Set up Git with your name and email:_
  ```bash
  git config --global user.name "FIRST_NAME LAST_NAME"
  git config --global user.email "MY_NAME@example.com"
  ```

- **[Visual Studio Code (VS Code)](https://code.visualstudio.com/download)**  
  _A code editor used for scripting and GitHub integration. Recommended plugins include:_
    - Python
    - Prettier
    - Markdown All in One


- **[Docker](https://docs.docker.com/engine/install/ubuntu/)**  
  _Pre-installed with BitCurator. Docker is used to run containers, including [browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)._

- **[Google Chrome or Chromium](https://www.google.com/chrome/)**  
  _Required for use with [browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)._


- **[AWS CLI](https://aws.amazon.com/cli/)**  
  _Used for ingesting large files into Preservica._


  - **[Node Version Manager (nvm)](https://github.com/nvm-sh/nvm)** / **Node.js**  
  _NVM allows quick installation and management of Node.js versions via the command line. Install NVM and the latest Node version using:_
  ```bash
  nvm install node
  ```

- **[yarn](https://github.com/yarnpkg/berry)**  
  _A modern package manager used for building/testing `browsertrix-behaviors`._
  ```bash
  npm install -g yarn
  yarn install
  ```
  _To solve compilation errors for `behaviors.js`:_
  ```bash
  export NODE_OPTIONS=--openssl-legacy-provider
  ```

- **[Puppeteer](https://pptr.dev/)**  
  _Install Puppeteer via:_
  ```bash
  npm i puppeteer
  ```

- **[Get cookies.txt LOCALLY (browser extension)](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)**  
  _Exports cookies in Netscape or JSON format._

- **[Wget](https://www.gnu.org/software/wget/)**  
  _Pre-installed with BitCurator. Wget is used to retrieve files via HTTP, HTTPS, FTP, and FTPS._

- **[Thunar](https://docs.xfce.org/xfce/thunar/start)**  
  _A bulk file rename utility. Install with:_
  ```bash
  sudo apt install thunar
  ```

## Archiving-Specific Software

The following tools are specialized for digital archiving:

- **[archiveweb.page](https://github.com/webrecorder/archiveweb.page)**  
  _A high-fidelity web archiving extension for Chrome/Chromium, installed via AppImage._

- **[replayweb.page](https://github.com/webrecorder/replayweb.page)**  
  _Serverless web archive replay directly in the browser, installed via AppImage._

- **[browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)**  
  _A high-fidelity browser-based crawler running in Docker. Install the Docker image via:_
  ```bash
  docker pull webrecorder/browsertrix-crawler
  ```

- **[OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF)**  
  _Adds OCR text layers to scanned PDFs, enabling text searchability._

- **[ExifTool](https://github.com/exiftool/exiftool)**  
  _A command-line tool for reading and writing metadata in various file formats._

- **[Heritrix](https://github.com/internetarchive/heritrix3)**  
  _An open-source web-scale crawler for web archiving, developed by the Internet Archive._

- **[youtube-dl](https://github.com/ytdl-org/youtube-dl)**  
  _A command-line program to download videos from YouTube and other websites._
  ```bash
  pip install youtube-dl
  ```

- **[py-wacz](https://github.com/webrecorder/py-wacz)**  
  _A Python module for working with web archive data in WACZ format._

- **[py-wasapi-client](https://github.com/unt-libraries/py-wasapi-client)**  
  _A client for downloading WARC files from Archive-It and Webrecorder's WASAPI Data Transfer API._

- **[twarc](https://github.com/DocNow/twarc)**  
  _Not in current use. A tool for collecting and archiving Twitter data._
  ```bash
  pip install twarc
  ```

- **[snscrape](https://github.com/JustAnotherArchivist/snscrape)**  
  _Not in current use. A scraper for social networking services like Twitter._
  ```bash
  pip install snscrape
  ```

- **[Brozzler](https://github.com/internetarchive/brozzler)**  
  _Not in current use. A distributed browser-based web crawler._

## ICAEW Digital Archive Handbook-Specific Software

This handbook is produced using the following tools:

- **[MkDocs](https://www.mkdocs.org)**  
  _A static site generator tailored for building project documentation._
  ```bash
  pip install mkdocs
  ```

- **[Material](https://squidfunk.github.io/mkdocs-material/)**  
  _Material theme for MkDocs, enhancing the visual presentation of documentation._
  ```bash
  pip install mkdocs-material
  ```
