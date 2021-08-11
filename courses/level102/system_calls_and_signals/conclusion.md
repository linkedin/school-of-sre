# Conclusion

One of the main goals of a SRE is to improve the reliability of high scale systems. Inorder to achieve this, a basic understanding of the internal workings of a system is necessary. 

Getting to know about how signals work is important since they play a big role in the lifecycle of processes. We see the use of signals in a range of operations on processes : from creating a process to killing a process. Knowledge of signals is important especially when handling them in programs. If you anticipate an event that causes signals, you can define a handler function and tell the operating system to run it when that particular type of signal arrives.

Understanding system calls is especially useful to SRE's while debugging any Linux process. System calls provide precise knowledge of the internal functionalities of an operating system. It gives an in-depth understanding for programmers about C library functions which implement system calls at a lower level. With the use of *strace* command, one may easily debug slow or hung processes.



# Further Reading
<https://www.oreilly.com/library/view/understanding-the-linux/0596002130/ch01s06.html>

<https://jvns.ca/blog/2021/04/03/what-problems-do-people-solve-with-strace/>

<https://medium.com/@akhandmishra/important-system-calls-every-programmer-should-know-8884381ceadb>

<https://www.brendangregg.com/blog/2014-05-11/strace-wow-much-syscall.html>