# YouTube-dl / yt-dlp

## Overview

> **Purpose:** Downloading videos from YouTube or other sites is done using [youtube-dl](https://github.com/ytdl-org/youtube-dl).

> **Note:** From the youtube-dl GitHub repository: _"youtube-dl is a command-line program to download videos from YouTube.com and a few more sites. It requires the Python interpreter, version 2.6, 2.7, or 3.2+, and it is not platform specific. It should work on your Unix box, on Windows or on macOS. It is released to the public domain, which means you can modify it, redistribute it or use it however you like"._

## Installation

> **Tip:** Installation is via pip (a virtual environment is recommended):

**Command:**
```bash
pip install youtube-dl
```

## Configuration

> **Purpose:** The following configuration works best for ICAEW needs:

**Configuration features:**
- Outputs JSON metadata files which can be converted to .metadata files via Python script
- `restrict-filenames` - "Restrict[s] filenames to only ASCII characters, and avoid "&" and spaces in filenames"
- Always saves the downloaded YouTube videos as mp4 format
- Saves the files with `%(upload_date)s-%(title)s` format
- The write-annotations, write-description, write-all-thumbnails, and sub parameters may not be necessary, but can be downloaded for completion sake

**Command:**
```bash
youtube-dl --write-annotations --write-description --write-info-json --write-all-thumbnails --all-subs --write-auto-sub --write-sub --restrict-filenames --format 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best' --output '%(upload_date)s-%(title)s' [YouTube Channel URL]
```

> **Note:** The YouTube channel URL is provided as the last argument. The files will be downloaded to a folder where youtube-dl was run from. The most important files are the videos themselves and the json files which contain all metadata relating to the videos. The files will still need to be manually renamed, i.e., removal of illegal characters, such as outlined here: [https://www.mtu.edu/umc/services/websites/writing/characters-avoid](https://www.mtu.edu/umc/services/websites/writing/characters-avoid)

## Basic Commands

> **Reference:** Parameters can be found at [https://github.com/ytdl-org/youtube-dl#options](https://github.com/ytdl-org/youtube-dl#options).

## Advanced Options

> **Note:** Advanced options section to be added.

## Best Practices

> **Note:** Best practices section to be added.

## Appendices

### Appendix A: Python script
    """
    Reads JSON files from youtube-dl and produce .metadata files based on data 
    contained.
    """
    import json
    import os
    from datetime import datetime

    # Directory where the .info.json files are
    directory = r'/media/kingston/work/youtube-dl/ICAEW-youtube'
    for filename in os.listdir(directory):
        # Filter by file extension
        if os.path.splitext(filename)[1] == '.json':
            # Open JSON file
            with open(os.path.join(directory, filename)) as json_file:
                data = json.load(json_file)
            # Get data from JSON; strip whitespace and replace \n and & 
            title = data['title'].replace('&','and').strip()
            description = data['description'].replace('\n',' ').replace('&','and').strip()
            uploader = data['uploader'].replace('&','and').strip()
            upload_date = data['upload_date']
            # Convert to appropriate format
            upload_date = datetime.strptime(data['upload_date'], '%Y%m%d').strftime('%Y-%m-%d')

            # DC template
            template_dc = (f"""<?xml version="1.0" encoding="UTF-8"?>
            <oai_dc:dc xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/oai_dc/ oai_dc.xsd" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <dc:title>{title}</dc:title>
                <dc:creator>ICAEW</dc:creator>
                <dc:subject></dc:subject>
                <dc:description>{description}</dc:description>
                <dc:publisher>{uploader} YouTube Channel</dc:publisher>
                <dc:contributor></dc:contributor>
                <dc:date>{upload_date}</dc:date>
                <dc:type>MovingImage</dc:type>
                <dc:identifier></dc:identifier>
                <dc:source></dc:source>
                <dc:language></dc:language>
                <dc:relation></dc:relation>
                <dc:coverage></dc:coverage>
                <dc:rights></dc:rights>
            </oai_dc:dc>""")

            # Write template to .metadata files
            with open(os.path.join(directory, filename).replace('.info.json', '.mp4.metadata'), 'w') as f:
                f.write(template_dc)

### Appendix B: Bash scripts for filtering by video duration
Output duration of all videos in folder
        
        for f in *.mp4
        do
        dur=`ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$f"`
        durint=${dur%.*}
        echo -n "$dur\t"
        echo "$f"
        done

Filter out videos less than 90 seconds long and move to new folder

        mkdir "Sub 90 second videos"
        for f in *.mp4
        do
        dur=`ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$f"`
        durint=${dur%.*}
        if [ $durint -lt 90 ]
        then
            echo -n "$dur\t"
            echo "$f"
            mv $f "./Sub 90 second videos/$f"
        fi
        done
