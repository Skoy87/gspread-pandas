===============
Getting Started
===============

|PyPI version|

author: Diego Fernandez

Overview
========

A package to easily open an instance of a Google spreadsheet and
interact with worksheets through Pandas DataFrames.

Some key goals/features:

-  Nicely handle headers and indexes.
-  Run on Jupyter, headless server, and/or scripts
-  Allow storing different user credentials
-  Automatically handle token refreshes

Installation / Usage
====================

To install use pip:

::

    $ pip install gspread-pandas

Or clone the repo:

::

    $ git clone https://github.com/aiguofer/gspread-pandas.git
    $ python setup.py install

Before using, you will need to download Google client credentials for
your app.

Client Credentials
------------------

To allow a script to use Google Drive API we need to authenticate our
self towards Google. To do so, we need to create a project, describing
the tool and generate credentials. Please use your web browser and go to
`Google console <https://console.developers.google.com/>`__ and :

-  Choose **Create Project** in popup menu on the top.
-  A dialog box appears, so give your project a name and click on
   **Create** button.
-  On the left-side menu click on **API Manager**.
-  A table of available APIs is shown. Switch **Drive API** and click on
   **Enable API** button. Other APIs might be switched off, for our
   purpose.
-  On the left-side menu click on **Credentials**.
-  In section **OAuth consent screen** select your email address and
   give your product a name. Then click on **Save** button.
-  In section **Credentials** click on **Add credentials** and switch
   **OAuth 2.0 client ID**.
-  A dialog box **Create Cliend ID** appears. Select **Application
   type** item as **Other**.
-  Click on **Create** button.
-  Click on **Download JSON** icon on the right side of created **OAuth
   2.0 client IDs** and store the downloaded file on your file system.
   Please be aware, the file contains your private credentials, so take
   care of the file in the same way you care of your private SSH key;
   i.e. move downloaded JSON to ``~/.google/google_secret.json`` (or you
   can configure the directory and file name by directly calling
   ``gspread_pandas.conf.get_config``

Thanks to similar project
`df2gspread <https://github.com/maybelinot/df2gspread>`__ for this great
description of how to get the client credentials.

User Credentials
----------------

Once you have your client credentials, you can have multiple user
credentials stored in the same machine. This can be useful when you have
a shared server (for example with a Jupyter notebook server) with
multiple people that may want to use the library. The first parameter to
``Spread`` must be the key identifying a user's credentials. The first
time this is called for a specific key, you will have to authenticate
through a text based OAuth prompt; this makes it possible to run on a headless
server through ssh or through a Jupyter notebook. After this, the
credentials for that user will be stored (by default in ``~/.google/creds``)and the tokens will be
refreshed automatically any time the tool is used.

Users will only be able to interact with Spreadsheets that they have
access to.

Contributing
============

::

    $ git clone https://github.com/aiguofer/gspread-pandas.git && cd gspread-pandas
    $ pip install -e ".[dev]"

TBD

Example
=======

.. code:: python

    from __future__ import print_function
    import pandas as pd
    from gspread_pandas import Spread

    file_name = "http://www.ats.ucla.edu/stat/data/binary.csv"
    df = pd.read_csv(file_name)

    # 'Example Spreadsheet' needs to already exist and your user must have access to it
    spread = Spread('example_user', 'Example Spreadsheet')
    # This will ask to authenticate if you haven't done so before for 'example_user'

    # Display available worksheets
    spread.sheets

    # Save DataFrame to worksheet 'New Test Sheet', create it first if it doesn't exist
    spread.df_to_sheet(df, index=False, sheet='New Test Sheet', start_row=2, replace=True)
    spread.update_cells((1,1), (1,2), ['Created by:', spread.email])
    print(spread)
    # <gspread_pandas.client.Spread - User: '<example_user>@gmail.com', Spread: 'Example Spreadsheet', Sheet: 'New Test Sheet'>


.. |PyPI version| image:: https://badge.fury.io/py/gspread-pandas.svg
   :target: https://badge.fury.io/py/gspread-pandas