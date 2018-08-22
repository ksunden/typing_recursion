## This is a strange bug that I have tried to narrow down as much as I can

Quick summary:

A combination of a number of quite standard elements results in infinite recursion within
a check of `isinstance`.

In all, I have a 12 line (including the test case) example showing it off with only pytest as a dependency.


I have only gotten the bug to present itself:
* In a `weakref.finalize` call
* In a relative import
* When `typing` is imported
* Using `pytest` via `setup.py test`
* only if the test script is *not* in the module


|       | 3.5.5             | 3.6.(5\|6)        | 3.7.0             |
|-------|-------------------|-------------------|-------------------|
|Linux  | :x:               | :x:  (both)       | :heavy_check_mark:|
|MacOS  |                   | :x:     (5)       |                   |
|Windows| other error       | :x:     (6)       | :x:               |

:x: Bug presents

:heavy_check_mark: Bug does not present

\<empty\> Not yet tested


Note that on windows python 3.5, an error about not being able to do a relative import because the parent module is not loaded is thrown.
This is odd, since the function doing the import is in the parent module.
This leads me to believe something is going on with modules being unloaded before the weakref call is completed.
Though I am unsure how such behavior would result in the bug I'm actually trying to produce.


A fuller history of all my zeroing in on here can be seen at:

https://github.com/ksunden/WrightTools/tree/type-hints
With appveyor builds:
https://ci.appveyor.com/project/wright-group/wrighttools-1oxes/history
