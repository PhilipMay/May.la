fastText
========

Help: fastText Commnand Line Tool
---------------------------------

::

   usage: fasttext <command> <args>

   The commands supported by fasttext are:

     supervised              train a supervised classifier
     quantize                quantize a model to reduce the memory usage
     test                    evaluate a supervised classifier
     test-label              print labels with precision and recall scores
     predict                 predict most likely labels
     predict-prob            predict most likely labels with probabilities
     skipgram                train a skipgram model
     cbow                    train a cbow model
     print-word-vectors      print word vectors given a trained model
     print-sentence-vectors  print sentence vectors given a trained model
     print-ngrams            print ngrams given a trained model and word
     nn                      query for nearest neighbors
     analogies               query for analogies
     dump                    dump arguments,dictionary,input/output vectors

::

   The following arguments are mandatory:
     -input              training file path
     -output             output file path

   The following arguments are optional:
     -verbose            verbosity level [2]

   The following arguments for the dictionary are optional:
     -minCount           minimal number of word occurences [5]
     -minCountLabel      minimal number of label occurences [0]
     -wordNgrams         max length of word ngram [1]
     -bucket             number of buckets [2000000]
     -minn               min length of char ngram [3]
     -maxn               max length of char ngram [6]
     -t                  sampling threshold [0.0001]
     -label              labels prefix [__label__]

   The following arguments for training are optional:
     -lr                 learning rate [0.05]
     -lrUpdateRate       change the rate of updates for the learning rate [100]
     -dim                size of word vectors [100]
     -ws                 size of the context window [5]
     -epoch              number of epochs [5]
     -neg                number of negatives sampled [5]
     -loss               loss function {ns, hs, softmax, one-vs-all} [ns]
     -thread             number of threads [12]
     -pretrainedVectors  pretrained word vectors for supervised learning []
     -saveOutput         whether output params should be saved [false]

   The following arguments for quantization are optional:
     -cutoff             number of words and ngrams to retain [0]
     -retrain            whether embeddings are finetuned if a cutoff is applied [false]
     -qnorm              whether the norm is quantized separately [false]
     -qout               whether the classifier is quantized [false]
     -dsub               size of each sub-vector [2]

Help: Python Usage
------------------

To load a model use ``load_model`` of ``fastText.FastText``. On how to
use it see code of ``class _FastText`` in ``FastText`` module.

Output of ``help(fastText.FastText)``:

::

   Help on module fastText.FastText in fastText:

   NAME
       fastText.FastText

   DESCRIPTION
       # Copyright [...]

   FUNCTIONS
       load_model(path)
           Load a model given a filepath and return a model object.

       tokenize(text)
           Given a string of text, tokenize it and return a list of tokens

       train_supervised(input, lr=0.1, dim=100, ws=5, epoch=5, minCount=1, 
                        minCountLabel=0, minn=0, maxn=0, neg=5, 
                        wordNgrams=1, loss='softmax', bucket=2000000, 
                        thread=7, lrUpdateRate=100, t=0.0001, 
                        label='__label__', verbose=2, pretrainedVectors='')
           Train a supervised model and return a model object.

           input must be a filepath. The input text does not need to be tokenized
           as per the tokenize function, but it must be preprocessed and encoded
           as UTF-8. You might want to consult standard preprocessing scripts such
           as tokenizer.perl mentioned here: http://www.statmt.org/wmt07/baseline.html

           The input file must must contain at least one label per line. For an
           example consult the example datasets which are part of the fastText
           repository such as the dataset pulled by classification-example.sh.

       train_unsupervised(input, model='skipgram', lr=0.05, dim=100, ws=5, epoch=5, 
                          minCount=5, minCountLabel=0, minn=3, maxn=6, neg=5, 
                          wordNgrams=1, loss='ns', bucket=2000000, thread=7, 
                          lrUpdateRate=100, t=0.0001, label='__label__', verbose=2, 
                          pretrainedVectors='')
           Train an unsupervised model and return a model object.

           input must be a filepath. The input text does not need to be tokenized
           as per the tokenize function, but it must be preprocessed and encoded
           as UTF-8. You might want to consult standard preprocessing scripts such
           as tokenizer.perl mentioned here: http://www.statmt.org/wmt07/baseline.html

           The input field must not contain any labels or use the specified label prefix
           unless it is ok for those words to be ignored. For an example consult the
           dataset pulled by the example script word-vector-example.sh, which is
           part of the fastText repository.

   DATA
       BOW = '<'
       EOS = '</s>'
       EOW = '>'
       absolute_import = _Feature((2, 5, 0, 'alpha', 1), (3, 0, 0, 'alpha', 0...
       division = _Feature((2, 2, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 8192...
       print_function = _Feature((2, 6, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0)...
       unicode_literals = _Feature((2, 6, 0, 'alpha', 2), (3, 0, 0, 'alpha', ...

   FILE
       [...]

Query for Analogies
-------------------

.. code:: bash

   $ fasttext analogies wiki-cbow-model-300.bin
   Loading model wiki-cbow-model-300.bin
   Query triplet (A - B + C)? Toyota Japan Deutschland
   Volkswagen 0.732376
   FAW-Volkswagen 0.707762
   Volkswagen-Fahrer 0.701164
   Volkswagen-Konzern 0.697515
   Volkswagen-Tochter 0.697494
   Volkswagen-Motor 0.695728
   Mercedes-Benz-Limousinen 0.690652
   Mercedes-Benz-Fahrzeugen 0.687446
   Volkswagen-Modelle 0.686741
   Volkswagenmotor 0.686598

How Tos
-------

-  obtain out-of-vocabulary vectors:
   https://fasttext.cc/docs/en/crawl-vectors.html#format

Links
-----

-  Main Website: https://fasttext.cc/
-  GitHub Repository: https://github.com/facebookresearch/fastText
-  Python PIP package for fastText:
   https://github.com/facebookresearch/fastText/tree/master/python
