Tensorflow and Keras
====================

Tensorflow
----------

-  `TensorFlow Developers (Google
   Groups) <https://groups.google.com/a/tensorflow.org/forum/#!forum/developers>`__
-  `SIG Addons (Google
   Groups) <https://groups.google.com/a/tensorflow.org/forum/#!forum/addons>`__
-  `SIG Addons
   Notes <https://docs.google.com/document/d/1kxg5xIHWLY7EMdOJCdSGgaPu27a9YKpupUz2VTXqTJg/edit#heading=h.7ck4k2782ggg>`__
-  `Tensorflow (GitHub) <https://github.com/tensorflow/tensorflow>`__
-  `Tensorflow Addons (GitHub) <https://github.com/tensorflow/addons>`__

   -  `Issue: Testing Jupyter / Colab
      Notebooks <https://github.com/tensorflow/addons/issues/485>`__

Keras
-----

-  `SIG Keras Meetings (Google
   Groups) <https://groups.google.com/a/tensorflow.org/forum/#!forum/keras-meetings>`__
-  `Keras-users (Google
   Groups) <https://groups.google.com/forum/#!forum/keras-users>`__
-  `SIG Keras Meeting Agenda and
   Notes <https://docs.google.com/document/d/1yz2kwWsZ3wx-cObodb84a-LgHuxbTbxo53VI5yFqms8/edit#heading=h.xh0kqonf2i20>`__
-  `Keras API Special Interest Group
   (SIG) <https://github.com/keras-team/governance>`__

   -  `RFC for Keras Preprocessing
      redesign <https://github.com/keras-team/governance/pull/6>`__

Turn off GPU usage
------------------

GPU usage can be turned off this way:

.. code:: python

   import os
   os.environ["CUDA_VISIBLE_DEVICES"]="-1" 
