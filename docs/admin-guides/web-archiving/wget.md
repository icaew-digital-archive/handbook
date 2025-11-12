# Wget Crawl

## Overview

> **Purpose:** This document outlines the process of using wget to create a secondary/back-up crawl of ICAEW.com and its subdomains.

[Wget](https://en.wikipedia.org/wiki/Wget) is a free utility for non-interactive downloading of files from the web, supporting HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.

## Prerequisites

**Required:**
- [Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc) browser extension
- Access to ICAEW.com with appropriate credentials
- Wget installed on your system

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

## Crawl Configurations

The following crawls can be run simultaneously:

### 1. ICAEW.com

> **Note:** `urls.txt` should be a list of URLs, which will almost always be a .txt version of the sitemap.

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
     -i urls.txt 2>&1 | tee icaew-careers-icaew-wget.log
```

### 2. Regulation.icaew.com
```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --recursive \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     --domains regulation.icaew.com \
     --no-parent \
     --regex-type pcre \
     --reject-regex '(?i)(.*log(?:off|out).*)' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     regulation.icaew.com 2>&1 | tee regulation-icaew-wget.log
```

### 3. Train.icaew.com (Blog Pages Only)
```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --recursive \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     --domains train.icaew.com \
     --no-parent \
     --regex-type pcre \
     --reject-regex '^(?!https:\/\/train\.icaew\.com\/?(blog\/?.*|article\/?.*|$)).+$' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     train.icaew.com 2>&1 | tee train-icaew-wget.log
```

### 4. Volunteer.icaew.com (Blog Pages Only)
```bash
wget --load-cookies cookies.txt \
     --keep-session-cookies \
     --recursive \
     --page-requisites \
     --adjust-extension \
     --span-hosts \
     --convert-links \
     --restrict-file-names=windows \
     --domains volunteer.icaew.com \
     --no-parent \
     --regex-type pcre \
     --reject-regex '^(?!https:\/\/volunteer\.icaew\.com\/?(blog\/?.*|article\/?.*|$)).+$' \
     --random-wait \
     --retry-connrefused \
     --waitretry=10 \
     --tries=3 \
     --timeout=15 \
     volunteer.icaew.com 2>&1 | tee volunteer-icaew-wget.log
```

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

## Appendix

### Previous Configuration

> **Note:** This was an earlier configuration that crawled icaew.com recursively. It was found to take too long to finish and resulted in large capture sizes, as it also crawled media library items which are saved separately in the crawl process.

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
     --reject-regex '(.*(l|L)(o|O)(g|G)((o|O)(f|F)(f|F)|(o|O)(u|U)(t|T)).*)|(.*membership\/active-members.*)|(^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com\/?(?:(?!blog(\/)?).)+$)' \
     --exclude-domains access.icaew.com,annualreturns.icaew.com,apps.icaew.com,dataprotection.icaew.com,demo.icaew.com,ebookshop.icaew.com,ebm.icaew.com,economia.icaew.com,elearning.icaew.com,events.icaew.com,examresults.icaew.com,fdw.icaew.com,find.icaew.com,ion.icaew.com,jobs.icaew.com,latest.icaew.com,learningshop.icaew.com,libcat.icaew.com,membersearch.icaew.com,my.icaew.com,recruit.icaew.com,review.icaew.com,students.icaew.com,uatapps.icaew.com,uatcdn.icaew.com,uatcloudcdn.icaew.com,uatevents.icaew.com,uatmy.icaew.com,vacancies.icaew.com,www.ion.icaew.com \
     --random-wait \
     -i '/mnt/fc6d53c3-3dad-406c-a8ff-222b171f9208/wget-icaew-com-logged-in-july-2022/202XXXXX_sitemap.txt
```
