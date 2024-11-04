---
title: Setup
---

In this workshop you will learn how to extract data from websites, what you'd call web scraping, using Python. In Episode 1 we begin by reviewing the structure of websites in HTML and how to retrieve information from it using your browser and the `BeautifulSoup` package. In Episode 2 we'll dive deep on how to get the HTML behind any website using the `requests` package and how to parse and find information with `BeautifulSoup`. At the end,you’ll learn about the differences between static and dynamic webpages, and how to scrape the latter with the `Selenium` package.

This workshop is designed for participants who already have a basic understanding of Python programming. In particular, it's best to know how to:

- Install and import packages and modules
- Use lists and dictionaries
- Use conditional statements (`if`, `else`, `elif`)
- Use `for` loops
- Calling functions, understanding parameters/arguments and return values

## Software Setup

Steps:

1. If you already have Anaconda, Jupyter Lab or Jupyter Notebooks installed in your computer, skip to step 2. Follow Miniforge's [download](https://github.com/conda-forge/miniforge?tab=readme-ov-file#download) and [installation](https://github.com/conda-forge/miniforge?tab=readme-ov-file#install) instructions for your respective operating system. If you are using a Windows machine, make sure you mark the option to "Add Miniforge3 to my PATH environment variable".
2. If you are using Mac or Linux, open the 'Terminal'. If you are using Windows, open the 'Command Prompt' or 'Miniforge Prompt'.
3. Activate the base conda environment by typing and running the code below to activate your environment.
~~~
conda activate
~~~
{: .source}

4. Install the necessary packages by running:
~~~
pip install 'requests' 'beautifulsoup4' 'selenium' 'webdriver-manager' 'pandas' 'tqdm' 'jupyterlab'
~~~
{: .source}

