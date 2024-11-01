---
title: "Dynamic websites"
teaching: 30
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you write a lesson using Markdown and `{sandpaper}`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain how to use markdown with The Carpentries Workbench
- Demonstrate how to include pieces of code, figures, and nested challenge blocks

::::::::::::::::::::::::::::::::::::::::::::::::

## You see it but the HTML doesn't have it!

Visit the following webpage created by Hartley Brody for the purpose of learning and practicing web scraping: [https://www.scrapethissite.com/pages/ajax-javascript/](https://www.scrapethissite.com/pages/ajax-javascript/) (read first the [terms of use](https://www.scrapethissite.com/faq/)). Select "2015" to see that year's Oscar winning films. Now look at the HTML behind it as we've learned, using the "View page source" tool on your browser or in Python using the requests and BeautifulSoup packages. Can you find in any place of the HTML the best picture winner "Spotlight"? Can you find any other of the movies or the data from the table? If you can't, how could you scrape this page?

When you explore a page like this, you'll notice that the movie data, including the title "Spotlight," isn’t actually in the initial HTML source. This is because the website uses JavaScript to load the information dynamically. JavaScript is a programming language that runs in your browser, allowing websites to fetch, process, and display content on the fly, often based on user actions like clicking a button. When you select "2015", the browser executes JavaScript (called by one of the `<script>` elements you see in the HTML) to retrieve the relevant movie information from the web server and build a new HTML document with actual information in the table. This makes the page feel more interactive, but it also means the initial HTML you see doesn’t contain the movie data itself.

Let’s explore a new way to view HTML elements in your browser to better understand the differences in an HTML document before and after JavaScript is executed. On the Oscar winners page you just visited, right-click (or Control key + Click on Mac) and select "Inspect" from the pop-up menu, as shown in the image below. This opens DevTools on the side of your browser, offering a range of features for inspecting, debugging, and analyzing web pages in real-time. For this workshop, however, we’ll focus on just the "Elements" tab. In the "Elements" tab, you’ll see an HTML document that actually includes the table element, unlike what you saw in "View Page Source". This difference is because "View Page Source" displays the original HTML, before any JavaScript is run, while "Inspect" reveals the HTML as it appears after JavaScript has executed.

![](fig/inspect.png){alt="A screenshot of Google Chrome web browser, showing how to use Inspect from the Chrome DevTools"}

As the `requests` package retrieves the source HTML, we need a different approach to scrape these types of websites. For this, we will use the `Selenium` package. But don't forget about the "Inspect" tool we just learned, it will be handy when scraping.

## Using Selenium to scrape dynamic websites

[Selenium](https://www.selenium.dev/) is an open source project for web browser automation. It will be useful for our scraping tasks as it will act as a real user interacting with a webpage in a browser. This way, Selenium will render pages in a browser environment, allowing JavaScript to load dynamic content, and therefore giving us access to the website HTML after JavaScript has executed. Additionally, this package simulates real user interactions like filling in text boxes, clicking, scrolling, or selecting drop-down menus, which will be useful when we scrape dynamic websites.

To use it, we'll load "webdriver" and "By" from the `selenium` package. webdriver open or simulate a web browser, interacting with it based on the instructions we give. By will allow us to specify how we will select a given element in the HTML, by tag (using "By.TAG_NAME") or by attributes like class ("By.CLASS_NAME"), id ("By.ID"), or name ("By.NAME"). We will also load the other packages we used in the previous episode.

```python
from bs4 import BeautifulSoup
import pandas as pd
from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By
```

Selenium can simulate multiple multiple browsers like Chrome, Firefox, Safari, etc. For now, we'll use Chrome. When you run the following line of code, you'll notice that a Google Chrome window opens up. Don't close it, as this is how Selenium interacts with the browser. Later we'll see how to do *headless* browser interactions, headless meaning that browser interactions will happen in the background, without opening a new browser window or user interface.

```python
driver = webdriver.Chrome()
```

Now, to tell the browser to visit our Oscar winners page, use the `.get()` method on the `driver` object we just created.

```python
driver.get("https://www.scrapethissite.com/pages/ajax-javascript/")
```

How can we direct Selenium to click the text "2015" for the table of that year tho show up? First, we need to find that element, in a similar way to how we find elements with BeautifulSoup. Just like we used `.find()` in BeautifulSoup to find the first element that matched the specified criteria, in Selenium we have `.find_element()`. Likewise, as we used `.find_all()` in BeautifulSoup to return a list of all coincidences for our search criteria, we can use `.find_elements()` in Selenium. But the syntax of how we specify the search paramenters will be a little different.

If we wanted to search a table element that has the `<table>` tag, we would run `driver.find_element(by=By.TAG_NAME, value="table")`. If we wanted to search an element with a specific value in the "class" attribute, for example an element like `<tr class="film">` we would run `driver.find_element(by=By.CLASS_NAME, value="film")`. To know what element we need to click to open "2015" table of Oscar winners we can use the "Inspect" tool (remember you can do this in Google Chrome by pointing your mouse over the "2015" value, make a right-click, and select "Inspect" from the pop-up menu). In the DevTools window, you'll see element `<a href="#" class="year-link" id="2015">2015</a>`. As the ID attribute is unique for only one element in the HTML, we can directly select the element by this attribute using the code you'll find after the following image.

![](fig/inspect_element.png){alt="A screenshot of Google Chrome web browser, showing how to search a specific element by using Inspect from the Chrome DevTools"}


```python
button_2015 = driver.find_element(by=By.ID, value="2015")
```

We've located the hyperlink element we want to click to get the table for that year, and on that element we will use the `.click()` method to interact with it. As the table takes a couple of seconds to load, we will also use the `sleep()` function from the "time" module to wait will the JavaScript runs and the table loads. Then, we use `driver.page_source` for Selenium to get the HTML document from the website, and we store it in a variable called `html_2015`. Finally, we close the web browser that Selenium was using with `driver.quit()`.

```python
button_2015.click()
sleep(3)
html_2015 = driver.page_source
driver.quit()
```

Importantly, the HTML document we stored in `html_2015` **is the HTML after the dynamic content loaded**, so it will contain the table values for 2015 that weren't there originally and that we wouldn't be able to see if we had used the `requests` package instead.

We could continue using Selenium and its `.find_element()` and `.find_elements()` methods to to extract our data of interest. But instead, we will use BeautifulSoup to parse the HTML and find elements, as we already have some practice using it. If we search for the first element with class attribute equal to "film-title" and return the text inside it, we see that this HTML has the "Spotlight" movie.

```python
soup = BeautifulSoup(html_2015, 'html.parser')
print(soup.find(class_='film').prettify())
```
```python
<tr class="film">
 <td class="film-title">
  Spotlight
 </td>
 <td class="film-nominations">
  6
 </td>
 <td class="film-awards">
  2
 </td>
 <td class="film-best-picture">
  <i class="glyphicon glyphicon-flag">
  </i>
 </td>
</tr>
```

# The scraping pipeline

By now, you've learned about the core tools for web scraping: requests, BeautifulSoup, and Selenium. These three tools form a versatile pipeline for almost any web scraping task. When starting a new scraping project, there are several important steps to follow that will help ensure you capture the data you need.

The first step is **understanding the website structure**. Every website is different and structures data in its own particular way. Spend some time exploring the site and identifying the HTML elements that contain the information you want. Next, **determine if the content is static or dynamic**. Static content can be directly accessed from the HTML source code using requests and BeautifulSoup, while dynamic content often requires Selenium to load JavaScript on the page before BeautifulSoup can parse it.

Once you know how the website presents its data, **start building your pipeline**. If the content is static, make a `requests` call to get the HTML document, and use `BeautifulSoup` to locate and extract the necessary elements. If the content is dynamic, use `Selenium` to load the page fully, perform any interactions (like clicking or scrolling), and then pass the rendered HTML to `BeautifulSoup` for parsing and extracing the necessary elements. Finally, **format and store the data** in a structured way that's useful for your specific project and that makes it easy to analyse later.

This scraping pipeline helps break down complex scraping tasks into manageable steps and allows you to adapt the tools based on the website’s unique features. With practice, you’ll be able to efficiently combine these tools to extract valuable data from almost any website.


::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
