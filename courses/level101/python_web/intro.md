# Python and The Web

## Prerequisites

- Basic understanding of Python language.
- Basic familiarity with Flask framework.

## What to expect from this course

This course is divided into two high-level parts. In the first part, assuming familiarity with Python language’s basic operations and syntax usage, we will dive a little deeper into understanding Python as a language. We will compare Python with other programming languages that you might already know like Java and C. We will also explore concepts of Python objects and with help of that, explore Python features like decorators.

In the second part which will revolve around the web, and also assume familiarity with the Flask framework, we will start from the `socket` module and work with HTTP requests. This will demystify how frameworks like Flask work internally.

And to introduce SRE flavour to the course, we will design, develop and deploy (in theory) a URL-shortening application. We will emphasize parts of the whole process that are more important as an SRE of the said app/service.

## What is not covered under this course

Extensive knowledge of Python internals and advanced Python.

## Lab Environment Setup

Have latest version of Python installed

## Course Contents

1. [The Python Language](https://linkedin.github.io/school-of-sre/level101/python_web/intro/#the-python-language)
      1. [Some Python Concepts](https://linkedin.github.io/school-of-sre/level101/python_web/python-concepts/)
      2. [Python Gotchas](https://linkedin.github.io/school-of-sre/level101/python_web/python-concepts/#some-gotchas)
2. [Python and Web](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/)
      1. [Sockets](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/#sockets)
      2. [Flask](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/#flask)
3. [The URL-Shortening App](https://linkedin.github.io/school-of-sre/level101/python_web/url-shorten-app/)
      1. [Design](https://linkedin.github.io/school-of-sre/level101/python_web/url-shorten-app/#design)
      2. [Scaling The App](https://linkedin.github.io/school-of-sre/level101/python_web/sre-conclusion/#scaling-the-app)
      3. [Monitoring The App](https://linkedin.github.io/school-of-sre/level101/python_web/sre-conclusion/#monitoring-strategy)

## The Python Language

Assuming you know a little bit of C/C++ and Java, let's try to discuss the following questions in context of those two languages and Python. You might have heard that C/C++ is a compiled language while Python is an interpreted language. Generally, with compiled language we first compile the program and then run the executable while in case of Python we run the source code directly like `python hello_world.py`. While Java, being an interpreted language, still has a separate compilation step and then it's run. So, what's really the difference?

### Compiled vs. Interpreted

This might sound a little weird to you: Python, in a way is a compiled language! Python has a compiler built-in! It is obvious in the case of Java since we compile it using a separate command, ie: `javac helloWorld.java` and it will produce a `.class` file which we know as a _bytecode_. Well, Python is very similar to that. One difference here is that there is no separate compile command/binary needed to run a Python program.

**What is the difference then, between Java and Python?**
Well, Java's compiler is more strict and sophisticated. As you might know Java is a statically typed language. So the compiler is written in a way that it can verify types-related errors during compile time. While Python being a _dynamic_ language, types are not known until a program is run. So in a way, Python compiler is dumb (or, less strict). But there indeed is a compile step involved when a Python program is run. You might have seen Python bytecode files with `.pyc` extension. Here is how you can see bytecode for a given Python program.

```bash
# Create a Hello World
$ echo "print('hello world')" > hello_world.py

# Making sure it runs
$ python3 hello_world.py
hello world

# The bytecode of the given program
$ python -m dis hello_world.py
 1           0 LOAD_NAME                0 (print)
             2 LOAD_CONST               0 ('hello world')
             4 CALL_FUNCTION            1
             6 POP_TOP
             8 LOAD_CONST               1 (None)
            10 RETURN_VALUE
```

Read more about `dis` module [here](https://docs.python.org/3/library/dis.html).

Now coming to C/C++, there of course is a compiler. But the output is different than what Java/Python compiler would produce. Compiling a C program would produce what we also know as _machine code_, as opposed to _bytecode_.

### Running The Programs

We know compilation is involved in all 3 languages we are discussing. Just that the compilers are different in nature and they output different types of content. In case of C/C++, the output is machine code which can be directly read by your operating system. When you execute that program, your OS will know how exactly to run it. **But this is not the case with bytecode.**

Those bytecodes are language specific. Python has its own set of bytecode defined (more in `dis` module) and so does Java. So naturally, your operating system will not know how to run it. To run this bytecode, we have something called Virtual Machines. Ie: The JVM or the Python VM (CPython, Jython). These so-called Virtual Machines are the programs which can read the bytecode and run it on a given operating system. Python has multiple VMs available. CPython is a Python VM implemented in C language, similarly Jython is a Java implementation of Python VM. **At the end of the day, what they should be capable of is to understand Python language syntax, be able to compile it to bytecode and be able to run that bytecode.** You can implement a Python VM in any language! (And people do so, just because it can be done)

```
                                                              The Operating System

                                                              +------------------------------------+
                                                              |                                    |
                                                              |                                    |
                                                              |                                    |
hello_world.py                    Python bytecode             |         Python VM Process          |
                                                              |                                    |
+----------------+                +----------------+          |         +----------------+         |
|print(...       |   COMPILE      |LOAD_CONST...   |          |         |Reads bytecode  |         |
|                +--------------->+                +------------------->+line by line    |         |
|                |                |                |          |         |and executes.   |         |
|                |                |                |          |         |                |         |
+----------------+                +----------------+          |         +----------------+         |
                                                              |                                    |
                                                              |                                    |
                                                              |                                    |
hello_world.c                     OS Specific machinecode     |         A New Process              |
                                                              |                                    |
+----------------+                +----------------+          |         +----------------+         |
|void main() {   |   COMPILE      | binary contents|          |         | binary contents|         |
|                +--------------->+                +------------------->+                |         |
|                |                |                |          |         |                |         |
|                |                |                |          |         |                |         |
+----------------+                +----------------+          |         +----------------+         |
                                                              |         (binary contents           |
                                                              |         runs as is)                |
                                                              |                                    |
                                                              |                                    |
                                                              +------------------------------------+
```

Two things to note for above diagram:

1. Generally, when we run a Python program, a Python VM process is started which reads the Python source code, compiles it to bytecode and run it in a single step. Compiling is not a separate step. Shown only for illustration purpose.
2. Binaries generated for C like languages are not _exactly_ run as is. Since there are multiple types of binaries (eg: ELF), there are more complicated steps involved in order to run a binary but we will not go into that since all that is done at OS level.
