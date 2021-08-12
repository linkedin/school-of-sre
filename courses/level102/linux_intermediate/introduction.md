
# Linux-Intermediate

## Prerequisites 

- Expect to have gone through the School Of SRE [<u>Linux Basics</u>](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/).

## What to expect from this course

 This course is divided into two sections. In the first section we will cover where we left off the Linux Basics, earlier in the School of SRE curriculum, we will deep dive into some of the more advanced linux commands and concepts.
 
 In this second section we will discuss how we use Bash scripting in day to day work, automation and toil reduction as an SRE with the help of real life examples of any SRE.

## What is not covered under this course

This course aims to make you familiar with the intersection of Linux commands, shell scripting and how SRE uses it. We would not be covering Linux internals.


## Lab Environment Setup

- Install docker on your system. [<u>https://docs.docker.com/engine/install/</u>](https://docs.docker.com/engine/install/)
    
- We would be using RedHat Enterprise Linux (RHEL) 8.

![](images/image1.png)

- We would be running most of the commands in the above docker container.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Course Content 

[<u>Package Management</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management.md)

- [<u>Package:</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management.md#package)
    
- [<u>Dependencies</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management.md#dependencies)
    
- [<u>Repository</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management.md#repository)
    
- [<u>High Level and Low-Level Package management tools</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management.md#high-level-and-low-level-package-management-tools)
    

[<u>Storage Media</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md)

- [<u>Listing the mounted storage devices</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#listing-the-mounted-storage-devices)
    
- [<u>Creating a FileSystem</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#creating-a-filesystem)
    
- [<u>Mounting the device</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#mounting-the-device)
    
- [<u>Unmounting the device</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#unmounting-the-device)
    
- [<u>Making it easier with /etc/fstab file?</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#making-it-easier-with-etcfstab-file)
    
- [<u>Checking and Repairing FS</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#checking-and-repairing-fs)
    
- [<u>RAID</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid)
    
    - [<u>RAID levels</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-levels)
        
    - [<u>RAID 0 (Striping)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-0-striping)
        
    - [<u>RAID 1(Mirroring)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-1mirroring)
        
    - [<u>RAID 5(Striping with distributed parity)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-5striping-with-distributed-parity)
        
    - [<u>RAID 6(Striping with double parity)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-6striping-with-double-parity)
        
    - [<u>RAID 10(RAID 1+0 : Mirroring and Striping)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#raid-10raid-10-mirroring-and-striping)
        
    - [<u>Commands to monitor RAID</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#commands-to-monitor-raid)
        
- [<u>LVM</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media.md#lvm)
    

[<u>Archiving and Backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md)

- [<u>Archiving</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#archiving)
    
    - [<u>gzip</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#gzip)
        
    - [<u>tar</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#tar)
        
    - [<u>Create an archive with files and folder</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#create-an-archive-with-files-and-folder)
        
    - [<u>Listing files in the archive</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#listing-files-in-the-archive)
        
    - [<u>Extract files from the archive</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#extract-files-from-the-archive)
        
- [<u>Backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#backup)
    
    - [<u>Incremental backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#incremental-backup)
        
    - [<u>Differential backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#differential-backup)
        
    - [<u>Network backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#network-backup)
        
    - [<u>Cloud Backup</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup.md#cloud-backup)
        
[<u>Introduction to Vim</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim.md)
 
- [<u>Opening a file and using insert mode</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim.md#opening-a-file-and-using-insert-mode)
    
- [<u>Saving a file</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim.md#saving-a-file)
    
- [<u>Exiting the VIM editor</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim.md#exiting-the-vim-editor)
    
[<u>Bash Scripting</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md)

- [<u>Writing the first bash script</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#writing-the-first-bash-script)
    
- [<u>Taking user input and working with variables</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#taking-user-input-and-working-with-variables)
    
- [<u>Exit status</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#exit-status)
    
- [<u>Command line arguments and understanding If … else branching</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#command-line-arguments-and-understanding-if-..-else-branching)
    
- [<u>Looping over to do a repeated task</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#looping-over-to-do-a-repeated-task.)
    
- [<u>Function</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting.md#function)
    
[<u>Conclusion</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/conclusion.md)
        
