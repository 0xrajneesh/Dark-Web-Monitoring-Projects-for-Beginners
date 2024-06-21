# Using Scrapy for Dark Web Data Collection

## Introduction
In this project, you will learn how to use Scrapy, an open-source web crawling framework for Python, to collect data from the dark web. Scrapy is a powerful and flexible tool that can be used to crawl and scrape information from websites, including those on the dark web accessed via Tor.

## Lab Setup
- **Operating System**: Linux (Ubuntu 20.04 recommended)
- **Tools Required**: Scrapy, Tor, Polipo (a caching web proxy)

### Tool Installation

#### 1. Installing Tor
- To access the dark web, you need to install the Tor browser.

```bash
sudo apt update
sudo apt install tor
```
- Start the Tor service:

```bash
sudo systemctl start tor
sudo systemctl enable tor
```
#### 2. Installing Polipo
- Polipo is a caching web proxy that we will use to route Scrapy traffic through Tor.

```bash
sudo apt install polipo
```

- Configure Polipo to use Tor. Edit the Polipo configuration file:

```bash
sudo nano /etc/polipo/config
```
- Add the following lines:

```makefile
socksParentProxy = "localhost:9050"
socksProxyType = socks5
proxyAddress = "0.0.0.0"
allowedClients = 127.0.0.1
allowedPorts = 1-65535
```
- Restart Polipo:

```bash
sudo systemctl restart polipo
```
#### 3. Installing Scrapy
Install Scrapy using pip:

```bash
pip3 install scrapy
```

## Exercises

### Exercise 1: Setting Up a Scrapy Project
1. Create a New Scrapy Project: Open a terminal and run the following command to create a new Scrapy project named darkweb_scraper.
```bash
scrapy startproject darkweb_scraper
```
2. Navigate to the Project Directory:
```bash
cd darkweb_scraper
```
**Expected Output**
- A new Scrapy project structure will be created in the darkweb_scraper directory.

### Exercise 2: Creating a Scrapy Spider for Dark Web Crawling
1. Create a New Spider: In the darkweb_scraper/spiders directory, create a new file named darkweb_spider.py.
```bash
nano darkweb_scraper/spiders/darkweb_spider.py
```
2. Define the Spider: Add the following code to define a basic spider that crawls a dark web site (replace example.onion with the actual .onion site you want to crawl).
```python
import scrapy

class DarkWebSpider(scrapy.Spider):
    name = 'darkweb_spider'
    allowed_domains = ['example.onion']
    start_urls = ['http://example.onion/']

    custom_settings = {
        'HTTP_PROXY': 'http://127.0.0.1:8123'
    }

    def parse(self, response):
        self.log('Visited: %s' % response.url)
        # Add your parsing logic here
```
**Expected Output**
- A spider script ready to crawl a specified .onion site.

### Exercise 3: Running the Spider
1. Run the Spider: Execute the following command to run the spider:
```bash
scrapy crawl darkweb_spider
```
**Expected Output**
- Scrapy will start crawling the specified dark web site, logging visited URLs to the console.

### Exercise 4: Extracting Data from the Dark Web
1. Modify the Spider to Extract Data: Update the parse method in darkweb_spider.py to extract specific data, such as titles, links, or any other information.
```python
def parse(self, response):
    self.log('Visited: %s' % response.url)
    titles = response.css('title::text').getall()
    for title in titles:
        self.log('Title: %s' % title)
```
2. Run the Spider Again:
```bash
scrapy crawl darkweb_spider
```
**Expected Output**
- Extracted data (e.g., titles) from the dark web site will be logged to the console.

### Exercise 5: Storing Extracted Data
1. Modify the Spider to Store Data: Update the parse method to save extracted data to a JSON file.
```python
import json

def parse(self, response):
    self.log('Visited: %s' % response.url)
    titles = response.css('title::text').getall()
    data = {'titles': titles}
    
    with open('data.json', 'w') as f:
        json.dump(data, f)
```
2. Run the Spider:

```bash
scrapy crawl darkweb_spider
```
**Expected Output**
- Extracted data will be saved to data.json.

## Conclusion
By completing these exercises, you have learned how to set up and use Scrapy for dark web data collection, configure a web proxy to route traffic through Tor, create and run a Scrapy spider, extract data from dark web sites, and store the collected data. This hands-on experience equips you with practical skills in dark web crawling and data extraction.
