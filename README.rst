Software Used
=============

This tutorial is written with Sphinx (http://sphinx.pocoo.org/).

Dictionary Overview
===================

* **base:** The makefile and readme are located at the base.

* **build:** Here goes all rendered output. Usually there will be sub-folders for different formates, e.g.: *html*, *latex* and *doctrees*

* **source:** The actual source-code is located here. It is structured as a standard sphinx - project.

* **extra-documents**: This folder is reserved for additional documents. Currently it holds some graphviz-dot diagrams that show the structure of the example CouchApp.


Build instructions
==================

Before building **always** make do ``make clean``. Sphinx does not overwrite used images or file-attachments but adds an incremet to their name. This leads to wrongly named files and unnecessary images.

HTML Build
----------

To make a html-build just enter ``make html``. You will find the complete tutorial rendered in html in the sub-folder ``build/html``. Open it with the file ``index.html``.

LateX Build
-----------

To generate a very nicely formatted latex (book) version of the tutorial, enter ``make latex``. A latex document will be generated inside ``build/latex``. You can open the *\*.tex* file in your favorite latex editor and render it or just use pdflaex.
