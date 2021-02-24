# Some Python Concepts

Though you are expected to know python and its syntax at basic level, let us discuss some fundamental concepts that will help you understand the python language better.

**Everything in Python is an object.**

That includes the functions, lists, dicts, classes, modules, a running function (instance of function definition), everything. In the CPython, it would mean there is an underlying struct variable for each object.

In python's current execution context, all the variables are stored in a dict. It'd be a string to object mapping. If you have a function and a float variable defined in the current context, here is how it is handled internally.

```python
>>> float_number=42.0
>>> def foo_func():
...     pass
...

# NOTICE HOW VARIABLE NAMES ARE STRINGS, stored in a dict
>>> locals()
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'float_number': 42.0, 'foo_func': <function foo_func at 0x1055847a0>}
```

## Python Functions

Since functions too are objects, we can see what all attributes a function contains as following

```python
>>> def hello(name):
...     print(f"Hello, {name}!")
...
>>> dir(hello)
['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__',
'__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__',
'__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__',
'__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__',
'__subclasshook__']
```

While there are a lot of them, let's look at some interesting ones

#### __globals__

This attribute, as the name suggests, has references of global variables. If you ever need to know what all global variables are in the scope of this function, this will tell you. See how the function start seeing the new variable in globals

```python
>>> hello.__globals__
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'hello': <function hello at 0x7fe4e82554c0>}

# adding new global variable
>>> GLOBAL="g_val"
>>> hello.__globals__
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'hello': <function hello at 0x7fe4e82554c0>, 'GLOBAL': 'g_val'}
```

### __code__

This is an interesting one! As everything in python is an object, this includes the bytecode too. The compiled python bytecode is a python code object. Which is accessible via `__code__` attribute here. A function has an associated code object which carries some interesting information.

```python
# the file in which function is defined
# stdin here since this is run in an interpreter
>>> hello.__code__.co_filename
'<stdin>'

# number of arguments the function takes
>>> hello.__code__.co_argcount
1

# local variable names
>>> hello.__code__.co_varnames
('name',)

# the function code's compiled bytecode
>>> hello.__code__.co_code
b't\x00d\x01|\x00\x9b\x00d\x02\x9d\x03\x83\x01\x01\x00d\x00S\x00'
```

There are more code attributes which you can enlist by `>>> dir(hello.__code__)`

## Decorators

Related to functions, python has another feature called decorators. Let's see how that works, keeping  `everything is an object` in mind.

Here is a sample decorator:

```python
>>> def deco(func):
...     def inner():
...             print("before")
...             func()
...             print("after")
...     return inner
...
>>> @deco
... def hello_world():
...     print("hello world")
...
>>>
>>> hello_world()
before
hello world
after
```

Here `@deco` syntax is used to decorate the `hello_world` function. It is essentially same as doing

```python
>>> def hello_world():
...     print("hello world")
...
>>> hello_world = deco(hello_world)
```

What goes inside the `deco` function might seem complex. Let's try to uncover it.

1. Function `hello_world` is created
2. It is passed to `deco` function
3. `deco` create a new function
      1. This new function is calls `hello_world` function
      2. And does a couple other things
4. `deco` returns the newly created function
5. `hello_world` is replaced with above function

Let's visualize it for better understanding

```text
       BEFORE                   function_object (ID: 100)

       "hello_world"            +--------------------+
               +                |print("hello_world")|
               |                |                    |
               +--------------> |                    |
                                |                    |
                                +--------------------+


       WHAT DECORATOR DOES

       creates a new function (ID: 101)
       +---------------------------------+
       |input arg: function with id: 100 |
       |                                 |
       |print("before")                  |
       |call function object with id 100 |
       |print("after")                   |
       |                                 |
       +---------------------------^-----+
                                   |
                                   |
       AFTER                       |
                                   |
                                   |
       "hello_world" +-------------+
```

Note how the `hello_world` name points to a new function object but that new function object knows the reference (ID) of the original function.

## Some Gotchas

- While it is very quick to build prototypes in python and there are tons of libraries available, as the codebase complexity increases, type errors become more common and will get hard to deal with. (There are solutions to that problem like type annotations in python. Checkout [mypy](http://mypy-lang.org/).)
- Because python is dynamically typed language, that means all types are determined at runtime. And that makes python run very slow compared to other statically typed languages.
- Python has something called [GIL](https://www.dabeaz.com/python/UnderstandingGIL.pdf) (global interpreter lock) which is a limiting factor for utilizing multiple CPI cores for parallel computation.
- Some weird things that python does: https://github.com/satwikkansal/wtfpython
