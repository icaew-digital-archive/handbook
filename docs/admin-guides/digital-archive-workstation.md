Here you will find an overview of the software currently installed on our digital archiving workstation.

Currently the workstation is setup as dual-boot machine, running Windows and Ubuntu/BitCurator. Ubuntu/BitCurator is the primary OS. F12 to switch between the OS at boot.

Login details for the Windows and Ubuntu/BitCurator can be found on the [Logins page](../logins.md)

## Installation of Ubuntu/BitCurator on a new laptop

1. Download Ubuntu image from [https://ubuntu.com/tutorials/install-ubuntu-desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop).
2. Create a bootable USB stick. Ubuntu already comes with software called Startup Disk Creator, however [balenaEtcher](https://www.balena.io/etcher/) is a good cross-platform option.
3. Reboot lap and press F12 at boot to select the boot device. Select the USB stick.
4. Follow installation process. Selectr "Install Ubuntu alongside Windows Boot Manager".
5. If BitLocker is enabled, you may need to boot into Windows to disable it. This may require turning it on and off ([https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338](  https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338)).
6. Install BitCurator using ther [Saltstack repository](https://github.com/bitcurator/bitcurator-salt).

## General software

* [Ubuntu](https://ubuntu.com/download/desktop)  
_Most web archiving tools have been developed for and/or work best with Linux._  

* [BitCurator](https://bitcurator.net/)  
_Installed on top of a fresh Ubuntu installation - includes many tools specific to digital archiving._  

* [Citrix Workspace](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)  
_For connecting to the ICAEW VDI via [https://www.mkportal.icaew.com](https://www.mkportal.icaew.com) or [https://www.lonportal.icaew.com/](https://www.lonportal.icaew.com/)_

* [Python](https://www.python.org/downloads/)  
_Comes installed with BitCurator. However, virtual environments may need to be installed via:_

        sudo apt install python3.10-venv  
        
    _The creation of a virtual environment is via:_ 
    
        python3 -m venv venv  
        
    _Activating the virtual environment is via:_  
        
        . ./venv/bin/activate

* [Node Version Manager](https://github.com/nvm-sh/nvm)  / Node.js  
_nvm allows you to quickly install and use different versions of node via the command line. Node.js is an open-source, cross-platform JavaScript runtime environment. Installation instructions are at the Github repository. Install nvm by visiting the github page and using the install script. Install latest node and npm via:_

        nvm install node

* [yarn](https://github.com/yarnpkg/berry)  
_Yarn is a modern package manager split into various packages. Used to build/test browsertrix-behaviors._

        npm install -g yarn
        yarn install  
Errors when compiling behaviors.js is solved using:

        export NODE_OPTIONS=--openssl-legacy-provide


* [Puppeteer](https://pptr.dev/)  
_Installed via:_  

        npm i puppeteer

* [Git](https://git-scm.com/download/linux)  
_Comes installed with BitCurator. Software version control. Used for pulling and pushing [https://github.com/icaew-digital-archive](https://github.com/icaew-digital-archive) and the private repository of this handbook_    
_Installed via:_  

        sudo apt-get install git   
_Setup name and email address in the config via:_  

        git config --global user.name "FIRST_NAME LAST_NAME"

        git config --global user.email "MY_NAME@example.com"


* [Docker](https://docs.docker.com/engine/install/ubuntu/)  
_Comes installed with BitCurator. For running Docker containers. Used to run [browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)_  

* [Chrome or Chromium browser](https://www.google.com/chrome/?brand=CHBD&brand=CHBD&gclid=EAIaIQobChMIyv6JoOqP-wIVGe3tCh2ROw8WEAAYASABEgLTovD_BwE&gclsrc=aw.ds)  
_For use with [browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)._

* [VS Code](https://code.visualstudio.com/download)  
_A text editor for writing code, running scripts, and pushing and pulling to Github._  
_Useful plugins:_
    * _Python_
    * _Prettier_
    * _Markdown All in One_

* [AWS CLI](https://aws.amazon.com/cli/)  
_For ingesting large files into Preservica._  

* [Get cookies.txt LOCALLY browser extension](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)  
_This extension exports cookies in Netscape or JSON format._

* [Wget](https://www.gnu.org/software/wget/)  
_Comes installed with BitCurator. A free software package for retrieving files using HTTP, HTTPS, FTP and FTPS, the most widely used Internet protocols._

* [Thunar](https://docs.xfce.org/xfce/thunar/start)  
_An easy to use bulk rename utility._

        sudo apt install thunar


## Archiving specific software

* [archiveweb.page](https://github.com/webrecorder/archiveweb.page)  
_AA High-Fidelity Web Archiving Extension for Chrome and Chromium based browsers! Installed via AppImage on the releases page._  

* [replayweb.page](https://github.com/webrecorder/replayweb.page)  
_Serverless Web Archive Replay directly in the browser. Installed via AppImage on the releases page._

* [browsertrix-crawler](https://github.com/webrecorder/browsertrix-crawler)  
_A high-fidelity browser-based crawler that runs in a single Docker container_  
_Install/pull the docker image via:_  

        docker pull webrecorder/browsertrix-crawler

* [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF)  
_OCR software. OCRmyPDF adds an OCR text layer to scanned PDF files, allowing them to be searched._

* [ExifTool](https://github.com/exiftool/exiftool)  
_ExifTool is a customizable set of Perl modules plus a full-featured command-line application for reading and writing meta information in a wide variety of files._

* [Heritrix](https://github.com/internetarchive/heritrix3)  
_Heritrix is the Internet Archive's open-source, extensible, web-scale, archival-quality web crawler project._

* [youtube-dl](https://github.com/ytdl-org/youtube-dl)  
_youtube-dl is a command-line program to download videos from YouTube.com and a few more sites._
_Installed via:_

        pip install youtube-dl

* [py-wacz](https://github.com/webrecorder/py-wacz)  
_py-wacz is a Python module and command line utility for working with web archive data using the WACZ format specification._


* [py-wasapi-client](https://github.com/unt-libraries/py-wasapi-client)  
_A client for the Archive-It And Webrecorder WASAPI Data Transfer API. Used to download WARC files from Archive-It._

* [twarc](https://github.com/DocNow/twarc)  
_Not currently in-use. twarc is a command line tool and Python library for collecting and archiving Twitter JSON data via the Twitter API._
_Installed via:_

        pip install twarc

* [snscrape](https://github.com/JustAnotherArchivist/snscrape)  
_Not currently in-use. snscrape is a scraper for social networking services (SNS). It scrapes things like user profiles, hashtags, or searches and returns the discovered items, e.g. the relevant posts._
_Installed via:_

        pip install snscrape

* [Brozzler](https://github.com/internetarchive/brozzler)  
_Not currently in-use. A distributed browser-based web crawler._


## ICAEW Digital Archive Handbook specific software

This ICAEW Digital Archive Handbook is produced using:

* [MkDocs](https://www.mkdocs.org)  
_A static site generator that's geared towards building project documentation_

* [Material](https://squidfunk.github.io/mkdocs-material/)  
_Material theme for MkDocs_
