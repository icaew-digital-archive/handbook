# Web Crawlers

> **Purpose:** This document contains legacy web crawler configurations and notes. For current web crawling processes, see [Browsertrix-crawler](../web-archiving/browsertrix.md).

## Legacy Heritrix Configuration

> **Note:** The following Heritrix configuration is provided for reference only. Heritrix is no longer the primary web crawler.

**Environment setup:**
```bash
export JAVA_HOME=/usr
export HERITRIX_HOME=/home/digital-archivist/Documents/apps/heritrix/heritrix-3.4.0-20220727
chmod u+x $HERITRIX_HOME/bin/heritrix
export JAVA_OPTS=-Xmx1024M
$HERITRIX_HOME/bin/heritrix -a admin:admin -j /media/digital-archivist/Elements/heritrix/jobs
```

**Process:**
1. Find a sitemap
2. Convert sitemap using sitemap script

> **Note:** TO UPDATE TO NEWEST CRAWLER
> ```bash
> docker pull webrecorder/browsertrix-crawler
> ```





## Browsertrix-crawler Configuration

> **Note:** Updated 25/01/2023

### Creating a Login Profile

> **Purpose:** First step is to create a login profile with the following:

**Command:**
```bash
sudo docker run -p 6080:6080 -p 9223:9223 -v $PWD/crawls/profiles:/crawls/profiles/ -it webrecorder/browsertrix-crawler create-login-profile --url "https://my.icaew.com/security/Account/Login"
```

> **Steps:**
> 1. Open http://localhost:9223/ (as prompted in the terminal) in Chrome
> 2. Login into ICAEW.com
> 3. Once done, click "Create Profile"

### Running a Crawl

> **Prerequisites:** Ensure that `crawl-config.yaml` and `urlSeedFile.txt` exist in `$PWD` and then run the following:

**Command:**
```bash
sudo docker run -p 9037:9037 -v $PWD/crawls:/crawls/ -v $PWD/crawl-config.yaml:/app/crawl-config.yaml -v $PWD/urlSeedFile.txt:/app/urlSeedFile.txt -it webrecorder/browsertrix-crawler crawl --config /app/crawl-config.yaml
```





### Using Custom Behaviors

> **Purpose:** To use custom behaviors with Browsertrix-crawler:

**Command:**
```bash
sudo docker run -p 9037:9037 -v /home/digital-archivist/Downloads/behaviors.js:/app/node_modules/browsertrix-behaviors/dist/behaviors.js -v $PWD/crawls:/crawls/ -v $PWD/crawl-config.yaml:/app/crawl-config.yaml -v $PWD/urlSeedFile.txt:/app/urlSeedFile.txt -it webrecorder/browsertrix-crawler crawl --config /app/crawl-config.yaml
```

> **Note:** For more information on custom behaviors, see [Browsertrix-crawler custom behaviors](../web-archiving/browsertrix-behaviors.md).


## Configuration Examples

### Single Page Crawl

> **Purpose:** Example YAML configuration for crawling a single page.

> **Reference:** For additional configuration options, see [Crawling Configuration Options](https://github.com/webrecorder/browsertrix-crawler).

**Example:**
```yaml
# Basic setup
profile: /crawls/profiles/profile.tar.gz
seedFile: /app/urlSeedFile.txt
collection: ia-icaew-co-uk
screencastPort: 9037

# Additional options
allowHashUrls: true
generateWACZ: true
workers: 6
text: true

# Crawl specific options
scopeType: "page-spa"
```

### ICAEW.com Full Crawl


> **Purpose:** Example YAML configuration for crawling the main ICAEW.com site, including some extra exclusions.

> **Reference:** For additional configuration options, see [Crawling Configuration Options](https://github.com/webrecorder/browsertrix-crawler).

**Example:**
```yaml
# Basic setup
profile: /crawls/profiles/profile.tar.gz
seedFile: /app/urlSeedFile.txt
collection: icaew-com
screencastPort: 9037

# Additional options
allowHashUrls: true
combineWARC: true
generateWACZ: true
workers: 8

# Crawl specific options
scopeType: "custom"
include:
- ^(http(s)?:\/\/)?(www\.)?(careers\.|cdn\.|live\.|latest\.)?icaew\.com.*$ # scope in icaew.com, careers.icaew.com, cd.icaew.com, live.icaew.com and latest.icaew.com
- ^(http(s)?:\/\/)?(www\.)?(train|volunteer)\.icaew\.com(\/)?(blog.*)?$ # scope in parent and blog pages only
exclude:
- ^.*(l|L)(o|O)(g|G)(o|O)(f|F)(f|F).*$ # block logout URLs
- ^(http(s)?:\/\/)?(www\.)?icaew\.com\/search.*$ # block search pages (robots.txt)
- ^(http(s)?:\/\/)?(www\.)?.*\/member(s|ship)\/active-members.*$ # block active-members pages and media files
- ^(http(s)?:\/\/)?(www\.)?.*sprint-test-pages.*$ # block sprint-test-pages
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/corporate-governance/new-boardroom-agenda/culture-holds-the-key.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/changes-ahead-for-rd-tax-relief.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/does-a-wayleave-change-a-property-from-residential-to-nonresidential.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/how-employers-can-fund-an-apprenticeship.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/Inheritance-tax-pitfalls-and-planning-points.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/making-the-most-of-capital-losses.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/plastic-packaging-tax-the-lessons-so-far.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/reflections-on-the-mtd-itsa-deferral.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/simplicity-sustainability-and-technology-key-themes-for-VAT-at-50.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/tax-thresholds-in-deep-freeze.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/Understanding-the-statutory-clearance-process.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/vat-50-years-young.*$
- ^(http(s)?:\/\/)?(www\.)?.*https://www.icaew.com/technical/tax/tax-faculty/taxline/2023/articles/why-now-is-the-time-to-revisit-company-share-option-plans.*$
```
