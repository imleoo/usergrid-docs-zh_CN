Usergrid Programmer's Guide

Documentation for the Usergrid project for Application Developers

Latest versions can be view online at:

http://usergrid.github.com/docs

We're using Sphinx (http://sphinx.pocoo.org/) as our documentation generation
system.  Sphinx is used by a number of Python projects, including the
Python.org project itself as well as Django.  Although it's built in Python
and has some specific features for documenting Python code, it's a general
purpose documentation generation system which is becoming increasingly popular
for generating this type of user documentation.

Sphinx uses reStructuredText (http://docutils.sourceforge.net/rst.html), which
is a plaintext markup language like Markdown or Textile.  Sphinx takes the rST
source documentation and generates an actual site with a table of contents
and navigation, using Jinja (http://jinja.pocoo.org/), which is a template
language that is derived from, and is a superset of, the template language
used by Django.  GitHub natively supports rST pages, so when you look at the
.rst source files in the project on GitHub, they're be rendered as HTML.  You
can also edit the .rst source pages directly in GitHub as well.

We're using an embedded custom template that was originally derived from the
Haiku theme (http://sphinx.pocoo.org/theming.html).  We copied the templates
from the theme into our source _templates directory and modified them there
rather than trying to extend the theme since we expect that over time as we
spend more effort customizing the look and feel of our documentation, that
we'll diverge significantly from the original theme templates.

Sphinx uses "domains" to support specific languages.  The initial domain was
for Python, with additional domains for C/C++, and Javascript added.  Custom
domains are also allowed.  The Usergrid documentation primarily uses the
Javascript domain as well as the HTTP domain for documenting  REST endpoints
(https://github.com/deceze/Sphinx-HTTP-domain).

Installation and Building
-------------------------

Install these two modules:

sudo easy_install -U Sphinx
sudo easy_install -U sphinx-http-domain
sudo easy_install -U Pygments

This will build the html website:

make html

The site is generated in build/html


