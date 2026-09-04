### EX8 Web Scraping On E-commerce platform using BeautifulSoup
### REG NO: 212224040036
### AIM: To perform Web Scraping on Amazon using (beautifulsoup) Python.
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
    price = re.sub(r'[^\d.]', '', str(price))
    return float(price) if price else 0.0


def convert_rating_to_float(rating):
    match = re.search(r'(\d+\.?\d*)', str(rating))

    if match:
        return float(match.group(1))

    return 0.0


def get_sample_products():

    return [
        {
            "Product": "Xiaomi 17 Ultra 1TB 16GB RAM",
            "Price": "64999",
            "Rating": 4.4
        },
        {
            "Product": "Nothing Phone (3) Black 16GB",
            "Price": "49999",
            "Rating": 4.4
        },
        {
            "Product": "Redmi Turbo 5 8GB 256GB",
            "Price": "44999",
            "Rating": 4.1
        },
        {
            "Product": "OnePlus Nord CE6 8GB 128GB",
            "Price": "39999",
            "Rating": 4.3
        },
        {
            "Product": "Samsung Galaxy S26 Awesome Black",
            "Price": "34999",
            "Rating": 4.3
        },
        {
            "Product": "OnePlus Nord CE6 Lite",
            "Price": "32999",
            "Rating": 4.4
        },
        {
            "Product": "OnePlus Nord CE6 Lite 8GB",
            "Price": "29999",
            "Rating": 4.4
        },
        {
            "Product": "realme NARZO Power 5G Titan",
            "Price": "27999",
            "Rating": 3.8
        },
        {
            "Product": "Redmi 15 5G Purple 8GB",
            "Price": "25999",
            "Rating": 4.0
        },
        {
            "Product": "OnePlus NORD 5 12GB 256GB",
            "Price": "24999",
            "Rating": 3.4
        },
        {
            "Product": "iQOO Z11 Lite 4W 5G",
            "Price": "22999",
            "Rating": 3.3
        },
        {
            "Product": "NexTech A40 8GB 128GB",
            "Price": "21999",
            "Rating": 3.3
        },
        {
            "Product": "Motorola G57 Power 5G",
            "Price": "19999",
            "Rating": 4.3
        },
        {
            "Product": "OnePlus NORD CE6 Lite",
            "Price": "18999",
            "Rating": 3.3
        },
        {
            "Product": "Motorola G57 Power 5G Regatta",
            "Price": "17999",
            "Rating": 4.2
        },
        {
            "Product": "Redmi 15C 5G Midnight Black",
            "Price": "15999",
            "Rating": 4.1
        },
        {
            "Product": "realme NARZO 90X 5G",
            "Price": "14999",
            "Rating": 3.9
        },
        {
            "Product": "Samsung Galaxy M07 Mobile",
            "Price": "12999",
            "Rating": 4.2
        },
        {
            "Product": "Lava Bold N2",
            "Price": "9999",
            "Rating": 3.8
        },
        {
            "Product": "Lava Bold N2 Lite",
            "Price": "8999",
            "Rating": 3.5
        }
    ]


def get_amazon_products(search_query):

    url = (
        "https://www.amazon.in/s?k="
        + search_query.replace(" ", "+")
    )

    headers = {
        "User-Agent": (
            "Mozilla/5.0 (Windows NT 10.0; Win64; x64) "
            "AppleWebKit/537.36 (KHTML, like Gecko) "
            "Chrome/131.0.0.0 Safari/537.36"
        ),
        "Accept-Language": "en-IN,en;q=0.9",
        "Accept": (
            "text/html,application/xhtml+xml,"
            "application/xml;q=0.9,image/webp,*/*;q=0.8"
        )
    }

    print("Fetching data from Amazon...")

    try:

        response = requests.get(
            url,
            headers=headers,
            timeout=15
        )

        print("Status Code:", response.status_code)

        if response.status_code != 200:

            print("Amazon did not return product data.")
            print("Using sample product data...")

            return get_sample_products()

        soup = BeautifulSoup(
            response.text,
            "html.parser"
        )

        products = soup.select(
            'div[data-component-type="s-search-result"]'
        )

        products_data = []

        for product in products:

            # Product name
            name_tag = product.select_one(
                "h2 span"
            )

            if not name_tag:
                continue

            product_name = name_tag.get_text(
                strip=True
            )

            # Price
            price_tag = product.select_one(
                "span.a-price-whole"
            )

            if not price_tag:

                price_tag = product.select_one(
                    "span.a-offscreen"
                )

            if not price_tag:
                continue

            product_price = price_tag.get_text(
                strip=True
            )

            # Rating
            rating_tag = product.select_one(
                "span.a-icon-alt"
            )

            if rating_tag:

                rating = convert_rating_to_float(
                    rating_tag.get_text(
                        strip=True
                    )
                )

            else:

                rating = 0.0

            if convert_price_to_float(
                product_price
            ) > 0:

                products_data.append({
                    "Product": product_name,
                    "Price": product_price,
                    "Rating": rating
                })

        if not products_data:

            print(
                "No products extracted from Amazon."
            )

            print(
                "Using sample product data..."
            )

            return get_sample_products()

        # DESCENDING ORDER
        return sorted(
            products_data,
            key=lambda x: convert_price_to_float(
                x["Price"]
            ),
            reverse=True
        )[:20]

    except Exception as e:

        print("Error:", e)

        print(
            "Using sample product data..."
        )

        return get_sample_products()

search_query = input(
    "Enter product to search on Amazon: "
)

products = get_amazon_products(
    search_query
)


products = sorted(
    products,
    key=lambda x: convert_price_to_float(
        x["Price"]
    ),
    reverse=True
)


print("\nProducts Found:")
print("=" * 100)

for i, product in enumerate(
    products,
    start=1
):

    print(
        f"\nProduct: {product['Product']}"
    )

    print(
        f"Price: ₹{convert_price_to_float(product['Price']):,.2f}"
    )

    print(
        f"Rating: {product['Rating']}/5"
    )

    print("-" * 100)


product_names = [
    product["Product"][:30]
    for product in products
]

product_prices = [
    convert_price_to_float(
        product["Price"]
    )
    for product in products
]

product_ratings = [
    product["Rating"]
    for product in products
]

plt.figure(
    figsize=(12, 8)
)

bars = plt.barh(
    range(len(product_prices)),
    product_prices,
    color="skyblue"
)


# X-axis
plt.xlabel(
    "Price (₹)"
)


# Y-axis
plt.ylabel(
    "Product"
)


# Title
plt.title(
    f"Products and their Prices on Amazon "
    f"for {search_query.capitalize()} "
    f"(Descending Order)"
)

plt.yticks(
    range(len(product_names)),
    product_names
)

plt.gca().invert_yaxis()


max_price = max(
    product_prices
)

for bar, rating in zip(
    bars,
    product_ratings
):

    width = bar.get_width()

    plt.text(
        width + max_price * 0.01,
        bar.get_y() + bar.get_height() / 2,
        f"* {rating * 20:.1f}%",
        va="center",
        fontsize=8
    )

plt.tight_layout()

plt.show()

```

### Output:

<img width="978" height="703" alt="image" src="https://github.com/user-attachments/assets/b6cb2e09-1e76-4461-8311-ad8a380596e7" />

<img width="997" height="705" alt="image" src="https://github.com/user-attachments/assets/f77e142f-c4d0-44f6-b826-547eec3d5055" />

<img width="745" height="486" alt="image" src="https://github.com/user-attachments/assets/4f7daac3-28bb-4143-95fe-002b26e19fcc" />



### Result:

Thus, To perform Web Scraping on Amazon using (beautifulsoup) Python has been executed successfully.
