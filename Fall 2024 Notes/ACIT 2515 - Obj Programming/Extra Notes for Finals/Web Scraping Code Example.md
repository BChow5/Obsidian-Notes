### Book Web Scraping
```python
import re  # Regular expressions for pattern matching
import httpx  # HTTP client for sending requests
from bs4 import BeautifulSoup  # Library for parsing HTML

class Book:
    # Class variable - defines the fields of a book object
    FIELDS = ["title", "price", "available", "rating", "upc", "url"]

    def __init__(self, url):
        # Print a message to indicate a book is being processed
        print("--- BOOK ---", url)
        
        # Fetch the HTML page from the provided URL
        response = httpx.get(url)
        
        # Parse the HTML content using BeautifulSoup
        soup = BeautifulSoup(response.text, "html.parser")
        
        # Save the URL and the parsed HTML content as instance attributes
        self.url = url
        self.soup = soup
        
        # Extract the book title from the <h1> element
        title = soup.find("h1")
        if not title:  # Raise an error if the title is not found
            raise RuntimeError("Title not found")
        self.title = title.text

        # Extract the price from the first element with the class "price_color"
        price = soup.select(".product_main p.price_color")
        if not price:  # Raise an error if the price is not found
            raise RuntimeError("Price not found")
        # Convert the price text into a float (removing any non-numeric characters except '.')
        self.price = float("".join([let for let in price[0].text if let.isnumeric() or let == "."]))
        
        # Extract the availability text and find the number of available items
        avail = soup.select(".product_main p.availability")
        if not avail:  # Raise an error if availability information is not found
            raise RuntimeError("Availability not found")
        # Use a regex to capture the number of items available in stock
        self.available = int(re.search(r"In stock \((\d+) available\)", avail[0].text.strip()).group(1))

        # Extract the UPC (unique product code) from the table
        self.upc = soup.find("th", string="UPC").next_sibling.text

        # Convert the star rating (e.g., "Three" stars) into a numeric value
        _ratings = {"one": 1, "two": 2, "three": 3, "four": 4, "five": 5}  # Mapping of words to numbers
        self.rating = [
            _ratings[c.lower()]  # Look up the numeric value for the rating
            for c in soup.find("p", attrs={"class": "star-rating"}).attrs["class"]  # Check class attributes
            if c.lower() in _ratings  # Only include classes that represent ratings
        ].pop()  # Get the first (and only) numeric rating value

    def to_dict(self):
        # Converts the book object into a dictionary with field names as keys
        return {field: getattr(self, field) for field in self.FIELDS}

```

### Web Scraping Example
```python

import csv  # Module for working with CSV files
from urllib.parse import urljoin  # For building absolute URLs from relative ones

import httpx  # HTTP client for sending requests
from bs4 import BeautifulSoup  # Library for parsing HTML

from book import Book  # Import the Book class to create book objects

def scrape(url="https://books.toscrape.com/index.html"):
    """
    Scrapes book data from the given website URL and writes it to a CSV file.
    Args:
        url (str): The starting URL for scraping. Defaults to the main page of Books to Scrape.
    Returns:
        list: A list of Book objects created from the scraped data.
    """
    # A list to store all the book objects
    books = []

    # Open a CSV file for writing and set up the writer with the book fields
    out = open("books.csv", "w", newline="")
    writer = csv.DictWriter(out, fieldnames=Book.FIELDS)
    writer.writeheader()  # Write the header row (field names)

    keep_going = True  # Flag to control the scraping loop
    while keep_going:
        # Main scraping loop for the current page
        print("--- INDEX PAGE ---", url)  # Log the current page being scraped
        
        # Send a GET request to the current URL
        response = httpx.get(url)
        
        # Parse the HTML response using BeautifulSoup
        soup = BeautifulSoup(response.text, "html.parser")
        
        # Find all book links on the page
        for book in soup.select(".product_pod h3 a"):
            # Build the absolute URL for the book's detail page
            book_url = urljoin(url, book.attrs["href"])
            
            # Create a Book object by scraping the detail page
            book_obj = Book(book_url)
            
            # Add the Book object to the books list
            books.append(book_obj)
            
            # Write the book's data as a row in the CSV file
            writer.writerow(book_obj.to_dict())
        
        # Find the link to the next page (if any)
        next_link = soup.select("li.next a")
        
        if not next_link or len(next_link) == 0:
            # If no "Next" link is found, stop the loop
            keep_going = False
        else:
            # Build the absolute URL for the next page and continue the loop
            url = urljoin(url, next_link[0].attrs["href"])
    
    # Close the CSV file after finishing the loop
    out.close()
    
    # Return the list of scraped Book objects
    return books


if __name__ == "__main__":
    # Run the scraper and save the scraped books to a variable
    books = scrape()

```