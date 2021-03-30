Split
=====

Split a file
------------

.. code:: bash

   split -d -b <size> <file_to_split> <file_to_split>.part-

The size argument is an integer and optional unit (example: 10K is
10*1024). Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,...
(powers of 1000).

Un-Split a file
---------------

.. code:: bash

   cat <file_to_split>-part-* > <file_to_split>
