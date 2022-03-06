# ETL (Extract)

Extract Transform Load is the process whereby some data is obtained, (extracted) cleaned, wrangled (transformed), and placed into a user-friendly data structure like a data frame (loaded).

Extraction involves using some tool to pull data from a source, most commonly with API and webpages will involve using the [Requests] package

---
## Webpages (HTML)
### BeautifulSoup 
[Documentation](https://beautiful-soup-4.readthedocs.io/en/latest/)

Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.
```python
from bs4 import BeautifulSoup as bs

html_doc = """
<html><head><title>BeautifulSoup Demo</title></head>
<body>
<p class="title"><b>BS4 Demo title</b></p>
<p class="story">...</p>
</body>
</html>
"""
soup = bs(html_doc, 'html.parser')
```
When pulling from `Requests` you can use:
```python
from bs4 import BeautifulSoup as bs
import requests

response = requests.get("http://some-url.com")
soup = bs(response.content, 'html.parser')
```

---
### MechanicalSoup
[Documentation](https://mechanicalsoup.readthedocs.io/en/stable/)

A Python library for automating interaction with websites. MechanicalSoup automatically stores and sends cookies, follows redirects, and can follow links and submit forms. It doesn’t do Javascript.

```python
"""
Example usage of MechanicalSoup to get the results from DuckDuckGo.
"""
import mechanicalsoup


# Connect to duckduckgo
browser = mechanicalsoup.StatefulBrowser(user_agent="MechanicalSoup")
# Need to use the non JS version of DDG since Python can't render JS
browser.open("https://html.duckduckgo.com/html/")

# methods available to browser
# browser.absolute_url(        browser.get_cookiejar(       browser.links(               browser.put(                 browser.set_user_agent(
# browser.add_soup(            browser.get_current_form(    browser.list_links(          browser.raise_on_404         browser.set_verbose(
# browser.close(               browser.get_current_page(    browser.new_control(         browser.refresh(             browser.soup_config
# browser.download_link(       browser.get_debug(           browser.open(                browser.request(             browser.submit(
# browser.find_link(           browser.get_request_kwargs(  browser.open_fake_page(      browser.select_form(         browser.submit_selected(
# browser.follow_link(         browser.get_url(             browser.open_relative(       browser.session              browser.url
# browser.form                 browser.get_verbose(         browser.page(                browser.set_cookiejar(       
# browser.get(                 browser.launch_browser(      browser.post(                browser.set_debug(

# Fill-in the search form
browser.select_form('#search_form_homepage')

# this will open a browser to show the HTML currently selected in the browser object
browser.launch_browser()

browser["q"] = "MechanicalSoup"
browser.submit_selected()

# Display the results
for link in browser.page.select('a.result__a'):
    print(link.text, '->', link.attrs['href'])
```

---
### Selenium
[Documentation](https://selenium-python.readthedocs.io/)

Selenium Python bindings provides a simple API to write functional/acceptance tests using Selenium WebDriver. Through Selenium Python API you can access all functionalities of Selenium WebDriver in an intuitive way.


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

# The instance of Firefox WebDriver is created
driver = webdriver.Firefox()

# The driver.get method will navigate to a page given by the URL. 
# WebDriver will wait until the page has fully loaded (the “onload” event has fired)
# before returning control to your test or script
driver.get("http://www.python.org")

# assertion to confirm that title has “Python” word in it
assert "Python" in driver.title

# WebDriver offers a number of ways to find elements using one of the find_element_by_* methods.
elem = driver.find_element_by_name("q")

# we’ll first clear any pre-populated text in the input field
elem.clear()

# we are sending keys, this is similar to entering keys using your keyboard
elem.send_keys("pycon")
elem.send_keys(Keys.RETURN)

# ensure that some results are found, make an assertion
assert "No results found." not in driver.page_source

# The driver.quit() will exit entire browser whereas drive.close() will close one tab
driver.close()
```
Some additional WebDrivers for Selenium include the following:
```
webdriver.Firefox
webdriver.FirefoxProfile
webdriver.Chrome
webdriver.ChromeOptions
webdriver.Ie
webdriver.Opera
webdriver.PhantomJS
webdriver.Remote
webdriver.DesiredCapabilities
webdriver.ActionChains
webdriver.TouchActions
webdriver.Proxy
```
To use without actually opening a window and save CPU use Chrome driver in `--headless` mode:
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

# instantiate a chrome options object so you can set the size and headless preference
chrome_options = Options()
chrome_options.add_argument("--headless")
```
---
## APIs

---
## Databases
### SQLite3
[Documentation](https://mechanicalsoup.readthedocs.io/en/stable/)

```python
import sqlite3

conn = sqlite3.connect('test_database') 
c = conn.cursor()

c.execute('''
          CREATE TABLE IF NOT EXISTS products
          ([product_id] INTEGER PRIMARY KEY, [product_name] TEXT, [price] INTEGER)
          ''')
          
c.execute('''
          INSERT INTO products (product_id, product_name, price)

                VALUES
                (1,'Computer',800),
                (2,'Printer',200),
                (3,'Tablet',300),
                (4,'Desk',450),
                (5,'Chair',150)
          ''')                     

conn.commit()
```


### SQLAlchemy
[Documentation](https://www.sqlalchemy.org/)

#### Viewing Tables
```python
import sqlalchemy as db
engine = db.create_engine('sqlite:///census.sqlite')

connection = engine.connect()
metadata = db.MetaData()
census = db.Table('census', metadata, autoload=True, autoload_with=engine)
```
#### Querying Tables
```python
#Equivalent to 'SELECT * FROM census'
query = db.select([census])

# ResultProxy: The object returned by the .execute() method. 
# It can be used in a variety of ways to get the data returned by the query.
ResultProxy = connection.execute(query)

# ResultSet: The actual data asked for in the query when using
# a fetch method such as .fetchall() on a ResultProxy.
ResultSet = ResultProxy.fetchall()
```
Additional Querying tutorial [SQLAlchemy — Python Tutorial](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)

### Pandas
[Documentation](https://pandas.pydata.org/docs/)


```python
# import the modules
import pandas as pd 
from sqlalchemy import create_engine
  
# SQLAlchemy connectable
cnx = create_engine('sqlite:///contacts.db').connect()
  
# table named 'contacts' will be returned as a dataframe.
df = pd.read_sql_table('contacts', cnx)
print(df)
```


---
## Flat Files
### Python
Python has the ability to read many files when given a path. Using `with open` will close the file when the with statement closes.
More parameters on reading files can be found here [Real Python Tutorial]("https://realpython.com/read-write-files-python/#opening-and-closing-a-file-in-python")
```python
with open('dog_breeds.txt', 'r') as reader:
    # Further file processing goes here
```
additional parameters include:

Character | Meaning |
---|---|
'r' | Open for reading (default) |
'w' | Open for writing, truncating (overwriting) the file first |
'rb' or wb' | Open in binary mode (read/write using byte data) |

### Pandas
[Documentation](https://pandas.pydata.org/docs/)

Pandas can read many types of flat files including `csv`, `parquet`, `xlsx`, `txt`, `pickle`, `clipboard`, `xml`, `html`, `json` and others.

```python
import pandas as pd

# Reading CSV
df = pd.read_csv("path/to/file.csv")
# Reading CSV Faster with Pyarrow in Pandas 1.4
df = pd.read_csv("large.csv", engine="pyarrow")

# Reading Parquet file
df = pd.read_parquet("large.parquet")
# Reading Parquet file with faster parquet engine
df = pd.read_parquet("large.parquet", engine="fastparquet")

# Reading Excel
df = pd.read_excel("path/to/file.xlsx")
# Reading Excel with multiple sheets
xls = pd.ExcelFile('path_to_file.xls')
df1 = pd.read_excel(xls, 'Sheet1')
df2 = pd.read_excel(xls, 'Sheet2')

# Reading JSON
df = pd.read_json("path/to/file.json")
# nested JSON often leaves the df in an undesirable state
df = pd.json_normalize(df['data'])
# this will flatten lists into columns
df.explode('col_of_lists')

# Reading JSONL
df = pd.read_json("path/to/file.jsonl", lines=True)

```

[Requests]: ../web/webscraping.md#Requests