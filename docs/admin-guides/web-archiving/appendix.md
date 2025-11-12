# Appendix: Advanced Web Archiving Processes

> **Purpose:** This appendix contains detailed documentation for specialized web archiving processes that are performed infrequently. These instructions are maintained for reference purposes.

## Table of Contents

- [Internet Archive Wayback Machine](#internet-archive-wayback-machine)
- [Twitter Archiving](#twitter-archiving)
- [Archive-It WARC Downloads](#archive-it-warc-downloads)
- [WARC File Naming Conventions](#warc-file-naming-conventions)

## Internet Archive Wayback Machine

### Overview

> **Purpose:** Process for downloading content from the Internet Archive Wayback Machine using the CDX Server API. Information and documentation can be found here: [Wayback CDX Server API](https://github.com/internetarchive/wayback/tree/master/wayback-cdx-server)

### Process Details

1. **Obtain Index**

   - The first step is to obtain an index of pages captured by the public IA crawls via the Internet Archive Wayback CDX Server API
   - Example API query: `http://web.archive.org/cdx/search/cdx?url=icaew.com&output=json&matchType=domain&filter=mimetype:application.*`
   - > **Note:** Obtaining the index for a large site can produce a response with many lines and will often crash the browser when queried directly through the browser address bar

2. **Download Index**

   - To overcome browser limitations, use wget to request the index and save to a local json file:

   ```bash
   wget 'http://web.archive.org/cdx/search/cdx?url=icaew.com&matchType=domain&output=json' -O icaew-com-index.json
   ```

   - This request matches all captured URLs from the icaew.com domain (including sub-domains) and outputs to icaew-com-index.json

3. **Process JSON**
   - Use the [ia_cdx_json_to_txt.py](https://github.com/craiglmccarthy/web-archiving-scripts/blob/main/downloading%20items%20from%20internet%20archive/ia_cdx_json_to_txt.py) script to convert the local json file to a list of URLs
   - The resulting URL list can be given to wget or another downloader/crawler

## Twitter Archiving

### Overview

> **Purpose:** Methods for archiving Twitter content, including full account histories and specific time periods.

### Prerequisites

- Twitter account credentials (available on the [Logins page](../../logins.md))
- > **Note:** Regarding Tweet limits (from the [Twitter Help Center](https://help.twitter.com/en/using-twitter/missing-tweets)):
  - Only the 800 most recent Tweets are displayed in the Tweets tab
  - Only the latest 3,200 Tweets are displayed in the Tweets and replies tab

### Available Tools

#### 1. archiveweb.page

- **Description**: The easiest way to capture a Twitter account
- **Options**: Chrome extension or Electron desktop application
- **Features**:
  - Twitter-specific browser behaviours
  - Advanced search capabilities
- **Usage**:
  - For tweets older than 3,200, capture the advanced search results feed
  - Example URL format: `https://twitter.com/search?q=(from%3AICAEW)%20until%3A2009-12-31%20since%3A2009-01-01&src=typed_query&f=live`

#### 2. snscrape

- **Description**: Tool for scraping all tweets (surpassing the 3,200 tweet limit)
- **Output Options**:

  ```bash
  # Get tweet URLs
  snscrape twitter-user ICAEW > ICAEW-tweet-URLs.txt

  # Get full tweet data
  snscrape --jsonl twitter-user ICAEW > ICAEW-tweets.json
  ```

- **Note**: A snapshot of the ICAEW Twitter was taken using this tool on 24/11/2022 which includes the entire history of the @ICAEW account up until the snapshot. The .txt and .json files are currently being held in the admin section of Preservica. The ICAEW-tweet-URLs.txt was supplied to the crawler to ensure that all Tweets have been crawled by a web crawler.

#### 3. twarc

- **Status**: Tested, but not currently in use as does not meet our requirements
- **Limitations**: Only captures latest 3,200 tweets in JSON format
- **Setup**:

  ```bash
  # Create virtual environment (recommended)
  python3 -m venv venv
  . ./venv/bin/activate

  # Install twarc
  pip install twarc

  # Configure API credentials
  twarc2 configure
  ```

- **Note**: A developer account needs to be setup. There is a development account setup using the same credentials as above. The login details are on [Logins page](../../logins.md).

## Archive-It WARC Downloads

### Overview

> **Purpose:** Process for downloading and managing WARC files from Archive-It.

### Installation and Configuration

1. **Install Client**

   ```bash
   pip install py-wasapi-client
   ```

   > **Tip:** Best practice is to install in a virtual environment

2. **Configure Credentials**
   Create `~/.wasapi-client`:
   ```
   [icaew]
   username = craig.mccarthy@icaew.com
   password = PASSWORD
   ```

### Common Commands

```bash
# Get MD5 file manifest of all WARC files on Archive-It
wasapi-client --profile icaew --manifest

# Get number of WARC files available after 2022-01-01
wasapi-client --profile icaew --collection 16306 --log /tmp/out.log --crawl-time-after 2022-01-01 --count

# Download WARC files
wasapi-client --profile icaew --collection 16306 --log /tmp/out.log --crawl-time-after 2022-01-01 -p 1
```

### Backup Process

There are regular crawls on Archive-It that will need to be backed up onto Preservica - at the time of writing the Insights Daily Summary and the public ICAEW.com capture.

1. **Get Checksums**

   - Run `get_asset_checksum_values.py` using the folder reference for "WACZ/WARC Files" (currently: f127c4de-f82f-4d8d-a7f7-d7552cdd4e97)
   - Run `wasapi-client --profile icaew --manifest` to get Archive-It checksums
   - Run `preservica-archive-it-checksum-cross-reference.py` to compare lists

2. **Download Missing Files**
   Use the following bash script:

   ```bash
   #!/bin/bash

   # Path to the text file to loop through
   file_path="/home/digital-archivist/Documents/apps/py-wasapi-client/files-not-present-in-preservica.txt"

   # Loop through each line in the file
   while read line; do

   # Extract the substring from the line (using sed command)
   sub_string=$(echo "$line" | sed 's/.*\(substring\).*/\1/')

   # Use the substring in a shell command (replace with your own command)
   # Replace the above echo statement with your own shell command that uses the sub_string
   echo "Downloading $sub_string"
   wasapi-client --profile icaew --filename $sub_string

   done < "$file_path"
   ```

### Ignored Collections

The following collection IDs can be ignored:

- 16126 (DELETEME-The Business Finance Guide - Local Brozzler)
- 14885 (DELETEME-ION (Public) - OLD)
- 14886 (DELETEME-ION (Excel Community) - OLD)
- 14752 (DELETEME-Insights - Daily Summary (Broken 2))
- 13348 (DELETEME-Insights - Daily Summary (Broken 1))

## WARC File Naming Conventions

### Overview

> **Purpose:** Before ingest into Preservica or uploading to Archive-It, WARC files must follow a specific naming convention to ensure proper identification and organization.

### Naming Format

WARC files should be named following this convention:

```
Prefix-Timestamp-Serial-Crawlhost.warc.gz
```

### Components

- **Prefix**: An abbreviation reflecting the project or crawl that created the file
- **Timestamp**: 14-digit GMT timestamp indicating when the file was initially begun
- **Serial**: Increasing serial number within the process creating the files
- **Crawlhost**: Domain name or IP address of the machine creating the file

### Example

For ICAEW use:

```
Business-finance-guide-20210304135532142-001-ICAEW.warc.gz
```

### Reference

This naming convention follows the recommendations from the [WARC 1.0 specification](https://iipc.github.io/warc-specifications/specifications/warc-format/warc-1.0/#annex-b-informative-warc-file-size-and-name-recommendations).
