# Scrapy

[Scrapy](https://github.com/scrapy/scrapy) is a powerful open-source web crawling framework built in Python. It's designed for efficiently extracting data from websites and processing it according to your needs. With Scrapy, users can define _spiders_—custom scripts that systematically navigate the web and scrape data, making it highly adaptable for various use cases. While we do not currently use Scrapy in our workflows, we may consider it in the future. This page outlines the steps to set up a web crawler using Scrapy.

## Installing Scrapy and Running the Script

Follow this step-by-step guide to install Scrapy, run the provided script, and understand its functionality:

### 1. Set Up Your Python Environment

1. Create a Virtual Environment:
   ```bash
   python -m venv venv
   ```
   2. Activate the Virtual Environment:
   - On Unix or MacOS:
     ```bash
     . ./venv/bin/activate
     ```

### 2. Install Scrapy

1. Install Scrapy within the activated virtual environment:
   ```bash
   pip install scrapy
   ```

### 3. Set Up a Scrapy Project

1. Create a Scrapy Project: Navigate to the desired directory in your terminal and run:
   ```bash
   scrapy startproject icaew_project
   ```
   - This command creates a new directory structure for your project, named `icaew_project`.
2. Navigate to the Project Directory:
   ```bash
   cd icaew_project
   ```

### 4. Create the Spider

1. Create a Spider File: Inside the `spiders` directory, create a new Python file:
   ```bash
   touch spiders/icaew_spider.py
   ```
2. Add the Spider Script: Open `icaew_spider.py` in your preferred text editor and insert the following code:

   ```python
   import scrapy
   from scrapy.spiders import SitemapSpider
   import csv
   import os

   class IcaewSpider(SitemapSpider):
       name = "icaew_spider"
       allowed_domains = ["icaew.com"]
       sitemap_urls = [
           "https://www.icaew.com/sitemap_corporate.xml"
       ]

       def __init__(self, *args, **kwargs):
           super(IcaewSpider, self).__init__(*args, **kwargs)
           # Initialize the CSV file
           if os.path.exists('icaew_results.csv'):
               os.remove('icaew_results.csv')  # Remove existing file
           with open('icaew_results.csv', 'w', newline='') as csvfile:
               csvwriter = csv.writer(csvfile)
               csvwriter.writerow(['URL', 'Element Present'])  # Write header

       def parse(self, response):
           element_present = bool(response.css('#c-app-widget-wrap'))
           url = response.url

           # Append the result to the CSV file
           with open('icaew_results.csv', 'a', newline='') as csvfile:
               csvwriter = csv.writer(csvfile)
               csvwriter.writerow([url, element_present])
   ```

### 5. Run the Spider

1. Run the Spider: Ensure you're in the root directory of your Scrapy project (where `scrapy.cfg` is located) and execute:
   ```bash
   scrapy crawl icaew_spider
   ```

### 6. View the Results

1. Check the Output: Once the spider completes its run, it generates a file named `icaew_results.csv` in your project directory.
   - Open this file with a text editor or spreadsheet application (e.g., Excel) to examine the results.

### Understanding the Script

Here's a breakdown of what the script does:

1. **Imports**:

   - The script imports necessary modules: `scrapy` for web scraping, `csv` for writing data to a CSV file, and `os` for file operations.

2. **Spider Class (`IcaewSpider`)**:

   - This class inherits from `SitemapSpider`, specialized for working with sitemaps. It defines the sitemap URL and the allowed domain (`icaew.com`).

3. **Initialization (`__init__` method)**:

   - The spider is initialized by creating or resetting the CSV file (`icaew_results.csv`) where the results will be stored. A header row is written to the file.

4. **Page Parsing (`parse` method)**:

   - For each URL in the sitemap, the spider checks if a specific HTML element (`#c-app-widget-wrap`) is present on the page. The URL and the presence of the element (`True` or `False`) are recorded in the CSV file.

5. **Running the Spider**:
   - The spider crawls all URLs listed in the sitemap, checks for the specified element, and logs the results into the CSV file.
