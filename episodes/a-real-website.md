---
title: "Scraping a real website"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you write a lesson using Markdown and `{sandpaper}`?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain how to use markdown with The Carpentries Workbench
- Demonstrate how to include pieces of code, figures, and nested challenge blocks

::::::::::::::::::::::::::::::::::::::::::::::::

## A "Requests" to a website

In the previous episode we used a simple HTML document, not an actual website. Now that we move to more real, complex escenario, we need to add another package to our toolbox, the `requests` package. For the purpose of this web scraping lesson, we will only use `requests` to get the HTML behind a website. However, there's a lot of extra functionality that we are not covering but you can find in the [Requests package documentation](https://requests.readthedocs.io/en/latest/).

We'll be scraping The Carpentries website, [https://carpentries.org/](https://carpentries.org/), and specifically, the list of upcoming and past workshop you can find in there. For that, first we'll load the `requests` package and then use the code `.get().text` to store the HTML document of the website.

```python
import requests
url = 'https://carpentries.org/'
req = requests.get(url).text
print(req)
```

```output
<!doctype html>
<html class="no-js" lang="en">
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>The Carpentries</title>

    <link rel="stylesheet" type="text/css" href="https://carpentries.org/assets/css/styles_feeling_responsive.css">

  

	<script src="https://carpentries.org/assets/js/modernizr.min.js"></script>

        <!-- matomo -->
        <script src="https://carpentries.org/assets/js/matomo-analytics.js"></script>

        <link href="https://fonts.googleapis.com/css?family=Lato:400,400i,700,700i|Roboto:400,400i,700,700i&display=swap" rel="stylesheet">

	<!-- Search Engine Optimization -->
	<meta name="description" content="The Carpentries is a fiscally sponsored project of Community Initiatives, a registered 501(c)3 non-profit organisation based in California, USA. We are a global community teaching foundational computational and data science skills to researchers in academia, industry and government.">
	
...
</body>
</html>
```

The output from our previous code was truncated, as it is too long, but we can see it is HTML and has some elements we didn't see in the example of the previous episode, like those identified with the `<meta>`, `<link>` and `<script>` tags.

There's another way to see the HTML document behind a website, directly from your web browser. Using Google Chrome, you can right-click in any part of the website (on a Mac, press and hold the Control key in your keyboard while you click), and from the pop-up menu, click 'View page source', as the next image shows. If the 'View page source' option didn't appear for you, try clicking in another part of the website. A new tab will open with the HTML document for the website you were in.

![](fig/view_page_source.png){alt="A screenshot of The Carpentries homepage in the Google Chrome web browser, showing how to View page source"}

In the HTML page source on your browser you can scroll down and look for the second-level header (`<h2>`) with the text "Upcoming Carpentries Workshops". Or more easily, you can use the Find Bar (Ctrl + F on Windows and Command + F on Mac) to search for "Upcoming Carpentries Workshops". Just right down of that header we have the table element we are interested in, which starts with the opening tag `<table class="table table-striped" style="width: 100%;">`. Inside that element we see nested different elements familiar for a table, the rows (`<tr>`) and the cells for each row (`<td>`), and additionally the image (`<img>`) and hyperlink (`<a>`) elements.

Now, going back to our coding, we left off on getting the HTML behind the website using `requests`, and stored it on the variable called `req`. From here we can proceed with BeautifulSoup as we learned in the previous episode, using the `BeautifulSoup()` function to parse our HTML, as the following code block shows. With the parsed document, we can use the `.find()` or `find_all()` methods to find the table element.

```python
soup = BeautifulSoup(req, 'html.parser')
tables_by_tag = soup.find_all('table')
print("Number of table elements found: ", len(tables_by_tag))
print("Printing only the first 1000 characters of the table element: \n", str(tables_by_tag[0])[0:1000])
```

```output
Number of table elements found:  1
Printing only the first 1000 characters of the table element:
 <table class="table table-striped" style="width: 100%;">
<tr>
<td>
<img alt="swc logo" class="flags" height="24" src="https://carpentries.org/assets/img/logos/swc.svg" title="swc workshop" width="24">
</img></td>
<td>
<img alt="mx" class="flags" src="https://carpentries.org/assets/img/flags/24/mx.png" title="MX">
<img alt="globe image" class="flags" src="https://carpentries.org/assets/img/flags/24/w3.png" title="Online">
<a href="https://galn3x.github.io/-2024-10-28-Metagenomics-online/">Nodo Nacional de BioinformÃ¡tica UNAM</a>
<br>
<b>Instructors:</b> CÃ©sar Aguilar, Diana Oaxaca, Nelly Selem-Mojica
      
      
          <br>
<b>Helpers:</b> Andreas Chavez, JosÃ© Manuel Villalobos Escobedo, Aaron Espinosa Jaime, AndrÃ©s Arredondo, Mirna VÃ¡zquez Rosas-Landa, David Alberto GarcÃ­a-Estrada
      
	</br></br></img></img></td>
<td>
		Oct 28 - Oct 31, 2024
	</td>
</tr>
<tr>
<td>
<img alt="dc logo" class="flags" height="24" src="https://carpentries.org/assets/img/logos/dc.svg" title="dc 
```

We can see that there was found only one table element in the entire HTML, which corresponds to the table we are looking for. The output you see in the previous code block will be different from what you have in your computer, as the data in the upcoming workshops table is continously being updated.

Besides searching elements using tags, sometimes it will be useful to search using attributes, like `id` or `class`. For example, we can see the table element has a class attribute with two values "table table-striped", which identifies all possible elements with similar styling. Therefore, we could have the same result than before using the `class_` argument on the `.find_all()` method as follows.

```python
tables_by_class = soup.find_all(class_="table table-striped")
```

Now that we know there is only onw table

## Navigating the tree





::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
