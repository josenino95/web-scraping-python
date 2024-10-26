---
title: "Hello-Scraping"
teaching: 40
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you write a lesson using Markdown and `{sandpaper}`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain how to use markdown with The Carpentries Workbench
- Demonstrate how to include pieces of code, figures, and nested challenge blocks

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

This is part 2 of an Introduction to Web Scraping workshop we offered on February 2024. You can refer to those [workshop materials](https://ucsbcarpentry.github.io/2024-02-27-ucsb-webscraping/) to have a more gentle introduction to scraping using XPath and the `Scraper` Chrome extension.

We'll refresh some of the concepts covered there to have a practical understanding of how content/data is structured in a website. For that purpose, we'll see what Hypertext Markup Language (HTML) is and how it structures and formats the content using `tags`. From there, we'll use the BeautifulSoup library to parse the HTML content so we can easily search and access elements of the website we are interested in. Starting from basic examples, we'll move to scrape more complex, real-life websites.

## HTML quick overview

All websites have a Hypertext Markup Language (HTML) document behind them. The following text is HTML for a very simple website, with only three sentences. If you read it, can you imagine how that website looks?

```html
<!DOCTYPE html>
<html>
<head>
<title>Sample web page</title>
</head>
<body>
<h1 id="head1">h1 Header #1</h1>
<p>This is a paragraph tag</p>
<h2 id="subhead1">h2 Sub-header</h2>
<p>A new paragraph, now in the <b>sub-header</b></p>
<h1 id="head2">h1 Header #2</h1>
<p>
This other paragraph has two hyperlinks,
one to <a href="https://carpentries.org/">The Carpentries homepage</a>,
and another to the
<a href="https://carpentries.org/past_workshops/">past workshops</a> page.
</p>
</body>
</html>
```

Well, if you put that text in a file with a .html extension, the job of your web browser when opening the file will be to interpret that (markup) language and display a nicely formatted website.

![](fig/simple_website.PNG){alt="Screenshot of a simple website with the previews HTML"}

An HTML document is composed of elements, which can be identified by tags written inside angle brackets (`<` and `>`). For example, the HTML root element, which delimits the beginning and end of an HTML document, is identified by the `<html>` tag.

Most elements have both a opening and a closing tag, determining the span of the element. In the previous simple website, we see a head element that goes from the opening tag `<head>` up to the closing tag `</head>`. Given than an element can be inside another element, an HTML document has a tree structure, where every element is a node that can contain child nodes, like the following image shows.

![The Document Object Model (DOM) that represents an HTML document with a tree structure. Source: Wikipedia. Author: Birger Eriksson](https://upload.wikimedia.org/wikipedia/commons/5/5a/DOM-model.svg){alt="Screenshot of a simple website with the previews HTML"}

Finally, we can define or modify the behavior, appeareance, or functionality of an element by using attributes. Attributes are inside the opening tag, and consist of a name and a value, formatted as `name="value"`. For example, we can give an unique id to any element using the `id` attribute.

Here is a non-exhaustive list of elements you'll find in HTML and their purpose:

- `<hmtl>...</html>` The root, which contains the entirety of the document.
- `<head>...</head>` Contains metadata, for example, the title that the web browser displays.
- `<body>...</body>` The content that is going to be displayed.
- `<h1>...</h1>, <h2>...</h2>, <h3>...</h3>` Defines headers of level 1, 2, 3, etc.
- `<p>...</p>` A paragraph.
- `<a href="">...</a>` Creates a hyperlink, and we provide the destination URL with the `href` attribute.
- `<img src="" alt="">` Embedds an image, giving a source to the image with the `src` attribute and specifying alternate text with `alt`.
- `<table>...</table>, <th>...</th>, <tr>...</tr>, <td>...</td>` Defines a table, that as children will have a header (defined inside `th`), rows (defined inside `tr`), and a cell inside a row (as `td`).
- `<div>...</div>` Is used to group sections of HTML content.
- `<script>...</script>` Embeds or references JavaScript code.

To summarize, an *element*  is identified by *tags* , and we can assign properties to an element by using *attributes*. Knowing this about HTML will make our lifes easier when trying to get some specific data from a website.


## Parsing HTML with BeautifulSoup

Now that we know how a website is structured, we can start extracting information from it


This is a lesson created via The Carpentries Workbench. It is written in
[Pandoc-flavored Markdown](https://pandoc.org/MANUAL.txt) for static files and
[R Markdown][r-markdown] for dynamic files that can render code into output. 
Please refer to the [Introduction to The Carpentries 
Workbench](https://carpentries.github.io/sandpaper-docs/) for full documentation.

What you need to know is that there are three sections required for a valid
Carpentries lesson:

 1. `questions` are displayed at the beginning of the episode to prime the
    learner for the content.
 2. `objectives` are the learning objectives for an episode displayed with
    the questions.
 3. `keypoints` are displayed at the end of the episode to reinforce the
    objectives.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Can you do it?

What is the output of this command?

```r
paste("This", "new", "lesson", "looks", "good")
```

:::::::::::::::::::::::: solution 

## Output
 
```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::


## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution 

You can add a line with at least three colons and a `solution` tag.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Figures

You can use standard markdown for static figures with the following syntax:

`![optional caption that appears below the figure](figure url){alt='alt text for
accessibility purposes'}`

![You belong in The Carpentries!](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides": 
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::


## Math

One of our episodes contains $\LaTeX$ equations when describing how to create
dynamic reports with {knitr}, so we now use mathjax to describe this:

`$\alpha = \dfrac{1}{(1 - \beta)^2}$` becomes: $\alpha = \dfrac{1}{(1 - \beta)^2}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
