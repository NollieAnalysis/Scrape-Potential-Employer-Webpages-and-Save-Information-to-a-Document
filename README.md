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

Sample section of website that was scraped

![website_sample](https://github.com/NollieAnalysis/Scrape-Webpages-and-Save-Info.-to-A-Document/assets/163913188/7400cac3-c4f5-4b46-a776-8ea833f43c9a)


Looking specifically for `<p>` tags

![website_p_tag_sample](https://github.com/NollieAnalysis/Scrape-Webpages-and-Save-Info.-to-A-Document/assets/163913188/f5f375ca-c9d4-4a19-90d0-ee4f4210eebe)


output1.docx file

![output1_document_sample](https://github.com/NollieAnalysis/Scrape-Webpages-and-Save-Info.-to-A-Document/assets/163913188/5bd63f42-0144-4c09-a7e9-d8d755577bbb)

Asking a LLM to summarize output file

```
Can you summarize (in bullet points) the below text?


We provide fully integrated facility solutions that ensure the execution of capital projects across markets, offering local presence and global reach to the private and public sectors.
With broad-based expertise across disciplines, we are an unparalleled source for performance-driven facilities built to deliver, from concept to ribbon-cutting.
With a broad range of in-house engineering expertise, we can optimize your current operations or take your new project from concept to completion, providing unrivaled service and results.
Haskell is a global network of experts providing integrated design, engineering, construction and professional services to clients and communities.
Home/About/Haskell 101
Walk the halls of Haskell’s Jacksonville, Florida, headquarters and you’ll see all the signs of a thriving design and construction company, as drafting tables and complex computerized models juxtapose with hardhats and transit levels.
To know Haskell is to know the spirit of innovation and the assurance of certainty that lie beneath. To know Haskell is to understand unrivaled client care, unparalleled high quality and global scale. But to really know Haskell is to know how deeply its more than 2,200 team members care for their customers, their colleagues and their communities.

From its first commercial construction contract, Haskell has been breaking new ground in the architectural, engineering and construction (AEC) world to create the greatest possible value for project owners. Design-build project delivery didn’t exist as a discipline in 1967, whenThe Preston H. Haskell Companycontracted to build a 53,160-square-foot manufacturing plant for Covington Industries. Haskell’s newly formed team broke the mold, however, by producing the entire project, from concept to commissioning.
The idea was simple and elegant. Adesign-buildcontractor could act as a single point of contact – and a single party accountable to the project owner – providing clients certainty of outcome in budget, schedule and performance. Preston Haskell was an early proponent andbecame a leading evangelistfor the design-build delivery method, which now accounts for nearly 50% of all construction spending in the United States.
In the more than almost six decades since, Haskell has shortened its name but grown in myriad ways.
Driven by its innovative spirit and relentless pursuit of optimal customer service, Haskell continued to lead the industry in adopting new project delivery methods. Its uniquePermanent Craft Employee (PCE)program staffed crews that pioneered tilt-wall construction for greater efficiency. Its leadership team leveraged organic growth and acquisition to assemble the process, packaging and material handling expertise to put Haskell on the cutting edge of the turnkeyEngineer Procure Construct (EPC)method of delivering solutions for manufacturing customers. Today, throughDysruptek, its innovation-focused venture capital arm, Haskell is scouting, piloting and investing in emerging technologies that stretch the status quo to provide clients with the greatest possible competitive advantage.
“The foundation of Haskell is fantastic,” said HaskellChairman, Chief Executive Officer (CEO) and President Jim O’Leary. “You’re talking about a place where you have high-quality top people in our industry; a place where you have one of the best reputations in the industry; a place where you have operational excellence and systems and everything in place to do well; a place where you have a broad customer base that is happy with what you’re doing. So, it’s all there; it’s all been there. It’s a foundation we can continue to build on.”
Build, O’Leary and his team have done. Haskell has doubled its workforce and revenue since August 2018, when he became just the third CEO in the company’s history. More importantly, it has further laid a foundation for the future.
O’Leary expanded the company’s top management, leveraging what had been the three-person Office of the Chief Executive to the eight-member Executive Leadership Team (ELT). Those eight leaders then thoughtfully created a strategic plan they called Haskell 2025, a set of goals, some operational and some aspirational, and a set of dynamic key initiatives designed to best serve clients and their 2,200-plus employee-owners alike. Indeed, the six pillars of Haskell 2025 start with a pledge to employees, who are known as team members:
Instilling an inclusive culture prompted the ELT expansion, and in 2023, it led to an organizational transformation to an Enterprise Project Delivery Model (EPDM), unifying similar disciplines from across its legacy Consumer Packaged Goods (CPG), Infrastructure & Transportation (I&T), Design and Consulting Services (DCS), and International groups to form four new operating groups that serve all markets.
In its first year, the new structure proved successful in meeting the goals of enhancing consistency, operational excellence, and improved career opportunities.
“Probably the largest accomplishment and the one that I’m most proud of is building culture and ingraining our core values of Team, Excellence, Service and Trust,” O’Leary said. “We have an environment of collaboration and transparent communication. This is just about us being open and honest and straightforward. We share. We share what we know when we know it. And that’s the way this organization is going to be.”
Since 2018, Haskell’s revenue has doubled to just shy of $2 billion, and its workforce has grown 40%. Ensuring that such expansion is meaningful and profitable has required planning, foresight and investment in several areas crucial to the entire AEC industry.
```





















