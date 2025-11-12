# Downloading StreamAMG Videos

> **Purpose:** This guide provides instructions for downloading videos from StreamAMG and exporting metadata in bulk.

## Prerequisites

**Required:**
- Access to KMC Management Console
- StreamAMG API credentials
- Python 3.x environment
- Required Python packages installed

## Access Requirements

**Required:**
- Valid StreamAMG account with appropriate permissions
- API access credentials (secret and partnerId)
- Sufficient storage space for video downloads

## Download Process

### 1. KMC Management Console Setup

**Steps:**
1. Log in to [KMC Management Console](https://kmc.mp.streamamg.com/login)
2. Navigate to Integration Settings
3. Note down your secret and partnerId

### 2. Video Download

**Steps:**
1. Access video library in KMC
2. Select videos for download
3. Use "MORE ACTIONS" → "Download"
4. Choose "Source" format
5. Save download link when received

### 3. Metadata Export

> **Purpose:** The process uses the [xml_to_csv.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/xml_to_csv.py) script to process metadata.

**Steps:**

1. **Obtain API Session:**
   - Use [API Test Console](https://mp.streamamg.com/api_v3/testme/index.php)
   - Select "session" service
   - Choose "start" action
   - Enter credentials
   - Set type to "ADMIN"
   - Generate KS string

2. **Retrieve Metadata:**
   - Select "media" service
   - Choose "list" action
   - Set pageSize to 500 (maximum)
   - Increment pageIndex for multiple pages
   - Save XML output

3. **Process Metadata:**
```bash
python3 xml_to_csv.py --xml_file input.xml --csv_file output.csv
```

## Quality Settings

> **Best Practices:**
> - Download original source files
> - Preserve original metadata
> - Maintain video quality
> - Include all associated files

## Error Handling
Common issues and solutions:
1. API Connection:
   - Verify credentials
   - Check network connection
   - Validate KS string

2. Download Issues:
   - Check storage space
   - Verify file permissions
   - Monitor download progress

3. Metadata Processing:
   - Validate XML structure
   - Check CSV output
   - Verify field mapping

## Best Practices
1. Always download source quality
2. Keep original filenames
3. Maintain metadata integrity
4. Document any issues
5. Test with single video first
6. Verify downloads before processing

## Support
For issues or questions, contact the Digital Archive team.

# StreamAMG Video Downloads and Metadata Export Guide

> **Purpose:** This guide provides clear, step-by-step instructions on how to download videos from StreamAMG and efficiently export metadata in bulk using StreamAMG's API and the [xml-to-csv.py script](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/xml_to_csv.py).

### Key Resources

Below are key links referenced throughout this guide:

- [KMC Management Console](https://kmc.mp.streamamg.com/login)
- [API Test Console](https://mp.streamamg.com/api_v3/testme/index.php)
- [API Documentation](https://mp.streamamg.com/api_v3/testmeDoc/index.php)

## Part 1: Downloading Videos from StreamAMG

Follow the steps below to download videos from StreamAMG:

### 1. Log in to the KMC Management Console

- Access the [KMC Management Console](https://kmc.mp.streamamg.com/login) and log in with your credentials.

### 2. Navigate to the Video Library

- Once logged in, go to the video library and select the videos you wish to download.

### 3. Initiate the Download

- With your videos selected, click on the "MORE ACTIONS" dropdown menu and choose the "Download" option.

![kmc-1](../../assets/images/kmc-1.png)

### 4. Choose the Download Format

- In the download options, select "Source" to download the original video file.

![kmc-2](../../assets/images/kmc-2.png)

### 5. Complete the Download

- You will receive an email with a download link. Right-click on the link and select "Save link as..." to save the file to your local device.

![kmc-3](../../assets/images/kmc-3.png)

## Part 2: Exporting StreamAMG Metadata

> **Tip:** For best results, use Firefox for this process. The XML output might not render correctly in Chrome.

This process involves using the [API Test Console](https://mp.streamamg.com/api_v3/testme/index.php) and the [xml-to-csv.py script](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/xml_to_csv.py).

### 1. Obtain a "KS" String

- Open the [API Test Console](https://mp.streamamg.com/api_v3/testme/index.php).
- From the "Select service" dropdown, choose "session".
- From the "Select action" dropdown, choose "start".
- Enter your "secret" and "partnerId" (these can be found in the Integration Settings in the [KMC Management Console](https://kmc.mp.streamamg.com/login)).
- Set the "type (KalturaSessionType)" to "ADMIN".
- Click "Submit" to generate a "KS" string, which will be used in the following steps.

![api-1](../../assets/images/api-1.png)

### 2. Retrieve Metadata

- In the same API Test Console session, select "media" from the "Select service" dropdown and "list" from the "Select action" dropdown.
- Tick and edit the "pager (KalturaFilterPager)".
- Enter "500" in the "pageSize" (this is the maximum allowed).
- Enter "1" in the "pageIndex" for the first page of results. For subsequent pages, increment this number (e.g., 2, 3, 4).

![api-2](../../assets/images/api-2.png)

### 3. Save the Metadata XML

- The results will appear as an XML file. Right-click within the frame containing the XML, select "This Frame," and then choose "Save Frame As..." to save the XML file.
- Repeat this process for each subsequent page of metadata.

![api-3](../../assets/images/api-3.png)

### 4. Convert XML to CSV

- Once you have saved the XML files, use the [xml-to-csv.py script](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/xml_to_csv.py) to convert them to CSV format.
- The script requires specifying an `xml_file` as input and a `csv_file` as output.
