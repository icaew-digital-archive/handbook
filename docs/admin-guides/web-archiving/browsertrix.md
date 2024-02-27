# Browsertrix-crawler

This document covers the process of crawling ICAEW.com but should also be applicable to other websites.

First ensure that you have the latest crawler via the following command: `docker pull webrecorder/browsertrix-crawler`

## Create a logged-in profile for ICAEW.com

This is done via the following command:

`sudo docker run -p 6080:6080 -p 9223:9223 -v $PWD/crawls/profiles:/crawls/profiles/ -it webrecorder/browsertrix-crawler create-login-profile --url "https://my.icaew.com/security/Account/Login"`

Open [http://localhost:9223/](http://localhost:9223/) (as prompted in the terminal) in Chrome. Login into ICAEW.com as normal. Once done click "Create Profile".

Ensure that **crawl-config.yaml** and **seedFile.txt** exist in present working directory. A folder called **custom-behaviors** should be created where the custom [icaew-com-behaviors.js](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/browsertrix-crawler%20files%20and%20scripts/icaew-com-behaviors.js) can be saved.


and then run the following -

`sudo docker run -p 9037:9037 -v $PWD/crawls:/crawls -v $PWD/custom-behaviors/:/custom-behaviors/ -v $PWD/crawl-config.yaml:/app/crawl-config.yaml -v $PWD/seedFile.txt:/app/seedFile.txt webrecorder/browsertrix-crawler crawl --config /app/crawl-config.yaml`


YAML file example (ICAEW.com crawl):


        # For an example of go to Crawling Configuration Options at https://github.com/webrecorder/browsertrix-crawler
        # Crawl of main ICAEW.com site - including some extra exclusions

        # Basic setup
        profile: /crawls/profiles/profile.tar.gz
        seedFile: /app/seedFile.txt
        collection: icaew-com
        screencastPort: 9037
        customBehaviors: /custom-behaviors/

        # Additional options
        allowHashUrls: true
        combineWARC: true
        generateWACZ: true
        workers: 8

        # Crawl specific options
        scopeType: "custom"
        include:
        - ^(http(s)?:\/\/)?(www\.)?(careers\.|cdn\.|live\.|regulation\.)?icaew\.com.*$ # scope in icaew.com, careers.icaew.com, cd.icaew.com, live.icaew.com, and regulation.icaew.com
        - ^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com(\/)?(blog.*)?$ # scope in parent and blog pages only
        exclude:
        - ^.*(l|L)(o|O)(g|G)(o|O)(f|F)(f|F).*$ # block logout URLs
        - ^(http(s)?:\/\/)?(www\.)?icaew\.com\/search.*$ # block search pages (robots.txt)
        - ^(http(s)?:\/\/)?(www\.)?.*\/member(s|ship)\/active-members.*$ # block active-members pages and media files
        - ^(http(s)?:\/\/)?(www\.)?.*sprint-test-pages.*$ # block sprint-test-pages


YAML file example (single page):

        # For an example of go to Crawling Configuration Options at https://github.com/webrecorder/browsertrix-crawler

        # Basic setup
        # profile: /crawls/profiles/profile.tar.gz
        seedFile: /app/seedFile.txt
        collection: legal_alert
        screencastPort: 9037
        customBehaviors: /custom-behaviors/

        # Additional options
        allowHashUrls: true
        combineWARC: true
        generateWACZ: true
        workers: 8

        # Crawl specific options
        scopeType: "page-spa"


## Post-crawl

- A script callled [pages_json_log_validate.py](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/browsertrix-crawler%20files%20and%20scripts/pages_json_log_validate.py) can be used to read the pages.json log file which can be found in the WACZ file to find any potentially missing URLs.
- Ingest into Preservica at Admin/Private Repository/Web Captures/WACZ/WARC using the [AWS client](../preservica/aws-cli.md).


## Appendix:


`sudo docker run -p 9037:9037 -v $PWD/crawls:/crawls/ -v $PWD/crawl-config.yaml:/app/crawl-config.yaml -v $PWD/urlSeedFile.txt:/app/urlSeedFile.txt -it webrecorder/browsertrix-crawler crawl --config /app/crawl-config.yaml`

`sudo docker run -p 9037:9037 -v $PWD/crawls:/crawls -v $PWD/custom-behaviors/:/custom-behaviors/ webrecorder/browsertrix-crawler crawl --url https://example.com --customBehaviors /custom-behaviors/ --screencastPort 9037 --scopeType page --behaviors siteSpecific`