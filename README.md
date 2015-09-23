Shippable Documentation (This is now deprecated. Please make changes to docsv2)
=======================

Overview
--------
Shippable is a docker based continuous integration and deployment platform. This repository contains the documentation, which is open source to allow anyone to make quick changes if needed.

The source is under ``sources/`` in the
form of .rst files. These files use
[reStructuredText](http://docutils.sourceforge.net/rst.html)
formatting with [Sphinx](http://sphinx-doc.org/) extensions for
structure, cross-linking and indexing.

The HTML files are built and hosted on
[readthedocs.org](https://readthedocs.org/projects/shippable/), appearing
via proxy on [docs.shippable.com](http://docs.shippable.com/en/latest). The HTML files update
automatically after each change to the master.


Getting Started
---------------

To edit and test the docs, you'll need to install the Sphinx tool and
its dependencies. There are two main ways to install this tool:

### Native Installation

Install dependencies from `requirements.txt` file in your `docs`
directory:

* Linux: `pip install -r docs/requirements.txt`
* Mac OS X: `[sudo] pip-2.7 install -r docs/requirements.txt`

# Contributing

Working using GitHub's file editor
----------------------------------

For small changes and typos you might want to use
GitHub's built in file editor. It allows you to preview your changes
right online (though there can be some differences between GitHub
markdown and Sphinx RST). Just be careful not to create many commits.

Images
------

When you need to add images, try to make them as small as possible
(e.g. as gif). Usually images should go in the same directory as the
.rst file which references them, or in a subdirectory if one already
exists.

Notes
-----

Guides on using sphinx
----------------------
* To make links to certain sections create a link target like so:

  ```
    .. _hello_world:

    Hello world
    ===========

    This is a reference to :ref:`hello_world` and will work even if we
    move the target to another file or change the title of the section. 
  ```

  The ``_hello_world:`` will make it possible to link to this position
  (page and section heading) from all other pages. See the [Sphinx
  docs](http://sphinx-doc.org/markup/inline.html#role-ref) for more
  information and examples.

* Notes, warnings and alarms

  ```
    # a note (use when something is important)
    .. note::

    # a warning (orange)
    .. warning::

    # danger (red, use sparsely)
    .. danger::

* Code examples

  * Start typed commands with ``$ `` (dollar space) so that they 
    are easily differentiated from program output.

Manpages
--------

* To make the manpages, run ``make man``. Please note there is a bug
  in Sphinx 1.1.3 which makes this fail.  Upgrade to the latest version
  of Sphinx.
* Then preview the manpage by running ``man _build/man/shippable.1``,
  where ``_build/man/shippable.1`` is the path to the generated manfile
