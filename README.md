# Scrape-Webpages-and-Save-Info.-to-A-Document

# Project Objectives:
Scrape text data from specific sections of a website and save results to a word document for later analysis. I wanted an easier way to collect information about companies I was interested in working for.

# Python packages used:
`requests`: Used for making HTTP requests to fetch web pages.

`BeautifulSoup` from `bs4`: Used for parsing HTML content and extracting relevant information from web pages.

`random`: Used to randomly select a user-agent from a list of user-agent strings.

`re`: Regular expression module, used for text manipulation and pattern matching.

`Document` from `docx`: Used for creating and manipulating Microsoft Word documents.

# Custom Python functions used:
`getdata(url)`: Fetches the HTML content of a given URL using the `requests` library and returns a BeautifulSoup object containing the parsed HTML.
   
`extract_text_no_whitespace(url)`: Extracts text content from HTML elements (specifically `<p>` tags) of a given URL, removes any extra whitespace, and returns a list of text strings.

`extract_text_from_href(href_links)`: Extracts text content from HTML elements (specifically `<p>` tags) of multiple URLs (provided as a list of href links), removes any extra whitespace, and returns a concatenated list of text strings.

`get_href_links(url, user_agents_list)`: Retrieves href links from a given URL by parsing the HTML content, filters out links that start with "http", and returns a list of extracted href links.

# Project deliverables:

# Python code

```python

# code to get text from specific href links (one or more links). Collect links from a single source and get text from specified links.

from bs4 import BeautifulSoup
import requests
import random
import re
from docx import Document

# homepage url
url = 'https://www.haskell.com/'

# use fake user agent to bypass 403 error
user_agents_list = [
    'Mozilla/5.0 (iPad; CPU OS 12_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.83 Safari/537.36',
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36'
]

# custom functions
def getdata(url):
    headers = {'User-Agent': random.choice(user_agents_list)}
    r = requests.get(url, headers=headers)
    soup = BeautifulSoup(r.content, 'html.parser')
    return soup.prettify()

def extract_text_no_whitespace(url):
    html_content = getdata(url)
    soup = BeautifulSoup(html_content, 'html.parser')
    elementor_items = soup.find_all(['p']) #'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'span', 'div', 'li', 'a'])
    text_list = [re.sub(r'\s+', ' ', item.get_text(strip=True)) for item in elementor_items]
    return text_list

def extract_text_from_href(href_links):
    all_text = []
    for href_link in href_links:
        html_content = getdata(href_link)
        soup = BeautifulSoup(html_content, 'html.parser')
        elementor_items = soup.find_all(['p']) #'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'span', 'div', 'li', 'a'])
        text_list = [re.sub(r'\s+', ' ', item.get_text(strip=True)) for item in elementor_items]
        all_text.extend(text_list)
    return all_text

def get_href_links(url, user_agents_list):
    href_links = []
    
    try:
        headers = {'User-Agent': random.choice(user_agents_list)}
        response = requests.get(url, headers=headers)
        if response.status_code == 200:
            soup = BeautifulSoup(response.content, 'html.parser')
            href_tags = soup.find_all(['a', 'link'], href=True)
            for tag in href_tags:
                href = tag['href']
                if href.startswith('http'):
                    href_links.append(href)
        else:
            print("Failed to fetch URL. Status code:", response.status_code)
    except Exception as e:
        print("An error occurred:", str(e))
    
    return href_links

# Specify the specific href links you want to extract text from
   # I chose links from the companies "About" section
href_links = [
    'https://www.haskell.com/about/haskell-101/',
    'https://www.haskell.com/about/our-team/',
    'https://www.haskell.com/about/awards-recognition/',
    'https://www.haskell.com/about/charitable-contributions/',
    'https://www.haskell.com/about/community-impact/',
    'https://www.haskell.com/careers/culture/',
    'https://www.haskell.com/careers/diversity-inclusion/',
    'https://www.haskell.com/about/history/',
    'https://www.haskell.com/about/innovation/',
    'https://www.haskell.com/about/quality/',
    'https://www.haskell.com/about/safety/',
    'https://www.haskell.com/about/sustainability/'
]

# Call the function to extract text from specific href links
result = extract_text_from_href(href_links)

# Open a new Word document
doc = Document()

# Iterate over the extracted text and add each element to a new paragraph in the Word document
for item in result:
    doc.add_paragraph(item)

# Save the Word document
doc.save('output1.docx')

```
























