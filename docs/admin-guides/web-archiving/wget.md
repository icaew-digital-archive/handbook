# Wget Crawl

## Overview

> **Purpose:** This document outlines the process of using wget to create a secondary/back-up crawl of ICAEW.com and its subdomains.

[Wget](https://en.wikipedia.org/wiki/Wget) is a free utility for non-interactive downloading of files from the web, supporting HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.

> **Note:** Wget crawls serve as a secondary/back-up capture method. The primary crawl should be performed using [Browsertrix-crawler](browsertrix.md), which creates the main WACZ file for ingestion into Preservica. For the complete web archiving workflow, see the [Web Archiving Process Overview](crawl-process-overview.md).

## Prerequisites

**Required:**
- [Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc) browser extension
- Access to ICAEW.com with appropriate credentials
- Wget installed on your system
- Sufficient disk space (crawls can generate several GB of data)
- `seedFile.txt` containing URLs to crawl (typically a .txt version of the sitemap, one URL per line)

## Setup Process

### 1. Obtaining Cookies
1. Install the Get cookies.txt LOCALLY browser extension
2. Log in to ICAEW.com using a browser with the extension installed
3. Export cookies using the extension
4. Rename the exported file to `cookies.txt`
5. Move the file to your working directory

### 2. Testing Authentication

> **Tip:** Before starting the full crawl, verify that the cookies file works by testing with a known restricted page:

```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     https://www.icaew.com/technical/technology/excel-community/excel-community-articles/2023/dont-expect-too-much-from-ai-after-all-its-not-only-human
```

> **Verification:** After the crawl has finished, navigate to the appropriate HTML file and ensure that it has remained logged in.

---

## Crawl Configurations

### Key Wget Flags Explained

Before running crawls, it's helpful to understand some key flags:

- `--recursive`: Follow links and crawl pages recursively (not used in non-recursive crawls)
- `--page-requisites`: Download all files needed to display HTML pages (images, CSS, JavaScript)
- `--span-hosts`: Allow downloading from different hosts (needed for CDN resources)
- `--adjust-extension`: Add appropriate file extensions based on content type
- `--convert-links`: Convert links in downloaded files to point to local files
- `--restrict-file-names=windows`: Ensure filenames are Windows-compatible
- `--no-parent`: Don't ascend to parent directories when using `--recursive`
- `--domains`: Restrict crawling to specified domains only
- `--reject-regex`: Exclude URLs matching the regex pattern

---

### Directory Structure

Wget will create directories based on the domain structure. For example:
- `www.icaew.com/` - Main site content
- `regulation.icaew.com/` - Regulation subdomain content
- Log files will be saved in the current working directory

---

## Crawl Configurations

The following crawls can be run simultaneously:

### 1. ICAEW.com

> **Note:** `seedFile.txt` should be a list of URLs, which will almost always be a .txt version of the sitemap.

> **Important:** This crawl configuration does not use `--recursive`, meaning it will only crawl the URLs explicitly listed in `seedFile.txt` and will not follow links. Media items (images, videos, etc.) are not included in the sitemap and therefore will not be crawled. If you need to crawl media items, use the [Media Items Crawl](#3-media-items-crawl-optional) configuration instead.

```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --page-requisites \
     --adjust-extension \
     --convert-links \
     --restrict-file-names=windows \
     --regex-type pcre \
     --reject-regex '((?i)(.*log(?:off|out).*))|((?i)(.*membership\/active-members.*))' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     -i seedFile.txt 2>&1 | tee icaew-com-wget.log
```

### 2. Subdomains (Regulation, Train, Volunteer)

> **Note:** This combined crawl covers regulation.icaew.com (full site), train.icaew.com (blog pages only), and volunteer.icaew.com (blog pages only). All three subdomains are crawled recursively from their seed URLs.

```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --recursive \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     --domains regulation.icaew.com,train.icaew.com,volunteer.icaew.com \
     --no-parent \
     --regex-type pcre \
     --reject-regex '((?i)(.*log(?:off|out).*))|(^(?!https:\/\/(train|volunteer)\.icaew\.com\/?(blog\/?.*|article\/?.*|$)).+$)' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     regulation.icaew.com train.icaew.com volunteer.icaew.com 2>&1 | tee subdomains-wget.log
```

> **Note:** The regex pattern restricts train.icaew.com and volunteer.icaew.com to blog/article pages only, while allowing full recursive crawling of regulation.icaew.com.

### 3. Media Items Crawl (Optional)

> **Note:** If you need to crawl media items (images, videos, PDFs, etc.) that are not included in the sitemap, use this domain crawl configuration. This uses `seedFile.txt` as starting points and then recursively crawls from those URLs, following links to capture media library items.

> **Warning:** This crawl can take significantly longer and result in much larger capture sizes compared to the non-recursive seedFile.txt crawl, as it follows all links and downloads all media content.

```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --recursive \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     --domains icaew.com \
     --no-parent \
     --regex-type pcre \
     --reject-regex '((?i)(.*log(?:off|out).*))|((?i)(.*membership\/active-members.*))' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     -i seedFile.txt 2>&1 | tee icaew-media-wget.log
```

> **Note:** This configuration uses `seedFile.txt` as seed URLs and then recursively crawls from those pages, following links to capture media items that are not listed in the sitemap. This ensures you're crawling media linked from the pages you care about, rather than starting from just the homepage.

---

## Post-Crawl Process

1. **Log Analysis with [wget_log_reader.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/web%20crawling/wget_log_reader.py)**
      - The script analyzes wget log files and compares them against the original URL list
      - Generates three CSV files with timestamps:
        - `matching_urls_[timestamp].csv`: URLs successfully crawled with 200 status
        - `missing_urls_[timestamp].csv`: URLs not found in the archives
        - `non_200_urls_[timestamp].csv`: URLs with non-200 status codes

        Usage:
        ```bash
        python wget_log_reader.py <log_file_path> <url_file_path>
        ```

2. **Verification Steps**
   
   - Review the generated CSV files
   - Investigate missing URLs and non-200 status codes
   - Check redirect chains for any unexpected behavior
   - If needed, re-run the crawl but for the missing URLs only.

3. **Zipping and Ingest**
   
   - Add all of the folders to a parent folder called: `202XXXXX-ICAEW-com-logged-in-wget` and compress this into a zip file.
   - This file is to be ingested into Preservica along with the other elements of the capture.
