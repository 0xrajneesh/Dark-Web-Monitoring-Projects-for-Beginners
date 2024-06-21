# Dark Web Data Extraction with Python and Beautiful Soup

## Introduction
In this project, you will learn how to use Python and Beautiful Soup, a powerful library for web scraping, to extract data from the dark web. You will set up Tor to access the dark web, use Python to connect to .onion sites, and utilize Beautiful Soup to parse and extract information.

## Lab Setup
- **Operating System**: Linux (Ubuntu 20.04 recommended)
- **Tools Required**: Python, Tor, Beautiful Soup

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
#### 2. Installing Python and Beautiful Soup
- Install Python and the necessary libraries:

```bash
sudo apt install python3 python3-pip
pip3 install requests[socks] beautifulsoup4
```

## Exercises

### Exercise 1: Setting Up a Python Script to Connect to the Dark Web

1. Create a Python Script: Create a new file named darkweb_scraper.py.
```bash
nano darkweb_scraper.py
```
2. Connect to a Dark Web Site: Add the following code to connect to a .onion site via Tor.
```python
import requests

session = requests.session()
session.proxies = {'http': 'socks5h://localhost:9050', 'https': 'socks5h://localhost:9050'}

url = 'http://example.onion'
response = session.get(url)

print(response.text)
```
**Expected Output**
- The HTML content of the .onion site will be printed to the console.

### Exercise 2: Parsing HTML with Beautiful Soup
1. Modify the Script to Use Beautiful Soup: Update darkweb_scraper.py to parse the HTML content.
```python
import requests
from bs4 import BeautifulSoup

session = requests.session()
session.proxies = {'http': 'socks5h://localhost:9050', 'https': 'socks5h://localhost:9050'}

url = 'http://example.onion'
response = session.get(url)

soup = BeautifulSoup(response.text, 'html.parser')
print(soup.prettify())
```
**Expected Output**
- The parsed HTML content of the .onion site will be printed to the console in a readable format.

### Exercise 3: Extracting Specific Data
1. Extract Data: Update darkweb_scraper.py to extract specific data, such as titles and links.
```python
import requests
from bs4 import BeautifulSoup

session = requests.session()
session.proxies = {'http': 'socks5h://localhost:9050', 'https': 'socks5h://localhost:9050'}

url = 'http://example.onion'
response = session.get(url)

soup = BeautifulSoup(response.text, 'html.parser')

# Extract titles
titles = soup.find_all('title')
for title in titles:
    print(title.get_text())

# Extract links
links = soup.find_all('a')
for link in links:
    print(link.get('href'))
```

**Expected Output**
- Titles and links from the .onion site will be printed to the console.

### Exercise 4: Storing Extracted Data
1. Store Data in a JSON File: Update darkweb_scraper.py to save extracted data to a JSON file.
```python
import requests
from bs4 import BeautifulSoup
import json

session = requests.session()
session.proxies = {'http': 'socks5h://localhost:9050', 'https': 'socks5h://localhost:9050'}

url = 'http://example.onion'
response = session.get(url)

soup = BeautifulSoup(response.text, 'html.parser')

data = {
    'titles': [title.get_text() for title in soup.find_all('title')],
    'links': [link.get('href') for link in soup.find_all('a')]
}

with open('data.json', 'w') as f:
    json.dump(data, f)
```
**Expected Output**
- Extracted data will be saved to data.json.

### Exercise 5: Scheduling the Script for Regular Data Extraction
1. Create a Cron Job: Schedule the script to run at regular intervals using cron.
```bash
crontab -e
```
2. Add the following line to run the script every day at midnight:

```bash
0 0 * * * /usr/bin/python3 /path/to/darkweb_scraper.py
```
**Expected Output**
- The script will run daily, extracting and storing data from the dark web.

## Conclusion
By completing these exercises, you have learned how to set up and use Python and Beautiful Soup for dark web data extraction. You configured Tor to access the dark web, wrote a Python script to connect to .onion sites, parsed and extracted specific information, stored the extracted data, and scheduled the script for regular execution. This hands-on experience equips you with practical skills in dark web scraping and data extraction.
