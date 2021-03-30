Creating a big German Dataset as a Reference for NLP Experiments
================================================================

The goal is to supply a big german dataset as a reference for NLP
experiments. This way different deep learning networks and techniques
like preprocessing can be compared.

The steps to create it are described here below. The code can be found
in the GitHub project
`de-nlp-train-set <https://github.com/PhilipMay/de-nlp-train-set>`__.
The result (full dataset) can be downloaded here:
https://eniak.de/download/nlp-train-set/text-data.zip

The resulting dataset contains 533,842 training samples and 60,000 test
samples.

This is a binary classification task with the following two labels:

-  gesch for "Geschichte" - the german word for history
-  gesell for "Gesellschaft" - the german word for society

The texts belong to exactly one of these categories and have to be
classified.

It contains the following 4 directories:

-  gesch_train - with 266,921 training samples for category "gesch"
-  gesell_train - with 266,921 training samples for category "gesell"
-  gesch_test - with 30,000 test samples for category "gesch"
-  gesell_test - with 30,000 test samples for category "gesell"

The filename is the id of the wikipedia article. It can be viewed when
you enter https://de.wikipedia.org/wiki?curid=10 and replace the suffix
number of the URL with the file name number.

Step by Step guide how this dataset has been created
----------------------------------------------------

Step 1 - Get Labels from Wikipedia
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here we use the tool `PetScan <https://petscan.wmflabs.org/>`__. You can
use it to create and download a list of all wikipedia articles that
belong to a certain category.

#. open `PetScan <https://petscan.wmflabs.org/>`__
#. set Language to ``de``
#. set Depth to ``30``
#. enter ``Geschichte`` to Categories
#. switch to tab "Output"
#. at Format set TSV
#. click ``Do it!``
#. if you get a timeout try again, maybe at an other daytime or with
   lower Dapth
#. do this again with ``Gesellschaft‎`` in Categories

The downloaded files can be found here on GitHub:
https://github.com/PhilipMay/de-nlp-train-set/tree/master/data/TSV

Step 2 - Get Data from Wikipedia
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To download all data from the German wikipedia open `Mirrors of XML
dumps, images and other
data <https://dumps.wikimedia.org/mirrors.html>`__ and select a mirror.
Go to ``dumps`` and ``dewiki``. There select the most recent date and
download the file called ``dewiki-<the_date>-pages-articles.xml.bz2``

Step 3 - Cleaning Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To clean the German wikipedia dump we use
`Wikiextractor <https://github.com/attardi/wikiextractor>`__. Just
download it.

Now we call it with the following additional parameters:

-  ``-b 10G`` this is to have just one output file
-  ``--filter_disambig_pages`` to remove disabmiguation pages
-  ``--min_text_length 300`` to remove short pages

The full call looks like this:
``./WikiExtractor.py -o . -b 10G –min_text_length 300 –filter_disambig_pages <some_path>/dewiki-<the_date>-pages-articles.xml.bz2``

Step 4 - Merging Labels
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now the labels from the two TSV files are merged. Some articles belong
to both categories (Geschichte and Gesellschaft). These articles are
removed. This is done in the jupyter notebook called ``step_1.ipynb`` in
the `de-nlp-train-set <https://github.com/PhilipMay/de-nlp-train-set>`__
GitHub project.

Step 5 - Mark Articles that are not available
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this step we check the merged labels and mark those that are not
available in our data file. The most frequent reason is that the article
was too short and was removed by the ``–min_text_length 300`` option of
the ``WikiExtractor`` call.

This is done in the jupyter notebook called ``step_2.ipynb`` in the
`de-nlp-train-set <https://github.com/PhilipMay/de-nlp-train-set>`__
GitHub project.

Step 6 - Select Labels for Train and Test Sets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this step the labels are selected for train and test data sets. We do
this in the following steps:

#. select all Geschichte tabels that are available
#. shuffle these labels
#. select all Gesellschaft tabels that are available
#. shuffle these labels
#. select as many Gesellschaft labels as we have Geschichte labels so
   the dataset is balanced
#. split Gesellschaft and Geschichte to 30,000 test labels each and the
   rest for training
#. save the labels to

   #. Gesellschaft train
   #. Gesellschaft test
   #. Geschichte train
   #. Geschichte test

This is done in the jupyter notebook called ``step_3.ipynb`` in the
`de-nlp-train-set <https://github.com/PhilipMay/de-nlp-train-set>`__
GitHub project.

Step 7 - Read Labels from Datafile and save Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Read the data from cleaned German wiki data file from step 3 and save
them in different directories by label. This is done in the jupyter
notebook called ``step_4.ipynb`` in the
`de-nlp-train-set <https://github.com/PhilipMay/de-nlp-train-set>`__
GitHub project.

List of possible other Categories
----------------------------------

see: https://de.wikipedia.org/wiki/Kategorie:!Hauptkategorie

-  Geschichte
-  Gesellschaft‎
-  Kommunikation und Medien
-  Kunst und Kultur
-  Naturwissenschaft und Technik
-  Personen
-  Politik
-  Religion
-  Sport
-  Umwelt und Natur
-  Wirtschaft
