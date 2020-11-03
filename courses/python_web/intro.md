# School of SRE: Python and The Web

## Pre - Reads

- Basic understanding of python language.
- Basic familiarity with flask framework.

## What to expect from this training

This course is divided into two high level parts. In the first part, assuming familiarity with python language’s basic operations and syntax usage, we will dive a little deeper into understanding python as a language. We will compare python with other programming languages that you might already know like Java and C. We will also explore concepts of Python objects and with help of that, explore python features like decorators.

In the second part which will revolve around the web, and also assume familiarity with the Flask framework, we will start from the socket module and work with HTTP requests. This will demystify how frameworks like flask work internally.

And to introduce SRE flavour to the course, we will design, develop and deploy (in theory) a URL shortening application. We will emphasize parts of the whole process that are more important as an SRE of the said app/service.

## What is not covered under this training

Extensive knowledge of python internals and advanced python.

## Training Content

### Lab Environment Setup

Have latest version of python installed

### TOC

1. The Python Language
      1. Some Python Concepts
      2. Python Gotchas
2. Python and Web
      1. Sockets
      2. Flask
3. The URL Shortening App
      1. Design
      2. Scaling The App
      3. Monitoring The App

## The Python Language

Assuming you know a little bit of C/C++ and Java, let's try to discuss the following questions in context of those two languages and python. You might have heard that C/C++ is a compiled language while python is an interpreted language. Generally, with compiled language we first compile the program and then run the executable while in case of python we run the source code directly like `python hello_world.py`. While Java, being an interpreted language, still has a separate compilation step and then its run. So what's really the difference?

### Compiled vs. Interpreted

This might sound a little weird to you: python, in a way is a compiled language! Python has a compiler built-in! It is obvious in the case of java since we compile it using a separate command ie: `javac helloWorld.java` and it will produce a `.class` file which we know as a _bytecode_. Well, python is very similar to that. One difference here is that there is no separate compile command/binary needed to run a python program.

**What is the difference then, between java and python?**
Well, Java's compiler is more strict and sophisticated. As you might know Java is a statically typed language. So the compiler is written in a way that it can verify types related errors during compile time. While python being a _dynamic_ language, types are not known until a program is run. So in a way, python compiler is dumb (or, less strict). But there indeed is a compile step involved when a python program is run. You might have seen python bytecode files with `.pyc` extension. Here is how you can see bytecode for a given python program.

```bash
# Create a Hello World
spatel1-mn1:tmp spatel1$ echo "print('hello world')" > hello_world.py

# Making sure it runs
spatel1-mn1:tmp spatel1$ python3 hello_world.py
hello world

# The bytecode of the given program
spatel1-mn1:tmp spatel1$ python -m dis hello_world.py
 1           0 LOAD_NAME                0 (print)
             2 LOAD_CONST               0 ('hello world')
             4 CALL_FUNCTION            1
             6 POP_TOP
             8 LOAD_CONST               1 (None)
            10 RETURN_VALUE
```

Read more about dis module [here](https://docs.python.org/3/library/dis.html)

Now coming to C/C++, there of course is a compiler. But the output is different than what java/python compiler would produce. Compiling a C program would produce what we also know as _machine code_. As opposed to bytecode.

### Running The Programs

We know compilation is involved in all 3 languages we are discussing. Just that the compilers are different in nature and they output different types of content. In case of C/C++, the output is machine code which can be directly read by your operating system. When you execute that program, your OS will know how exactly to run it. **But this is not the case with bytecode.**

Those bytecodes are language specific. Python has its own set of bytecode defined (more in `dis` module) and so does java. So naturally, your operating system will not know how to run it. To run this bytecode, we have something called Virtual Machines. Ie: The JVM or the Python VM (CPython, Jython). These so called Virtual Machines are the programs which can read the bytecode and run it on a given operating system. Python has multiple VMs available. Cpython is a python VM implemented in C language, similarly Jython is a Java implementation of python VM. **At the end of the day, what they should be capable of is to understand python language syntax, be able to compile it to bytecode and be able to run that bytecode.** You can implement a python VM in any language! (And people do so, just because it can be done)

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

1. Generally, when we run a python program, a python VM process is started which reads the python source code, compiles it to byte code and run it in a single step. Compiling is not a separate step. Shown only for illustration purpose.
2. Binaries generated for C like languages are not _exactly_ run as is. Since there are multiple types of binaries (eg: ELF), there are more complicated steps involved in order to run a binary but we will not go into that since all that is done at OS level.
