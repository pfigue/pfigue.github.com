title: Python annotations
tags: python3, python, annotations
date: 2015-12-02
slug: python-annotations
authors: pfigue
category: python
status: published

# Why annotations?
It is handy to know which data types a function expects to receive or which data types you can expect a function to return.

That's why the Python community developed some mechanism (namely, [PEP 3107 - Function Annotations](http://www.python.org/dev/peps/pep-3107/) and [PEP 0484 - Type Hints](https://www.python.org/dev/peps/pep-0484/)) to provide some information for the developer (the one using the function).

# How do they look like?
A declaration like this one:

    def file_size(path: str) -> int:
        pass

would define a function which is returning an integer and is receving a parameter *path*, which is expected to be a string.

This information is only a **recommendation** for the person writing calls to `file_size`.

**Python will not ever do any type checking**, neither during compile nor during run time.

That information on type is just a help for the users of the functions. Or given the case, for some code analysis tool, which could use it to check for (potential) typing problems.

# Data types and default values
The type don't need even to be real existing valid Python types. We could have written something like:

    def file_size(path: "path to some object in the filesystem") -> "size in bytes":
        pass

Probably those imaginary (Note1) code analysis tools which benefit of annotations will dislike this new *types*, but for an human-enough developer they can work as some kind of documentation.

Of course, we can still have optional parameters with default values:

    DEFAULT_PATH = '/here/and/there'
    def file_size(path: str=DEFAULT_PATH) -> 'size in bytes':
        pass

# The \__annotations__ attribute
        
In this last case, if we ask about the attribute `__annotations__` of that file_size function:

    In [1]: file_size.__annotations__
    Out[1]: {'path': str, 'return': 'size in bytes'}
    
we get a list of parameters and their types. That's cool!


# Python 3 vs Python 2

They work for Python3 but **NOT for Python2**. And right now I am not 100% sure they work for all versions of Py3.

If you have to work with Py2, this could be your alternative:

    DEFAULT_PATH = '/here/and/there'
    def file_size(path=DEFAULT_PATH):
        """
        :param path: path to some object in the filesystem
        :type path: str
        :returns: size in bytes
        :rtype: int
        """
        pass

# Notes:

  1. imaginary because right now I don't know a single tool which profits from annotations. Maybe a `pip search annotations` would help.

#Refs:

  * [StackOverflow Question - Function parameter types in Python](http://stackoverflow.com/a/21384492)
  * [PEP 3107 - Function Annotations](http://www.python.org/dev/peps/pep-3107/)
  * [PEP 0484 - Type Hints](https://www.python.org/dev/peps/pep-0484/)
  