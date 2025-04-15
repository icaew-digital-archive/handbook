# Web Archiving Process Overview

## Prerequisites

- Digital Archive Workstation access
- Required software installed:
  - wget
  - browsertrix-crawler
  - ArchiveWeb.page
  - ReplayWeb.page
- Sufficient storage space
- Network access to target sites

## Process Overview

### 1. Template Testing Crawl

Before running a full crawl, it's important to test how the crawler handles various templates and elements on the site. This helps identify potential issues with capture and playback.

#### Setup for Template Testing

1. Update the crawler:

   ```bash
   docker pull webrecorder/browsertrix-crawler
   ```

2. Create required directories and files:

   - Create a folder with `crawl-config.yaml`
   - Create `custom-behaviors` subfolder
   - Add ICAEW behaviors file from GitHub

3. Create logged-in profile:

   ```bash
   sudo docker run -p 6080:6080 -p 9223:9223 -v $PWD/crawls/profiles:/crawls/profiles/ \
   -it webrecorder/browsertrix-crawler create-login-profile \
   --url "https://my.icaew.com/security/Account/Login"
   ```

4. Profile Setup Steps:

   1. Open http://localhost:9223/ in Chrome
   2. Click "Allow all cookies"
   3. Disable Brave shields
   4. Login with credentials
   5. Disable Brave shields again
   6. Close "Discover the latest MyICAEW App" popup
   7. Navigate to a page with StreamAMG video player
   8. Create Profile

5. Start the crawl:

   ```bash
   sudo docker run -p 9037:9037 \
   -v $PWD/crawls:/crawls \
   -v $PWD/custom-behaviors/:/custom-behaviors/ \
   -v $PWD/crawl-config.yaml:/app/crawl-config.yaml \
   -v $PWD/seedFile.txt:/app/seedFile.txt \
   webrecorder/browsertrix-crawler crawl \
   --config /app/crawl-config.yaml
   ```

6. Monitor the crawl at http://localhost:9037

#### Post-Template Testing

1. Validate the WACZ file:

   ```bash
   python3 web_archive_validator.py [WACZ_FILE]
   ```

2. Run QA crawl:
   ```bash
   sudo docker run -v $PWD/crawls/:/crawls/ \
   -it webrecorder/browsertrix-crawler qa \
   --qaSource /crawls/collections/template-testing/template-testing.wacz \
   --collection example-qa \
   --generateWACZ \
   --workers 8
   ```

### 2. Sitecore Media Library Download

1. Access the Sitecore Media Library
2. Download the complete library as a zip file
3. Verify the download size (~10GB expected)
4. Note: This includes only media within Sitecore, not third-party content

### 3. Ingest to Preservica

1. Rename file following standard naming convention
2. Ingest to: `Admin / Private Repository / Web Captures / Sitecore Media Library Downloads`
3. Verify checksums:
   ```bash
   sha1sum [FILE]
   ```
4. Confirm in Preservica: Advanced → Bitstream

### 4. Sitemap Processing

1. Save ICAEW sitemap for:

   - Input for crawlers
   - Post-crawl validation
   - Content reference

2. Convert Sitemap:
   ```bash
   python3 sitemap_xml_to_txt_or_html.py --to_file 202XXXXX_sitemap.txt \
   https://www.icaew.com/sitemap_corporate.xml \
   https://www.icaew.com/sitemap_careers.xml \
   --exclude_strings "sprint-test-pages" "active-members"
   ```

### 5. Crawl Execution

#### wget Crawl

- Follow [wget guide](./wget.md) for setup
- Use provided configurations
- Monitor log files
- Validate output

#### browsertrix-crawler

- Follow [browsertrix guide](./browsertrix.md)
- Use custom behaviors
- Monitor progress
- Validate captures

### 6. Post-Crawl Processing

1. Validate WACZ files:

   ```bash
   python3 wacz_validator.py [WACZ_FILE]
   ```

2. Check logs:

   ```bash
   python3 wget_log_reader.py [LOG_FILE]
   python3 pages_json_log_validate.py [PAGES.JSON]
   ```

3. Upload to Preservica:
   - Use AWS CLI for large files
   - Verify checksums
   - Validate in Preservica

## Quality Assurance

### Validation Steps

1. Check file integrity
2. Verify metadata completeness
3. Test playback in ReplayWeb.page
4. Validate checksums
5. Review crawl coverage
6. Verify template functionality
7. Check video playback
8. Validate interactive elements

### Common Issues

1. Incomplete crawls
2. Missing resources
3. Authentication failures
4. Storage limitations
5. Network timeouts
6. Template rendering issues
7. Video playback problems
8. Interactive element failures

## Best Practices

1. Always test with small crawls first
2. Monitor system resources
3. Keep detailed logs
4. Document any issues
5. Regular validation checks
6. Maintain backup copies
7. Test all template types
8. Verify interactive content

## Support

For issues or questions, contact the Digital Archive team.
