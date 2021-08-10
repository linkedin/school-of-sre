
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

[<u>Package Management</u>](package_management.md)

- [<u>Package:</u>](package_management.md#package)
    
- [<u>Dependencies</u>](package_management.md#dependencies)
    
- [<u>Repository</u>](package_management.md#repository)
    
- [<u>High Level and Low-Level Package management tools</u>](package_management.md#high-level-and-low-level-package-management-tools)
    

[<u>Storage Media</u>](storage_media.md)

- [<u>Listing the mounted storage devices</u>](storage_media.md#listing-the-mounted-storage-devices)
    
- [<u>Creating a FileSystem</u>](storage_media.md#creating-a-filesystem)
    
- [<u>Mounting the device</u>](storage_media.md#mounting-the-device)
    
- [<u>Unmounting the device</u>](storage_media.md#unmounting-the-device)
    
- [<u>Making it easier with /etc/fstab file?</u>](storage_media.md#making-it-easier-with-etcfstab-file)
    
- [<u>Checking and Repairing FS</u>](storage_media.md#checking-and-repairing-fs)
    
- [<u>RAID</u>](storage_media.md#raid)
    
    - [<u>RAID levels</u>](storage_media.md#raid-levels)
        
    - [<u>RAID 0 (Striping)</u>](storage_media.md#raid-0-striping)
        
    - [<u>RAID 1(Mirroring)</u>](storage_media.md#raid-1mirroring)
        
    - [<u>RAID 5(Striping with distributed parity)</u>](storage_media.md#raid-5striping-with-distributed-parity)
        
    - [<u>RAID 6(Striping with double parity)</u>](storage_media.md#raid-6striping-with-double-parity)
        
    - [<u>RAID 10(RAID 1+0 : Mirroring and Striping)</u>](storage_media.md#raid-10raid-10-mirroring-and-striping)
        
    - [<u>Commands to monitor RAID</u>](storage_media.md#commands-to-monitor-raid)
        
- [<u>LVM</u>](storage_media.md#lvm)
    

[<u>Archiving and Backup</u>](archiving_backup.md)

- [<u>Archiving</u>](archiving_backup.md#archiving)
    
    - [<u>gzip</u>](archiving_backup.md#gzip)
        
    - [<u>tar</u>](archiving_backup.md#tar)
        
    - [<u>Create an archive with files and folder</u>](archiving_backup.md#create-an-archive-with-files-and-folder)
        
    - [<u>Listing files in the archive</u>](archiving_backup.md#listing-files-in-the-archive)
        
    - [<u>Extract files from the archive</u>](archiving_backup.md#extract-files-from-the-archive)
        
- [<u>Backup</u>](archiving_backup.md#backup)
    
    - [<u>Incremental backup</u>](archiving_backup.md#incremental-backup)
        
    - [<u>Differential backup</u>](archiving_backup.md#differential-backup)
        
    - [<u>Network backup</u>](archiving_backup.md#network-backup)
        
    - [<u>Cloud Backup</u>](archiving_backup.md#cloud-backup)
        
[<u>Introduction to Vim</u>](introvim.md)
 
- [<u>Opening a file and using insert mode</u>](introvim.md#opening-a-file-and-using-insert-mode)
    
- [<u>Saving a file</u>](introvim.md#saving-a-file)
    
- [<u>Exiting the VIM editor</u>](introvim.md#exiting-the-vim-editor)
    
[<u>Bash Scripting</u>](bashscripting.md)

- [<u>Writing the first bash script</u>](bashscripting.md#writing-the-first-bash-script)
    
- [<u>Taking user input and working with variables</u>](bashscripting.md#taking-user-input-and-working-with-variables)
    
- [<u>Exit status</u>](bashscripting.md#exit-status)
    
- [<u>Command line arguments and understanding If … else branching</u>](bashscripting.md#command-line-arguments-and-understanding-if-..-else-branching)
    
- [<u>Looping over to do a repeated task</u>](bashscripting.md#looping-over-to-do-a-repeated-task.)
    
- [<u>Function</u>](bashscripting.md#function)
    
[<u>Conclusion</u>](conclusion.md)
        
