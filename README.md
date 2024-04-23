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

# dd
