.. This document was generated by tools/gen-cpydiff.py

Syntax
======
Generated Mon 11 May 2020 17:55:38 UTC

Spaces
------

.. _cpydiff_syntax_spaces:

uPy requires spaces between literal numbers and keywords, CPy doesn't
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sample code::

    try:
        print(eval('1and 0'))
    except SyntaxError:
        print('Should have worked')
    try:
        print(eval('1or 0'))
    except SyntaxError:
        print('Should have worked')
    try:
        print(eval('1if 1else 0'))
    except SyntaxError:
        print('Should have worked')

+-------------+------------------------+
| CPy output: | uPy output:            |
+-------------+------------------------+
| ::          | ::                     |
|             |                        |
|     0       |     Should have worked |
|     1       |     Should have worked |
|     1       |     Should have worked |
+-------------+------------------------+

Unicode
-------

.. _cpydiff_syntax_unicode_nameesc:

Unicode name escapes are not implemented
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sample code::

    print("\N{LATIN SMALL LETTER A}")

+-------------+-----------------------------------------------+
| CPy output: | uPy output:                                   |
+-------------+-----------------------------------------------+
| ::          | ::                                            |
|             |                                               |
|     a       |     NotImplementedError: unicode name escapes |
+-------------+-----------------------------------------------+

