# Conclusion

We have covered the basics of Linux operating systems and basic commands used in linux.
We have also covered the Linux server administration commands.

We hope that this course will make it easier for you to operate on the command line.

## Applications in SRE Role

1. As a SRE, you will be required to perform some general tasks on these Linux servers. You will also be using the command line when you are troubleshooting issues.
2. Moving from one location to another in the filesystem will require the help of `ls`, `pwd` and `cd` commands.
3. You may need to search some specific information in the log files. `grep` command would be very useful here. I/O redirection will become handy if you want to store the output in a file or pass it as an input to another command.
4. `tail` command is very useful to view the latest data in the log file.
5. Different users will have different permissions depending on their roles. We will also not want everyone in the company to access our servers for security reasons. Users permissions can be restricted with `chown`, `chmod` and `chgrp` commands.
6. `ssh` is one of the most frequently used commands for a SRE. Logging into servers and troubleshooting along with performing basic administration tasks will only be possible if we are able to login into the server.
7. What if we want to run an apache server or nginx on a server? We will first install it using the package manager. Package management commands become important here.
8. Managing services on servers is another critical responsibility of a SRE. Systemd related commands can help in troubleshooting issues. If a service goes down, we can start it using `systemctl start` command. We can also stop a service in case it is not needed.
9. Monitoring is another core responsibility of a SRE. Memory and CPU are two important system level metrics which should be monitored. Commands like `top` and `free` are quite helpful here.
10. If a service is throwing an error, how do we find out the root cause of the error ? We will certainly need to check logs to find out the whole stack trace of the error. The log file will also tell us the number of times the error has occurred along with time when it started.

## Useful Courses and tutorials

* [Edx basic linux commands course](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS101x+1T2020/course/)
* [Edx Red Hat Enterprise Linux Course](https://courses.edx.org/courses/course-v1:RedHat+RH066x+2T2017/course/)
* [https://linuxcommand.org/lc3_learning_the_shell.php](https://linuxcommand.org/lc3_learning_the_shell.php)
