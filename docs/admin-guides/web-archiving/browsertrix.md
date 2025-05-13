# Browsertrix-crawler

## Overview
This document covers the process of crawling ICAEW.com using Browsertrix-crawler. While focused on ICAEW.com, the process should be applicable to other websites. The configuration files, Python scripts, and custom JavaScript files can be found in the [digital-archiving-scripts repository](https://github.com/icaew-digital-archive/digital-archiving-scripts/tree/main/web%20crawling/browsertrix-crawler%20files%20and%20scripts).

## Prerequisites
Before starting any crawl, ensure you have:

1. Docker installed and running on your system
2. The latest Browsertrix-crawler image pulled:
   ```bash
   docker pull webrecorder/browsertrix-crawler
   ```
3. Required configuration files and scripts from the digital-archiving-scripts repository
4. Sufficient disk space for the crawl

## Crawl Types
This guide covers three types of crawls:

1. [Template crawl](#template-crawl) - For testing specific page templates
2. [ICAEW.com (logged-in)](#full-icaewcom-crawl-logged-in) - Full site crawl with authentication
3. [ICAEW.com (public)](#full-icaewcom-crawl-public) - Full site crawl without authentication
4. [ICAEW.com patch crawl](#icaewcom-patch-crawl) - 1 hop patch crawl



---

## Template crawl {#template-crawl}

### 1. Creating a Browser Profile

Purpose: This step creates a browser profile with your authentication cookies for the logged-in crawl.

Command:
```bash
sudo docker run -p 6080:6080 -p 9223:9223 \
    -v $PWD/crawls/profiles:/crawls/profiles/ \
    -it webrecorder/browsertrix-crawler \
    create-login-profile --url "https://my.icaew.com/security/Account/Login"
```

Profile Creation Steps:

1. Open Chrome and navigate to [http://localhost:9223/](http://localhost:9223/)
2. Configure browser settings:
   
      - Click "Allow all cookies"
      - Disable Brave shields
  
3. Authentication:
   
      - Login with your credentials
      - Disable Brave shields again (if prompted)
  
4. Clean up:
   
      - Close the "Discover the latest MyICAEW App" banner
  
5. Verification:

      - Navigate to a page with StreamAMG video player (e.g., https://www.icaew.com/for-current-aca-students/training-agreement/your-online-training-file-guide/online-training-file-videos)
      - Verify that the video element loads correctly
  
6. Finalize:

      - Click "Create Profile"

### 2. Setting Up and Starting the Template Crawl

#### Prerequisites
Ensure you have the following files in your working directory:

- `crawl-config.yaml` - Configuration file for the crawl
- `seedFile.txt` - Contains URLs to crawl. This should be the URLs found in the template document.
- `custom-behaviors/icaew-com-behaviors.js` - Custom behavior scripts

Required directory structure:
```bash
$PWD/
├── seedFile.txt
├── crawl-config.yaml
├── custom-behaviors/
│   └── icaew-com-behaviors.js
```

#### Configuration
Example `crawl-config.yaml` for template testing:
```yaml
# For an example of go to Crawling Configuration Options at https://github.com/webrecorder/browsertrix-crawler
# A "template test" crawl; i.e. used for crawling all the types of templates found on ICAEW.com.
# The crawl will only crawl pages defined in seedFile.txt and no others.

# Basic setup
profile: /crawls/profiles/profile.tar.gz
seedFile: /app/seedFile.txt
collection: template-test
screencastPort: 9037
customBehaviors: /custom-behaviors/

# Additional options
allowHashUrls: true
combineWARC: true
generateWACZ: true
workers: 8
text:
  - to-warc
#  - to-pages
screenshot: view
diskUtilization: 0

# Crawl specific options
scopeType: "page-spa"
```

#### Starting the Crawl
Command:
```bash
sudo docker run -p 9037:9037 \
    -v $PWD/crawls:/crawls \
    -v $PWD/custom-behaviors/:/custom-behaviors/ \
    -v $PWD/crawl-config.yaml:/app/crawl-config.yaml \
    -v $PWD/seedFile.txt:/app/seedFile.txt \
    webrecorder/browsertrix-crawler crawl \
    --config /app/crawl-config.yaml
```

Monitor the crawl at [http://localhost:9037](http://localhost:9037/)

### 3. Quality Assurance and Verification

After the crawl completes, perform the following verification steps:

#### 1. Run web_archive_validator.py
- Generates three CSV files:
    - `matching_urls.csv` - Successfully crawled URLs
    - `missing_urls.csv` - URLs that failed to crawl
    - `non_200_urls.csv` - URLs that returned non-200 status codes
- Investigate missing and non-200 URLs. If neccessary, create a new seedFile.txt with problematic URLs. Run a secondary crawl using the same configuration. It may be worth updating the [Sitecore templates and examples](https://icaew.sharepoint.com/:w:/s/digitalarchive/EY-WRGmke3VGmyYkTdEDDYMBfZ5nyLgefubtdJNa4WMfDQ?e=CMGN8D&CID=db808ef5-54c9-52fb-079f-87384e3ca7aa&ovuser=30a7efd6-7f05-437d-bc19-e7c6df3892f1%2CCraig.McCarthy%40icaew.com&clickparams=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiIxNDE1LzI1MDMwMjAxMDEwIiwiSGFzRmVkZXJhdGVkVXNlciI6ZmFsc2V9) document at this point (i.e. updating the URLs to the redirects and removing non-existent pages).


#### 2. QA Crawl
Run a QA crawl to verify the capture:
```bash
sudo docker run -p 9037:9037 \
    -v $PWD/crawls/:/crawls/ \
    -v $PWD/custom-behaviors/:/custom-behaviors/ \
    -it webrecorder/browsertrix-crawler qa \
    --qaSource /crawls/collections/template-test/template-test.wacz \
    --collection template-qa \
    --generateWACZ \
    --profile /crawls/profiles/profile.tar.gz \
    --customBehaviors /custom-behaviors/ \
    --screencastPort 9037 \
    --workers 8
```

Generate a QA report using [extract_qa.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/web%20crawling/extract_qa.py). Use this to futher inspect problematic URLs.

#### 3. Manual Inspection
- Use [replayweb.page](https://github.com/webrecorder/replayweb.page) to inspect the WACZ file
- Verify page rendering and functionality
- Go to the last captured page and ensure that it has remained "logged-in"


---

## Full ICAEW.com crawl (logged-in) {#full-icaewcom-crawl-logged-in}

### 1. Creating a Browser Profile

Purpose: This step creates a browser profile with your authentication cookies for the logged-in crawl.

Command:
```bash
sudo docker run -p 6080:6080 -p 9223:9223 \
    -v $PWD/crawls/profiles:/crawls/profiles/ \
    -it webrecorder/browsertrix-crawler \
    create-login-profile --url "https://my.icaew.com/security/Account/Login"
```

Profile Creation Steps:

1. Open Chrome and navigate to [http://localhost:9223/](http://localhost:9223/)
2. Configure browser settings:
   
      - Click "Allow all cookies"
      - Disable Brave shields
  
3. Authentication:
   
      - Login with your credentials
      - Disable Brave shields again (if prompted)
  
4. Clean up:
   
      - Close the "Discover the latest MyICAEW App" banner
  
5. Verification:

      - Navigate to a page with StreamAMG video player (e.g., https://www.icaew.com/for-current-aca-students/training-agreement/your-online-training-file-guide/online-training-file-videos)
      - Verify that the video element loads correctly
  
6. Finalize:

      - Click "Create Profile"

### 2. Setting Up and Starting the Template Crawl

#### Prerequisites
Ensure you have the following files in your working directory:

- `crawl-config.yaml` - Configuration file for the crawl
- `seedFile.txt` - Contains URLs to crawl. This should be the URLs from the sitemap.
- `custom-behaviors/icaew-com-behaviors.js` - Custom behavior scripts

Required directory structure:
```bash
$PWD/
├── seedFile.txt
├── crawl-config.yaml
├── custom-behaviors/
│   └── icaew-com-behaviors.js
```

#### Configuration
Example `crawl-config.yaml` for a full ICAEW.com crawl.

NOTE: There is a problem with generating combined WARCs and combined WACZs, so this is done at a later stage in the process.
```yaml
# For an example of go to Crawling Configuration Options at https://github.com/webrecorder/browsertrix-crawler
# Example config for a full ICAEW.com crawl. Whether it is a logged-in session is defined within the crawl profile.
# This crawl will read the seedFile.txt and also discover pages within the defined scope.

# Basic setup
profile: /crawls/profiles/profile.tar.gz
seedFile: /app/seedFile.txt
# Collection name should be 'icaew-com-logged-in' or 'icaew-com-public'
collection: icaew-com-logged-in
screencastPort: 9037
customBehaviors: /custom-behaviors/

# Additional options
allowHashUrls: true
# combineWARC: true
# generateWACZ: true
workers: 8
text:
  - to-warc
#  - to-pages
screenshot: view
diskUtilization: 0

# Crawl specific options
scopeType: "custom"
include:
  - ^(http(s)?:\/\/)?(www\.)?(careers\.|cdn\.|regulation\.)?icaew\.com.*$ # scope in icaew.com, careers.icaew.com, cd.icaew.com, and regulation.icaew.com
  - ^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com(\/)?(blog.*)?$ # scope in parent and blog pages only
exclude:
  - ^.*(l|L)(o|O)(g|G)(o|O)(f|F)(f|F).*$ # block logout URLs
  - ^(http(s)?:\/\/)?(www\.)?icaew\.com\/search.*$ # block search pages (robots.txt)
  - ^(http(s)?:\/\/)?(www\.)?.*\/member(s|ship)\/active-members.*$ # block active-members pages and media files
  - ^(http(s)?:\/\/)?(www\.)?.*sprint-test-pages.*$ # block sprint-test-pages
```

#### Starting the Crawl
Command:
```bash
sudo docker run -p 9037:9037 \
    -v $PWD/crawls:/crawls \
    -v $PWD/custom-behaviors/:/custom-behaviors/ \
    -v $PWD/crawl-config.yaml:/app/crawl-config.yaml \
    -v $PWD/seedFile.txt:/app/seedFile.txt \
    webrecorder/browsertrix-crawler crawl \
    --config /app/crawl-config.yaml
```

Monitor the crawl at [http://localhost:9037](http://localhost:9037/)

### 3. Quality Assurance and Verification

After the crawl completes, perform the following verification steps:

#### 1. Run web_archive_validator.py
- Generates three CSV files:
    - `matching_urls.csv` - Successfully crawled URLs
    - `missing_urls.csv` - URLs that failed to crawl
    - `non_200_urls.csv` - URLs that returned non-200 status codes
- Investigate missing and non-200 URLs. If neccessary, create a new seedFile.txt with problematic URLs. Run a secondary crawl using the same configuration. The URLs in the `missing_urls.csv` file is obvious, but also look for 404/500 error URLs in `non_200_urls.csv` too. This secondary crawl should be done via [crawl-config-icaew-com-one-hop.yaml](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/web%20crawling/browsertrix-crawler%20files%20and%20scripts/crawl-config-icaew-com-one-hop.yaml), i.e. the ICAEW.com patch crawl {#icaewcom-patch-crawl}. This crawl config differs in the sense that it will only crawl the URLs from the seedFile.txt + 1 hop (to pickup things such as links to PDFs etc.) but will not discover/crawl the entire site like the primary crawl.

#### 2. Final packaging of WARCs into WACZ
-  Once you are happy with the QA and verification, use [warc_processor.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/web%20crawling/warc_processor.py) to:

      - Process a single or multiple WARC files into WACZ (Web Archive Collection Zipped) format
      - Combine multiple WARC files into a single WARC file
      - The command should look something like this:

```bash
warc_processor.py --input "/media/digital-archivist/Elements/20250426-ICAEW-COM/crawls/collect
ions/icaew-com-logged-in/archive" --output "/media/digital-archivist/Elements/20250426-ICAEW-COM/crawls/collections/icaew-com-logged-in/archive/20250426-ICAEW-com-logged-in.wacz"
```

#### 3. Manual Inspection
- Use [replayweb.page](https://github.com/webrecorder/replayweb.page) to inspect the WACZ file
- Verify page rendering and functionality
- Go to the last captured page and ensure that it has remained "logged-in"


---
## Full ICAEW.com crawl (public) {#full-icaewcom-crawl-public}

The process for a public crawl is similar to the logged-in crawl, with the main difference being the browser profile creation.

### Creating a Browser Profile

Purpose: This step creates a browser profile for the public crawl without authentication.

Command:
```bash
sudo docker run -p 6080:6080 -p 9223:9223 \
    -v $PWD/crawls/profiles:/crawls/profiles/ \
    -it webrecorder/browsertrix-crawler \
    create-login-profile --url "https://www.icaew.com/"
```

Profile Creation Steps:

1. Open Chrome and navigate to [http://localhost:9223/](http://localhost:9223/)
2. Configure browser settings:
   
      - Click "Allow all cookies"
      - Disable Brave shields
  
3. Verification:
   
      - Navigate to a page with StreamAMG video player
      - Verify that the video element loads correctly
  
4. Finalize:

      - Click "Create Profile"

The crawl-config is the same, with the exception of the collection name.


## ICAEW.com patch crawl {#icaewcom-patch-crawl}

This crawl config will only crawl the URLs from the seedFile.txt + 1 hop (to pickup things such as links to PDFs etc.) but will not discover/crawl the entire site like the full ICAEW.com crawls:


```yaml
# For an example of go to Crawling Configuration Options at https://github.com/webrecorder/browsertrix-crawler
# Example config for a full ICAEW.com crawl. Whether it is a logged-in session is defined within the crawl profile.
# This crawl will read the seedFile.txt and only crawl 1 hop from the seed URLs.

# Basic setup
profile: /crawls/profiles/profile.tar.gz
seedFile: /app/seedFile.txt
# Collection name should be 'icaew-com-logged-in' or 'icaew-com-public'
collection: icaew-com-logged-in
screencastPort: 9037
customBehaviors: /custom-behaviors/

# Additional options
allowHashUrls: true
# combineWARC: true
# generateWACZ: true
workers: 8
text:
- to-warc
#  - to-pages
screenshot: view
diskUtilization: 0

# Crawl specific options
depth: 1  # Limit crawl to 1 hop from seed URLs
scopeType: "custom"
include:
- ^(http(s)?:\/\/)?(www\.)?(careers\.|cdn\.|regulation\.)?icaew\.com.*$ # scope in icaew.com, careers.icaew.com, cd.icaew.com, and regulation.icaew.com
- ^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com(\/)?(blog.*)?$ # scope in parent and blog pages only
exclude:
- ^.*(l|L)(o|O)(g|G)(o|O)(f|F)(f|F).*$ # block logout URLs
- ^(http(s)?:\/\/)?(www\.)?icaew\.com\/search.*$ # block search pages (robots.txt)
- ^(http(s)?:\/\/)?(www\.)?.*\/member(s|ship)\/active-members.*$ # block active-members pages and media files
- ^(http(s)?:\/\/)?(www\.)?.*sprint-test-pages.*$ # block sprint-test-pages
```