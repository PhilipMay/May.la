Read and write to ZIP file with Python
======================================

also see: https://docs.python.org/3/library/zipfile.html

Daten schreiben
---------------

.. code:: python

   import zipfile

   # open ZipFile with 'x' option so we do not overwrite a zip-file but get an exception
   with zipfile.ZipFile('my_new_zip_file.zip', 'x', zipfile.ZIP_DEFLATED) as myzip:
       with myzip.open('my_new_file_inside_the_new_zip_file.txt', 'w') as myfile:
           for x in range(10000):
               str_to_write = 'My new Text Line number ' + str(x) + '.\n'
               myfile.write(str_to_write.encode())

Daten lesen
-----------

Die Daten komplett lesen:

.. code:: python

   import zipfile

   # open ZipFile with 'r' option so we only read the data
   with zipfile.ZipFile('my_new_zip_file.zip', 'r', zipfile.ZIP_DEFLATED) as myzip:
       with myzip.open('my_new_file_inside_the_new_zip_file.txt', 'r') as myfile:
           data = myfile.read()
           print(data.decode())

Die Daten zeilenbasiert lesen:

.. code:: python

   import zipfile

   # open ZipFile with 'r' option so we only read the data
   with zipfile.ZipFile('my_new_zip_file.zip', 'r', zipfile.ZIP_DEFLATED) as myzip:
       with myzip.open('my_new_file_inside_the_new_zip_file.txt', 'r') as myfile:
           for line in myfile.readlines():
               print(line.decode())
