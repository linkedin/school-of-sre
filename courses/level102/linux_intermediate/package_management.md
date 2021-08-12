# Package Management
## Introduction 

One of the main features of any operating system is the ability to run other programs and softwares, and hence Package management comes into picture. Package management is a method of installing and maintaining software programs on any operating system.

## Package

In the early days of Linux, one had to download source code of any software and compile it to install and run the software. As the Linux space became more mature, it is  understood the software landscape is very dynamic and started distributing software in the form of packages. Package file is a compressed collection of files that contains software, its dependencies, installation instructions and metadata about the package.

## Dependencies

It is rare that a software package is stand-alone, it depends on the different software, libraries and modules. These subroutines are stored and made available in the form of shared libraries which may serve more than one program. These shared resources are called dependencies. Package management does this hard job of resolving dependencies and installing them for the user along with the software.

## Repository

Repository is a storage location where all the packages, updates, dependencies are stored. Each repository can contain thousands of software packages hosted on a remote server intended to be installed and updated on linux systems. We usually update the package information ( *often referred to as metadata*) by running “*sudo dnf update”.*

![](images/image29.png)

Try out *`sudo dnf repolist all`* to list all the repositories.

We usually add repositories for installing packages from third party vendors.

> dnf config-manager --add-repo http://www.example.com/example.repo

## High Level and Low-Level Package management tools

There are mainly two types of packages management tools:

> 1\. *Low-level tools*: This is mostly used for installing, removing and upgrading package files.
> 
> 2\. *High-Level tools*: In addition to Low-level tools, High-level tools do metadata searching and dependency resolution as well.

| Linux Distribution | Low-Level Tools | High-Level tools |
| --- | --- | --- |
| Debian | dpkg | apt-get |
| Fedora, RedHat | dnf | dnf |

