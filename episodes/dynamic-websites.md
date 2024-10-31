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


::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
