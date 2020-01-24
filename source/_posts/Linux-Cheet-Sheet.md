---
title: Linux Cheet Sheet
date: 2020-01-24 12:34:22
tags:  
- Linux
- Cheet Sheet
categories:
- [Cheet Sheet]
---
## Global
- **man command** - check the `man` page of `command`
-  **command1 | command2** - execute `command2` with `command1`'s output as its input

## File
- **ls -la** - formateed listing directory with hidden files
-  **cd dir** - change directory to dir
-  **cd** or **cd ~** - change to home
-  **cd /** - go to the root directory
-  **cd -** - go to the last directory you were just in
-  **pwd** - show present working directory
-  **mkdir dir** - create a directory dir
-  **rm file** - delete file
-  **rm -rf dir** - force delete directory dir 
-  **cp file1 file2** - copy file1 to file2
-  **cp -r dir1 dir2** - copy dir1 to dir2, create dir2 if it doesn't exit
-  **mv file1 file2** - rename or move file1 to file2, if file2 is an existing directory, moves file1 into directory file2
-   **ln -s file link** - create symbolic link `link` to `file`
-   **touch file** - create or update file
-   **cat file** -output the contents of file
-   **less file** - 
-   **head file** - output the first 10 lines of file
-   **head file -n 20** - output the first 20 lines of file
-   **tail file** - output the last 10 lines of file
-   **tail -f file** - output the contents of file as it grows
-   **alias name 'command'** - create an alias for `command`

## System
- **shutdown** - shut down machine
- **reboot** - restart machine
- **date** - show the current date and time
- **cal** - show this month's calendar
- **w** - display who is online
- **whoami** - who you are logged in as
- **uname -a** - show kernel information
- **cat /proc/cpuinfo** - cpu information
- **cat /proc/meminfo** -memory information
- **df -h** - show disk usage, human readable
- **du -h** - show directory space usage
- **free** - show memory and swap usage
- **whereis app** - show possible location of app
- **which app** - show which app will be run by default

## Search
- **grep pattern file1 file2** - search for pattern in `file1` and `file2`
-  **grep -rn pattern dir** - search recursively for pattern in dir and show the line number found
-  **grep -r pattern dir --include='*.ext** search recursively for pattern in dir and only search in files with `.ext` extension
-  **find file** - find all instances of file in real system
-  **locate file** - find all instances of file

## Process Management
- **ps** - display your currently active processes
- **ps -aux** list 
- **top** - display all running processes
- **kill pid** kill process id `pid`
- **kill -9 pid** - force kill process
-  **killall proc** - kill all processes named `proc*`
-   **bg** - list stopped or background jobs
-   **fg n** - brings the most recent job to foreground
- **nohup ./run.sh &** - run `./run.sh` in the background

## Networking
- **ping host** - ping host
- **whois domain** - get whois information for domain
- **dig domain** - get DNS information for domain
- **dig -x host** - reverse lookup host
- **wget file** - download file
- **wget -c file** - continue a stopped download
- **curl file** - download file
- **lsof -i tcp:1337** - list all processes running on port 1337
- **scp user@host:file dir** - secure copy a file from remote server to the dir directory on your machine
- **scp file user@host:dir** - secure copy a file from your machine to the dir directory on a remote server
- **scp -r user@host:dir dir** - secure copy the directory dir from remote server to the directory dir on your machine

## Shortcuts
- **Ctrl+a** - move cursor to beginning of line
- **Ctrl+f** - move cursor to end of line
- **alt+f** - move cursor forward one word
- **alt+b** - move cursor backward one word
- **Ctrl+C** - halts the current command
- **Ctrl+Z** - stops the current command, resume with `fg` in the foreground or `bg` in the background
- **Ctrl+W** - erase one word in the current line
- **Ctrl+U** - erase the whole line
- **Ctrl+R** - type to bring up a recent command
- **Ctrl+D** or **exit** - logout of current session

## Permissions
- **chmod octal file** - change the permissions of file, which can be updated separately for user, group and all by adding:
```
4 - read (r)
2 - write (w)
1 - execute (x)
```
- **chmod +x file** - add execute permission to file for the current user
- **chmod 777** - read, wirte and execute permission for all
- **chmod 755** - rwx for owner, rx for group and others
- **chmod 700** -rwx for owner, none for group and others

## Compression
- **tar cf file.tar files** - create a tar named `file.tar` containing files
- **tar xf file.tar** - extract the files from file.tar
- **tar czf file.tar.gz files** - create a tar with Gzip commpression
- **tar xzf file.tar.gz** - extract a tar using Gzip
- **tar cjf file.tar.gz** - create a tar with Bzip2 compression
- **tar xjf file.tar.bz2** - extract a tar using Bzip2
- **gzip file** - compress file and remames it to `file.gz`
- **gzip -d file.gz** - decompress file.gz back to file

## SSH
- **ssh user@host** - connect to host as user
- **ssh user@host -i id_rsa** - connect to host with private key `id_rsa`
-  **ssh -p port user@host** connect to host on port `port`
-  **ssh-copy-id user@host** - add your key to host for user to enable password-less login

## Installation
- **dpkg -i pkg.deb** - install a package (Debian)
- **rpm -Uvh pkg.rpm** - install a package (RPM)
- **./configure**
- **make**
- **make install**

## Links
-  http://cheatsheetworld.com/programming/unix-linux-cheat-sheet/
-  https://files.fosswire.com/2007/08/fwunixref.pdf