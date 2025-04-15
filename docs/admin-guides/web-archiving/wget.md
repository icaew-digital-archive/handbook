## Wget crawl

The following page outlines the process of using wget to create a secondary/back-up crawl.

[Wget](https://en.wikipedia.org/wiki/Wget) being a free utility for non-interactive downloading of files from the web. It supports HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.

### Obtaining a cookies.txt file

- To be used in conjunction with the Get cookies.txt LOCALLY browser extension.

- Login to the ICAEW.com as normally via a browser that has the [Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc) extension installed.
- Once logged in export the cookies via the Get cookies.txt LOCALLY extension, rename the cookies file to cookies.txt and move to the folder where you want the wget download to begin.
- Test that the cookies file works by using the following command. This uses a known logged in/restricted page to test whether wget has a successfully logged in session. This "known" logged-in page will need to be reviewed periodically.

        wget --load-cookies cookies.txt --keep-session-cookies --page-requisites --adjust-extension --span-hosts --convert-links --restrict-file-names=windows https://www.icaew.com/technical/technology/excel-community/excel-community-articles/2023/dont-expect-too-much-from-ai-after-all-its-not-only-human

- Once confirmed to be working perform the following crawls. Only the first crawl containing icaew.com and careers.icaew.com needs a sitemap .txt file.

### ICAEW.com wget crawl configurations

- **icaew.com / careers.icaew.com** (this is a non-recursive crawl and it needs the sitemaps to be supplied via -i):

        wget --load-cookies cookies.txt --keep-session-cookies --page-requisites --adjust-extension --convert-links --restrict-file-names=windows --regex-type pcre --reject-regex '((?i)(.*log(?:off|out).*))|((?i)(.*membership\/active-members.*))' --random-wait --retry-connrefused --waitretry=10 --tries=3 --timeout=15 -i urls.txt 2>&1 | tee icaew-careers-icaew-wget.log

- **regulation.icaew.com** (this is a recursive crawl, the sitemap doesn't exist for this domain):

        wget --load-cookies cookies.txt --keep-session-cookies --recursive --page-requisites --adjust-extension --span-hosts --convert-links --restrict-file-names=windows --domains regulation.icaew.com --no-parent regulation.icaew.com/ --regex-type pcre --reject-regex '(?i)(.*log(?:off|out).*)' --random-wait --retry-connrefused --waitretry=10 --tries=3 --timeout=15 regulation.icaew.com 2>&1 | tee regulation-icaew-wget.log

- **train.icaew.com** (blog pages only):

        wget --load-cookies cookies.txt --keep-session-cookies --recursive --page-requisites --adjust-extension --span-hosts --convert-links --restrict-file-names=windows --domains train.icaew.com --no-parent icaew.com/ --regex-type pcre --reject-regex '^(?!https:\/\/train\.icaew\.com\/?(blog\/?.*|article\/?.*|$)).+$' --random-wait --retry-connrefused --waitretry=10 --tries=3 --timeout=15 train.icaew.com 2>&1 | tee train-icaew-wget.log

- **volunteer.icaew.com** (blog pages only):

        wget --load-cookies cookies.txt --keep-session-cookies --recursive --page-requisites --adjust-extension --span-hosts --convert-links --restrict-file-names=windows --domains volunteer.icaew.com --no-parent icaew.com/ --regex-type pcre --reject-regex '^(?!https:\/\/volunteer\.icaew\.com\/?(blog\/?.*|article\/?.*|$)).+$' --random-wait --retry-connrefused --waitretry=10 --tries=3 --timeout=15 volunteer.icaew.com 2>&1 | tee voluneteer-icaew-wget.log

## Post-crawl

- A script callled [wget_log_reader.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/wget_log_reader.py) can be used to read the log file to find any potentially missing URLs.
- When complete, zip the contents of the crawls into a single file and ingest into Preservica at Admin/Private Repository/Web Captures/Wget Captures using the [AWS client](../preservica/aws-cli.md). Ensure that you include the original seed url .txt file.

## Appendix

- This was an earlier configuration that crawled icaew.com recursively, but it was found to take too long to finish and resulted in large capture sizes, i.e. it also crawled the media library items which are saved at another point of the crawl process.

        wget --load-cookies cookies.txt --keep-session-cookies --recursive --page-requisites --adjust-extension --span-hosts --convert-links --restrict-file-names=windows --domains icaew.com --no-parent icaew.com/ --reject-regex '(.*(l|L)(o|O)(g|G)((o|O)(f|F)(f|F)|(o|O)(u|U)(t|T)).*)|(.*membership\/active-members.*)|(^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com\/?(?:(?!blog(\/)?).)+$)' --exclude-domains access.icaew.com,annualreturns.icaew.com,apps.icaew.com,dataprotection.icaew.com,demo.icaew.com,ebookshop.icaew.com,ebm.icaew.com,economia.icaew.com,elearning.icaew.com,events.icaew.com,examresults.icaew.com,fdw.icaew.com,find.icaew.com,ion.icaew.com,jobs.icaew.com,latest.icaew.com,learningshop.icaew.com,libcat.icaew.com,membersearch.icaew.com,my.icaew.com,recruit.icaew.com,review.icaew.com,students.icaew.com,uatapps.icaew.com,uatcdn.icaew.com,uatcloudcdn.icaew.com,uatevents.icaew.com,uatmy.icaew.com,vacancies.icaew.com,www.ion.icaew.com --random-wait -i '/mnt/fc6d53c3-3dad-406c-a8ff-222b171f9208/wget-icaew-com-logged-in-july-2022/202XXXXX_sitemap.txt
