# Web crawl process overview

This page outlines how to create a full archival capture of ICAEW.com. However, the methods and techniques will be applicable for archiving other websites.

Generally the workflow will be as follows:

```mermaid
flowchart LR
    id1[Pre-crawl work \n\n Request updated site templates \n Media Library download \n Save a copy of the sitemap] --> id2
    id2[Crawl/s \n\n wget crawl \n Main crawl via browsertrix-crawler] --> id3[Post-crawl work \n\n Adding details to the web-crawl-log]
```

## ICAEW.com full capture

A full capture of the ICAEW.com consists of the following:

- A public capture in that is hosted by Archive-It (WARC/Web Archive format). Accessible to the public and staff.

- A restricted capture that has been made locally and has been ingested into the admin area of Preservica (WARC/Web Archive format). Accessible to staff for offline viewing via [https://replayweb.page/](https://replayweb.page/)</a>.

- A restricted capture made using the wget crawler with assets saved in their native formats, i.e., HTML, CSS, JS etc. This crawl is primarily made a backup for the other two crawls. Only available to view offline via the digital archiving workstation.

- A full media library download from Sitecore which has been ingested into the admin area of Preservica. The media library download contains all public/private media content that is hosted on Sitecore at the time of download. Not readily accessible, however, the digital archivist can access the downloads if needed.


## Pre-crawl work

### Request updated site templates

- Request a list of new templates that may have been added to the site since the last crawl. Check for problems that they may present for capture/playback via browsertrix-crawler and ReplayWeb.page
- At this point it might warrant investigating the writing of custom behaviors for the web crawler. This is outlined further on the [browsertrix custom behaviors](./browsertrix-behaviors.md).

### Media Library download

Log into the backend of Sitecore [here](http://master.icaew.com/sitecore/login).

The login credentials can be found at the [Logins page](../../logins.md).

Navigate to the Media Library by selecting it via the bottom toolbar.

![media-lib-1](../../assets/images/media-lib-1.png)

Right click the root level folder, i.e., “Media Library”. Select Scripts and then Download. The process will take quite a while and will result in a ~10 GB zip file that contains the media content that is being stored directly in Sitecore. It will not download items that a linked to from other media content providers such as Vimeo and StreamAMG.

![media-lib-2](../../assets/images/media-lib-2.png)

You should see the following -

![media-lib-3](../../assets/images/media-lib-3.png)

![media-lib-4](../../assets/images/media-lib-4.png)

Once the download is complete, ingest to Admin/Private Repository/Web Captures/Sitecore Media Library Downloads, following the established naming convention for the file.

Once the crawls have completed, upload to Preservica and check the checksums match with `sha1sum [FILE]` in the local machine and then in Preservica by navigating to Advanced and then the Bitsteam page.

The file will need to be transferred out of the VDI via SharePoint to be uploaded to Preservica via the [AWS client](../preservica/aws-cli.md).

### Saving a copy of the ICAEW.com sitemap

A copy of the sitemap is saved for multiple reasons:

- To help the crawls via input to wget and browsertrix-crawler
- For post-crawl testing, to ensure all URLs have been crawled
- For future reference, as a record of what the crawls/WARC files contain

The following script can be used to produce the sitemaps in .txt format: [sitemap_xml_to_txt_or_html.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/sitemap%20tools/sitemap_xml_to_txt_or_html.py).

The script requires the 'requests' library to be install via:

        pip install requests

#### Example sitemap_xml_to_txt_or_html.py usage

The following outputs a list of URLs from the sitemap to a text file with pages that contain "sprint-test-pages" or "active-members" exluded:

        python3 sitemap_xml_to_txt_or_html.py --to_file 202XXXXX_sitemap.txt https://www.icaew.com/sitemap_corporate.xml https://www.icaew.com/sitemap_careers.xml  --exclude_strings "sprint-test-pages" "active-members"

## Crawls

### Wget crawl

Information regarding the use of wget can be found on the [wget](./wget.md) page.

### browsertrix-crawler

Information regarding the setup and use of browsertrix-crawler can be found on the [browsertrix-crawler](./browsertrix.md) page.

Once the crawls have completed, upload to Preservica and check the checksums match with `sha1sum [FILE]` in the local machine and then in Preservica by navigating to Advanced and then the Bitsteam page.

### Public crawls via Archive-It

Ensure that the inclusion/exclusion rules match the browsertrix configuration. Run the crawl for 30 days, using the Standard crawling technology and with a 50 GB data limit. This crawl will not require post-crawl validation work.


## Post-crawl work

### web-crawl-log

Upload the sitemap used for the crawls to Web Crawls/Web Crawl Sitemaps. Fill out the details in web-crawl-log.xlsx in Web Crawls.


## Appendix

### warc-reader.py

The WARC reader script (as found here - [https://github.com/craiglmccarthy/web-archiving-scripts/blob/main/warc-reader.py](https://github.com/craiglmccarthy/web-archiving-scripts/blob/main/warc-reader.py)) is used to check the sitemap against the capture.

The script needs to be edited here:

        # URL_LIST is most likely going to be a sitemap 'snapshot'
        URL_LIST = ''
        # WARC_FOLDER_PATH is a path to a folder containing WARC files
        WARC_FOLDER_PATH = ''
        # CSV_FILENAME is the filename for the CSV output
        CSV_FILENAME = ''

The URL_LIST should point to a .txt file of URLs, i.e. the 202XXXXX_sitemap.txt created earlier in the crawl process. WARC_FOLDER_PATH should point to a folder containing WARC files. CSV_FILENAME is the name of .csv file output.
