### EX8 Web Scraping On E-commerce platform using BeautifulSoup
### DATE: 10-05-2025
### AIM: 
To perform Web Scraping on Snapdeal using (beautifulsoup) Python.
### Description: 
<div align = "justify">
Web scraping is the process of extracting data from various websites and parsing it. In other words, it’s a technique 
to extract unstructured data and store that data either in a local file or in a database. 
There are many ways to collect data that involve a huge amount of hard work and consume a lot of time. Web scraping can save programmers many hours. Beautiful Soup is a Python web scraping library that allows us to parse and scrape HTML and XML pages. 
One can search, navigate, and modify data using a parser. It’s versatile and saves a lot of time.
<p>The basic steps involved in web scraping are:
<p>1) Loading the document (HTML content)
<p>2) Parsing the document
<p>3) Extraction
<p>4) Transformation

### Procedure:

1) Import necessary libraries (requests, BeautifulSoup, re, matplotlib.pyplot).
2) Define convert_price_to_float(price) Function: to Remove non-numeric characters from a price string and convert it to a float.
3) Define get_amazon_products(search_query) Function: to Scrape Amazon for product information based on the search query.
4) Fetch and parse the HTML content then Extract product names and prices from the search results and Sort product information based on converted prices in ascending order.
5) Return sorted product data as a list of dictionaries.
6) Call get_amazon_products(search_query) to get product data based on the user's search query.
7) Check if products are found; if not, display "No products found."
8) Visualize Product Data using a Bar Chart

### Program:
```PYTHON
import requests
from bs4 import BeautifulSoup
import re
import matplotlib.pyplot as plt

def convert_price_to_float(price):
    # Remove currency symbols and commas, and then convert to float
    price = re.sub(r'[^\d.]', '', price)  # Remove non-digit characters except '.'
    return float(price) if price else 0.0

def get_amazon_products(search_query):
    base_url = 'https://www.snapdeal.com/'
    headers = {
        'User-Agent': 'Your User Agent'  # Add your User Agent here
    }

    search_query = search_query.replace(' ', '-')
    url = f'{base_url}/products/{search_query}'

    response = requests.get(url, headers=headers)
    products_data = []  # List to store product information

    if response.status_code == 200:
        # TYPE YOUR CODE HERE
        soup = BeautifulSoup(response.content, 'html.parser')
        products = soup.find_all('div', {'class': 'product-tuple-listing'})

        for product in products:
            title = product.find('p', {'class': 'product-title'})
            price = product.find('span', {'class': 'product-price'})
            if price:
                product_price = convert_price_to_float(price.get('data-price', '0'))
            else:
                product_price = 0.0  # Default to 0 if no price found
            rating = product.find('div', {'class': 'filled-stars'})  # Assuming rating is shown with this class

            if title and price:
                product_name = title.text.strip()
                #product_price = re.sub(r'[^\d.]', '', price.text.strip())  # Remove non-numeric chars for price
                product_rating = rating['style'].split(';')[0].split(':')[-1] if rating else "No rating"
                products_data.append({
                    'Product': product_name,
                    'Price': float(product_price),
                    'Rating': product_rating
                })
                print(f'Product: {product_name}')
                print(f'Price: {product_price}')
                print(f'Rating: {product_rating}')
                print('---')
    else:
        print('Failed to retrieve content')

    return sorted(products_data, key=lambda x: convert_price_to_float(x['Price']))

search_query = input('Enter product to search on Snapdeal: ')
products = get_amazon_products(search_query)

# Displaying product data using a bar chart
if products:  # Check if products list is not empty
    product_names = [product['Product'][:30] if len(product['Product']) > 30 else product['Product'] for product in products]
    product_prices = [convert_price_to_float(product['Price']) for product in products]

    plt.figure(figsize=(10, 6))
    plt.barh(range(len(product_prices)), product_prices, color='skyblue')
    plt.xlabel('Price')
    plt.ylabel('Product')
    plt.title(f'Products and their Prices on Amazon for {search_query.capitalize()} (Ascending Order)')
    plt.yticks(range(len(product_prices)), product_names)  # Setting y-axis labels as shortened product names
    plt.tight_layout()
    plt.show()
else:
    print('No products found.')

```

### Output:
![sample_output](image.png)

### Result:
Thus, web scraping in snapdeal using (Beautiful Soup) Python is performed successfully.