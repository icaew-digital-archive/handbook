# Monthly Web Captures

## Overview

> **Purpose:** This document outlines the process for capturing three specific ICAEW webpages on a monthly basis using ArchiveWeb.page.

## Target Pages

> **Schedule:** The following three homepages are required to be captured on the second Monday of each month:

1. **Audit and Beyond**
   - URL: <https://www.icaew.com/technical/audit-and-assurance/faculty/audit-and-beyond>

2. **By All Accounts**
   - URL: <https://www.icaew.com/technical/corporate-reporting/corporate-reporting-faculty/by-all-accounts>

3. **Taxline**
   - URL: <https://www.icaew.com/technical/tax/tax-faculty/taxline>

## Tools

> **Tool:** For the monthly web captures, we use [ArchiveWeb.page](https://github.com/webrecorder/archiveweb.page), a web archiving tool that allows for interactive capture of web content.

## Capture Process

### 1. Initial Setup
1. Open ArchiveWeb.page
2. Create a new Archive
3. Name the Archive following the WARC naming convention:
   ```
   audit-and-beyond-19951012-000-ICAEW
   ```
   > **Note:** Additional GMT data will be added during Preservica ingestion

### 2. Crawl Execution
1. Start the crawl from the three target homepages
2. Begin in preview mode
3. Log in to the ICAEW website using your work credentials
4. After successful login, start the crawl
5. During the crawl:
   - Manually click through each tab on the webpage
   - Ensure all interactive elements are captured
6. Wait for the crawl to complete and all URLs to be collected
7. Download the completed crawl

### 3. Preservica Ingestion
1. Rename the WACZ file to include GMT time:
   ```
   audit-and-beyond-19951012111005-000-ICAEW
   ```
2. Ingest the files into the following Preservica folders:
   - **Audit and Beyond**: 
     ```
     Admin/Private Repository/Web Captures/WACZ/WARC Files/audit-and-beyond-logged-in
     ```
   - **By All Accounts**: 
     ```
     Admin/Private Repository/Web Captures/WACZ/WARC Files/by-all-accounts-logged-in
     ```
   - **Taxline**: 
     ```
     Admin/Private Repository/Web Captures/WACZ/WARC Files/taxline-logged-in
     ```

## Annual Series Creation

> **Note:** At the end of each year, the monthly WACZ files for each webpage will be combined into an annual series, creating a comprehensive archive of the year's content. 

