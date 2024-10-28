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

We'll be scraping The Carpentries website, [https://carpentries.org/](https://carpentries.org/), and the list of upcoming and past workshop you can find in there. For that, first we'll load the `requests` package and then use the code `.get().text` to store the HTML document of the website.

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

The output from our previous code was truncated, as it is too long, but we can see that it is HTML and has some elements we didn't see in our previous simple example, like those identified with the `<meta>`, `<link>` and `<script>` tags.

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
