Unicode standard
---------------

Unicode is a standard that describes the representation and encoding of almost
all languages and other characters.

A few facts about Unicode:

* version 12.1 (May 2019) describes 137 994 codes
* each code is a number that corresponds to a certain character
* standard also defines the encoding - the way of representing the symbol code in bytes

Each character in Unicode has a specific code. This is a number that is usually
written as follows: ``U+0073``, where 0073 - hexadecimal digits.
Apart from the code, each symbol has its own unique name. For example,
letter "s" corresponds to code ``U+0073`` and the name "LATIN SMALL LETTER S".

Examples of codes, names and corresponding symbols:

-  ``U+0073``, "LATIN SMALL LETTER S" - s
-  ``U+00F6``, "LATIN SMALL LETTER O WITH DIAERESIS" - ö
-  ``U+1F383``, "JACK-O-LANTERN" - 🎃
-  ``U+2615``, "HOT BEVERAGE" - ☕
-  ``U+1f600``, "GRINNING FACE" - 😀

Encodings
~~~~~~~~~

Encodings allow to write character code in bytes.

Unicode supports several encodings:

* UTF-8 
* UTF-16 
* UTF-32

One of the most popular encoding to date is UTF-8. This encoding uses a variable
number of bytes to write Unicode characters.

Examples of Unicode characters and their representation in bytes in UTF-8 encoding:

* H - ``48`` 
* i - ``69`` 
* 🛀 - ``01 f6 c0`` 
* 🚀 - ``01 f6 80`` 
* ☃ - ``26 03``
