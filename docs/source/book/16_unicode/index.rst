.. raw:: latex

   \newpage

.. _unicode_index:

16. Unicode
============================

The programs we write are not isolated. They download data from the Internet, read and write data on disk, transmit data over the network.

So it’s very important to understand the difference between how a computer stores and transmits data and how that data is perceived by a person. We take the text, computer takes the bytes.

Python 3, respectively, has two concepts:

* text  - an immutable sequence of unicode characters. Type *string* (str) is used to store these characters
* data - an immutable sequence of bytes. Type *bytes* is used for storage

.. note::

    It is more correct to say that the text is an immutable sequence of codes (codepoints) Unicode.
    
.. toctree::
   :maxdepth: 1

   unicode_standard
   python_3_unicode
   python_3_convert
   convert_examples
   errors
   further_reading
