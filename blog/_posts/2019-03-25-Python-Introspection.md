---
layout: post
title: "Python Introspection"
categories:
  - guide
tags:
  - guide
---
# Python Introspection
Updated on: 25-03-2019

This article has been heavily influenced by the [Python introspection](https://www.ibm.com/developerworks/library/l-pyint/index.html) post on IBM Developer works. This article is more of a cheat sheet and crash-course to get you started.

## Python `help`
`help` command in python cli is very useful. Here are a few help commands that you might find quite useful:

`help('modules')` will provide you the list of available python modules on your computer. Modules are simply text files.

`help('topics')` will provide help on different topics regarding programming python. They are quite useful when you are getting starting with the language.

## `sys` module
Here are some useful `sys` module commands:

`sys.executable` provides python to the python interpreter

`sys.platform` provides information about the operating system

`sys.version` provides the version of the python

`sys.maxint` provides the maximum value of an int

`sys.path` provides the list of directories in which python will look for modules during imports

`sys.modules` is a very useful attribute of the sys module that provides a dictionary mapping module names to the module objects for all currently loaded modules

**Don't forget to import the `sys` module using `import sys` before experimenting with the attributes above in the python cli.**

## The `dir()` function
This built-in function is your guide is you want to figure out what each module contains. Since it's not easy to remember this information. Try it for the `sys` module by calling `dir(sys)` and see what you get.
You can check the modules, functions, and attributes available in the current scope using `dir()`. Using `dir()` you can figure out all the attributes associated with an object and since everything in python is an object you can apply the function to anything. Try these `dir(42)`, `dir(dir)`, `dir("Hello World")`.

`dir(__builtins__)` provides built-in functions, error objects and attributes.

## Introspecting Python objects
First function we will look at is `type()`. Type function can help us determine which class the object is an instance of. Try `type(42)`, `type("Hello World")`, `type([])`.

Identity function `id()` returns a unique identifier for an object. Try `print id.__doc__` to see what is actually returns (Hint: It's the memory address). Try declaring some variables and then passing the variable name as the argument to the identity function.

If you want to see whether an object has a particular attribute and want to retrieve that attribute then you can use the `hasattr()` and `getattr()` functions. For example, try the following:

```python
hasattr(id, '__doc__')
print getattr(id, '__doc__')
```

To check whether an attribute can be invoked or called use `callable()`. For example `callable("Hello World")` and `callable(dir)`.

To check whether a class is a subclass of another one you can use `issubclass()`. For example, if you have two classes `SuperHero` and `Person`. You can try `issubclass(SuperHero, Person)` to check if `SuperHero` is a subclass of `Person` class.

Hopefully, these functions and techniques will help you get a better understanding of unknown modules and objects as you play with them. Happy Introspection!
