## This is a strange bug that I have tried to narrow down as much as I can

Quick summary:

A combination of a number of quite standard elements results in infinite recursion within
a check of `isinstance`.

In all, I have a 12 line (including the test case) example showing it off with only pytest as a dependency.


I have only gotten the bug to present itself:
* On Windows
* In a `weakref.finalize` call
* In a relative import
* When `typing` is imported
* Using `pytest` via `setup.py test`
* only if the test script is *not* in the module
* Python 3.6, 3.7 (3.5 complains about the relative import without the parent module being loades)
    - Have not tested on 2.7
