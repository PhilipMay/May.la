How-To use Python Pickle
========================

Pickle
------

.. code:: python

   import pickle

   my_data = [1, 5, 7, 8]

   with open('pickle_test.pkl', 'wb') as output_file:
       pickle.dump(my_data, output_file)

Un-Pickle
---------

.. code:: python

   import pickle

   with open('pickle_test.pkl', 'rb') as input_file:
       my_read_data = pickle.load(input_file)
       
   print(my_read_data)

