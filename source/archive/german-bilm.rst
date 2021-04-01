Training of German Word Embedding for NLP
=========================================

Step by step
------------

#. Download Wikipedia dump: https://dumps.wikimedia.org/

   #. use a mirror: https://dumps.wikimedia.org/mirrors.html
   #. I use the latest dump from german wikipedia:
      http://ftp.acc.umu.se/mirror/wikimedia.org/dumps/dewiki/
   #. ``dewiki-20190520-pages-articles.xml.bz2`` at the moment

#. Use wikiextractor: https://github.com/attardi/wikiextractor

   #. ``<nowiki>python WikiExtractor.py -o data/we_output --processes 8 data/dewiki-20190520-pages-articles.xml.bz2</nowiki>``

WikiExtractor.py
----------------

-  Um den Dump auch mit Talk Seiten zu bekommen den Dump mit "meta" im
   Namen laden.
-  ``<nowiki>./WikiExtractor.py -o output3 --processes 8 ../dewiki-20181001-pages-meta-current.xml.bz2</nowiki>``
-  Datenmenge nach erfolgreicher Extraktion: 5.2 GB

Code Anpassung um auch Disskusionen anzugeben
---------------------------------------------

Das Script von
`Wikiextractor <https://github.com/attardi/wikiextractor>`__ wird
angepasst:

.. code:: bash

   def keepPage(ns, page):
       if ns != '0' and ns != '1': # Aritcle and Talk
           print('skipped ns:', ns)
           return False
       # remove disambig pages if desired
       if options.filter_disambig_pages:
           for line in page:
               if filter_disambig_page_pattern.match(line):
                   return False
       return True

Nutzung von BILM
----------------

-  Kommando:
   ``<nowiki>python bin/train_elmo.py --train_prefix='big-set/*' --vocab_file='vocab-2016-09-10.txt' --save_dir='output_dir'</nowiki>``

Verhindern, dass der gesamte Speicher genutzt wird
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  durch ``config.gpu_options.allow_growth = True`` getestet wie viel
   Speicher genutzt wird: 8583 MiB
-  dann durch
   ``config.gpu_options.per_process_gpu_memory_fraction = 0.75`` Sper
   Verbrauch auf 8771 MiB hart begrenzt
-  siehe auch hier:
   https://www.tensorflow.org/guide/using_gpu#allowing_gpu_memory_growth
-  Codeänderungen waren in folgenden Dateien notwendig:
   ``less bilm/training.py`` und ``less bilm/model.py``
-  die Codestellen können durch eine Volltextsuche nach ``Session``
   gefunden werden
-  Beispiel:

.. code:: python

       tf_config = tf.ConfigProto(allow_soft_placement=True)
       tf_config.gpu_options.per_process_gpu_memory_fraction = 0.75

       with tf.Session(config=tf_config) as sess:
           sess.run(init)

Links
-----

-  Tool: https://github.com/attardi/wikiextractor
-  Texte: http://www.statmt.org/wmt14/training-monolingual-news-crawl/
-  http://www.statmt.org/
-  https://github.com/ciprian-chelba/1-billion-word-language-modeling-benchmark

Tools
-----

-  split files by line:
   ``split -l <split_after_n_lines> <filename_to_split>``
-  shuffle files by line: ``shuf <input_file> -o <output_file>``
-  count lines in file: ``wc -l <filename>``

BILM Vorgehen / Erkenntnisse
----------------------------

-  Checkpointing

   -  Checkpoints scheinen immer nur die letzten 2 gespeichert werden
   -  alle 1250 batches wird gespeichert
   -  max_to_keep=2 auf None setzen - siehe hier:
      https://www.tensorflow.org/api_docs/python/tf/train/Saver
   -  siehe hier:
      https://github.com/allenai/bilm-tf/blob/master/bilm/training.py#L755
   -  Beim Restart folgende Warnung bekommen:

::

   2018-10-26 15:39:31.112162: W tensorflow/core/common_runtime/bfc_allocator.cc:215]
   Allocator (GPU_0_bfc) ran out of memory trying to allocate 2.06GiB. The caller
   indicates that this is not a failure, but may mean that there could be performance
   gains if more memory were available.

Perplexity testen
-----------------

Kleines Experiment mit nur wenigen "Übungsdaten"

-  ``<nowiki>python bin/run_test.py --test_prefix='val.txt' --vocab_file='vocab_val.txt' --save_dir='output'</nowiki>``
-  nach 2 Epochen: PERPLEXITY = 8069.869
-  ``<nowiki>python bin/restart.py --train_prefix='input_train/*' --vocab_file='vocab_train.txt' --save_dir='output'</nowiki>``
-  nach 2 weiteren Epochen: AVERAGE PERPLEXITY = 10405.855
-  nach 2 weiteren Epochen: AVERAGE PERPLEXITY = 14552.747
-  Ergebnis: schnelles Overfitting

Perplexity at Training
----------------------

We have overfitting at epoch number 15 - so we are done with training.

.. image:: /_static/img/perplexity-german-bilm.png
