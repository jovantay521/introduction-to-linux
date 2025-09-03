*Author: Tay Jovan*
*Last Updated: 2025/09/03*
# Intro to Linux Workshop Guide

## 0. Outline

[[#1. Introduction & Setup]]
[[#2. The Command Line]]
[[#3. Files & Directories]]
[[#4. Users, Permissions & Ownership]]
[[#5. Processes & System Monitoring]]
[[#6. Software Management]]
[[#7. Redirection, Pipes & Text Processing]]
[[#8. Networking & Remote Access]]
[[#9. Next Steps]]
[[#10. References]]

## 1. Introduction & Setup

### 1.1. What is Linux?

>Linux is a family of open source Unix-like operating systems based on Linux kernel, an operating system kernel first released on September 17, 1991, by Linux Torvalds ([Wikipedia](https://en.wikipedia.org/wiki/Linux))

Open source is source code that is made freely available for possible modification and redistribution. That means anyone can view it, modify it and create new distributions for it.

![[linux-distribution-timeline.svg]]

What is unix?
What is a kernel?

### 1.2. Setting Up Linux
#### 1.2.1. Windows Subsystem for Linux (WSL)

>  The Windows Subsystem for Linux (WSL) lets developers install a Linux distribution (such as Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc) and use Linux applications, utilities, and Bash command-line tools directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.

Installation command:

```
wsl --install
```

System will prompt you to enter your username:

```
Enter new UNIX username: jovan
```

It will then prompt you to create a new password:

```
New password: password
```

Retype your password you just set:

```
Retype new password: password
```

Once successfully installed, you should see a message similar to this:

```
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 24.04.5 LTS (GNU/Linux 5.15.153.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Tue Sep  2 21:33:38 +08 2025

  System load:  0.64                Processes:             63
  Usage of /:   0.1% of 1006.85GB   Users logged in:       0
  Memory usage: 3%                  IPv4 address for eth0: <REDACTED>
  Swap usage:   0%


This message is shown once a day. To disable it please create the
/home/jovan/.hushlogin file.
```

## 2. The Command Line

### 2.1. Terminal vs Shell

A terminal is the software application that provides a text-based interface to interact with the shell e.g. `Windows Terminal`

The shell is the program that interprets and executes commands typed by the user. On Ubuntu, we are using [Bash](https://www.gnu.org/software/bash/) (Bourne Again Shell) and on Mac OS, it is [ZSH](https://zsh.sourceforge.io/) (Z Shell).

![[Untitled-2025-09-02-2207.png]]

### 2.2. Basic Commands

First, let's figure out what is the default directory that we are put into when we first start up Linux:

```
pwd
```

To list the current directory contents:

```
ls
```

We get nothing back in the output because the default files in your home directory is hidden. To view hidden files:

```
ls -a
```

To view the files in list form, we add the `-l` switch:

```
ls -la
```

To change our directory we use:

```
cd
```

If we don't specify a directory, by default the system will return us to our home directory (`/home/$USER`).

![[directory-diagram.png]]

To change to the parent directory:

```
cd ..
```

To verify that we have moved to the `/home` directory:

```
pwd
```

```
jovan@jovan-laptop:/home$ pwd
/home
```

To move back to our home directory:

```
cd
```

If you need more information about a command, instead of searching the internet, we can instead use a command to view the manual:

```
man
```

Let's view the manual for `man`:

```
man man
```

We can also use the `-h` / `--help` switch to quickly view the options available for a command:

```
man -h
```

or

```
man --help
```

## 3. Files & Directories
### 3.1. Creating and Managing Files & Directories

On Linux, we use the term `directory` instead of `folders`. Unix-like systems represent most resources as files. This means that we can interact with them in a unified way (e.g. `cat`, `ls`, `chmod`).

Some examples:
- `/dev/`: devices
- `/proc/`: processes
- `/etc/`: system config files
- `/sys/`: kernel and device settings

Let's create a directory:

```
mkdir directory
```

Let's create a file:

```
touch file
```

Copy the file:

```
cp file file2
```


Move the file into the directory:

```
mv file directory
```

Remove the file:

```
rm file2
```

Remove the directory:

```
rmdir directory
```

We get an error because the directory contains `file2`:

```
rmdir: failed to remove 'directory': Directory not empty
```

We can change to the directory and manually remove each file inside. But a more efficient method would be to add the `-r` flag to recursively remove all the files within the directory and then the directory itself:

```
rm -r directory
```

Note: a safe practice would be to add `-i` switch to receive a prompt before remove each file:

```
rm -ri directory
```

**WARNING: DO NOT EVER RUN `rm -rf` ON YOUR ROOT DIRECTORY `/`:**

```
# rm -rf /
```

### 3.2. Viewing the file content

Let's download a file to demonstrate the next few commands:

```
curl -o moby-dick.txt https://www.gutenberg.org/files/2701/2701-0.txt
```

To display a file's content, we use `cat` to concatenate it:

```
cat moby-dick.txt
```

Sometimes the file has too much content to be viewed easily in the command line. We can utilise `head` and `tail` to view the first few and last few lines respectively:

```
head moby-dick.txt
```

```
tail moby-dick.txt
```

If we want to view `x` number of lines, we use the `-n` switch to specify:

```
head -n 5 moby-dick.txt
```

```
tail -n 5 moby-dick.txt
```

We can also use the file-pager `less`. This allows you to view the contents of a file one sreen at a time:

```
less moby-dick.txt
```

Common controls:
- `space`: Scroll forward one page
- `b`: Scroll backward one page
- `up/down`: Scroll up/down one line
- `g`: Go to beginning of file
- `G`: Go to end of file
- `/word`: Search forward for "word"
- `n`: Next search result
- `shift + n`: Previous search result
- `q`: Quit

### 3.3. File/Directory Properties

To view a file/directory properties with have to list our current directory in the list format with the `-l` switch:

```
ls -l
```

We should be able to view the properties of the `moby-dick.txt` file we downloaded earlier:

```
-rw-r--r-- 1 jovan jovan 1256529 Jan 19  2025 moby-dick.txt
```

Let's breakdown the output:
1. `-rw-r--r--`: permissions
2. `1`: number of hard links
3. `jovan`: owner
4. `jovan`: group
5. `1256529`: size of the file (in bytes)
6. `Jan 19  2025`: last modified date
7. `moby-dick.txt`: file name
## 4. Users, Permissions & Ownership

### 4.1. Users, Groups & Root

![[user-group-root-diagram.png]]

We will use a company to breakdown Users, Groups and the Root user.

In a company, users represents a regular employee. They each have their individual accounts on the system. Each user has a username (e.g. `jovan`), a home directory (e.g. `/home/jovan`) and user ID.

A group represent a department, which the employees belong to.

The root user represents the system administrator. Root can:
- Read, write, or delete _any_ file.
- Install or remove software.
- Manage users and groups.
- Control system processes and settings.

![[thanos.jpg]]

To see which user you are currently logged in as:

```
whoami
```

To view your user information:

```
id
```

To view root information:

```
id root
```

For a normal user to run commands as root temporarily, we will use `sudo` (*superuser do*):

```
sudo
```

Let's create a new user for Alice:

```
sudo adduser alice
```

The system will prompt you to enter the information for `alice`. We are going to leave everything except her password empty.

```
Adding user `alice' ...
Adding new group `alice' (1001) ...
Adding new user `alice' (1001) with group `alice' ...
The home directory `/home/alice' already exists.  Not copying from `/etc/skel'.
New password: password
Retype new password: password
passwd: password updated successfully
Changing the user information for alice
Enter the new value, or press ENTER for the default
        Full Name []: <empty>
        Room Number []: <empty>
        Work Phone []: <empty>
        Home Phone []: <empty>
        Other []: <empty>
Is the information correct? [Y/n] y
```

Let's create a new group for our book club:

```
sudo groupadd book-club
```

Add alice to the `book-club`

```
sudo usermod -aG book-club alice
```

we can verify that `alice` is now part of `book-club`:

```
groups
```

### 4.2. File Permissions

We can view file permissions of `mody-dick.txt` using the `ls -l` command we learnt earlier:

```
ls -l moby-dick.txt
```

```
-rw-r--r-- 1 jovan jovan 1256529 Jan 19  2025 moby-dick.txt
```

Let's change the group ownership of the book to `book-club` and user ownership to `alice`:

```
sudo chown :book-club moby-dick.txt
sudo chown alice moby-dick.txt
```

To verify that ownership has been changed:

```
-rw-r--r-- 1 alice book-club 1256529 Jan 19  2025 moby-dick.txt
```

Let's try reading the file now:

```
cat moby-dick.txt
```

```
cat: moby-dick.txt: Permission denied
```

![[file-permissions-2-diagram.webp]]

![[file-permissions-diagram.webp]] 

Let's add ourselves to the `book-club` as well:

```
sudo usermod -aG book-club $USER
```

Now we should be able to read the file again:

```
cat moby-dick.txt
```

Note: if the file still can't be read, you might need to logout and login again.

Let's see if we can write to the file (nano/vim):

```
nano moby-dick.txt
```

or

```
vim moby-dick.txt 
```

The text editor informs us that the file is read-only. This is because as seen earlier, the group `book-club` only has read permissions.

```
-rw-r--r-- 1 alice book-club 1256529 Jan 19  2025 moby-dick.txt
```

Let's give book-club members write permission as well:

```
sudo chmod 660 moby-dick.txt
```

or

```
sudo chmod g+w moby-dick.txt
```

Now we can write to the file as well (nano/vim):

```
nano moby-dick.txt
```

or

```
vim moby-dick.txt 
```

## 5. Processes & System Monitoring
Running and killing processes

ps, top, htop, kill

Foreground vs. background jobs

&, jobs, fg, bg

## 6. Software Management

### 6.1. Package Managers

### 6.1.2 Updating, Installing, Removing Software

update packages

```
sudo apt update
```

```
sudo apt upgrade
```

```
sudo apt install neofetch cowsay
```

## 7. Redirection, Pipes & Text Processing

## 8. Networking & Remote Access

ip
ping
curl
ssh -> ssh into my personal server

## 9. Next Steps

man
searching online, chatgpt
linux journey, ubuntu docs, OverTheWire Wargames
q&a

## 10. References

- https://linuxhandbook.com/linux-file-permissions/
- 

