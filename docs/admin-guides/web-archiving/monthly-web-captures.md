# Monthly Web Captures

The following three homepages are required to be captured on the second Monday of each month:

* Audit and Beyond <https://www.icaew.com/technical/audit-and-assurance/faculty/audit-and-beyond>
* By All Accounts <https://www.icaew.com/technical/corporate-reporting/corporate-reporting-faculty/by-all-accounts>
* Taxline <https://www.icaew.com/technical/tax/tax-faculty/taxline>

## Tools

For the monthly web captures we use [ArchiveWeb.page](https://github.com/webrecorder/archiveweb.page).

## Process

1. Open the ArchiveWeb.page.

2. Create a new Archive, name Archive in line with the warc naming conventions
    > example: audit-and-beyond-19951012-000-ICAEW
    > add the additional GMT data when ingesting the crawl in Preservica.

3. Start crawl from the top three homepages. Start the crawl in preview mode.

4. Log in to the ICAEW website using your work details.

5. After logging in start the crawl.

6. During crawl manually, click through each tab on the webpage.

7. When crawl has finished and all URL's collected, download the crawl.

8. Ingest crawl into Preservica:
    > rename the WACZ file to include the GMT time. Example: audit-and-beyond-19951012111005-000-ICAEW
    > ingest the files into the following folders:
        
    - Audit and Beyond: Admin/Private Repository/Web Captures/WACZ/WARC Files/audit-and-beyond-logged-in
    - By All Accounts: Admin/Private Repository/Web Captures/WACZ/WARC Files/by-all-accounts-logged-in
    - Taxline: Admin/Private Repository/Web Captures/WACZ/WARC Files/taxline-logged-in

## End of Year Series

At the end of year each seperate months WACZ file will be combined into an annual series for these webpages. 

