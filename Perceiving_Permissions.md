# Perceiving Permissions

Different files have different permissions/file modes in Linux

## 'ls -l'

Allows you to see the permissions of a file or directory.

Sample use:

```bash
hacker@dojo:~$ mkdir pwn_directory
hacker@dojo:~$ touch college_file
hacker@dojo:~$ ls -l
total 4
-rw-r--r-- 1 hacker hacker    0 May 22 13:42 college_file
drwxr-xr-x 2 hacker hacker 4096 May 22 13:42 pwn_directory
hacker@dojo:~$
```

First character of each line: indicates file type. 'd' indicates directory, whereas '-' indicates normal file.

Next 9 characters of each line: access permissions of file/dir. Split into: 3 characters denoting permissions that the user (termed owner) has to the file, 3 characters denoting what permissions the group that owns the file has to the file, and finally 3 characters describing what permissions that all other accessors (other users/other groups) have to the file.

The next two columns of output describe the user owner and the group owner of the file.

## File ownership in Linux

Every file in a Linux system is owned by a user. The root account is administrative account. Further, linux has many "service" user accounts for different specific tasks.

This is how we're banned from reading the flag files straight. Only the root user is given read permissions, and when we access the flag program the intended way, it is invoked via root user.

## 'chown'

stands for 'change owner'. Syntax is: 'chown [username] [file]' chown can typically only be invoked by root user.

*Challenge 1: pwn.college{kK3h-L_8pOYqzo-N7sp0an20tC5.dFTM2QDLwEDN1czW}*

---

Note that Linux files have both a user owner and a group owner. Groups can have multiples users and a user can belong to multiple groups.

## 'id'

Tells current user what groups they're a part of.

Groups help control access to different system resources

NOTE: graphical output is done via the /dev/fb0 file, which is given file type 'c' in the ls -l output. This 'c' denotes a 'character device' i.e., this file is a special 'device file'.

## 'chgrp'

Stands for 'change group', functions identically to chown

*Challenge 2: pwn.college{I-PJ54Z3QC6iLt2UGbYN5bBDfhg.dFzNyUDLwEDN1czW}*

NOTE: it is convention that every user in a linux system has their own group, however this does not always have to be the case.

*Challenge 3: pwn.college{08NWPyBHDLZ0V3MN0TqBOFVqFtg.dJzNyUDLwEDN1czW}*

---

Earlier, we discussed how 9 characters are used to assign permissions to each tier of file accessor : owner, group, other. Each were given 3 characters in the file access description for a total of 9.

For those 3 characters, there are the following meanings:

r - can read the file or list the directory
w - can write to the file or create/delete files in the directory
x - can execute file as program or enter the directory using cd
\- - nothing

eg. rw-r--r--: owner can read and write file, group and others can only read. None of them can execute it.

## 'chmod'

Stnads for 'change mode' allows you to change file access mode.

Syntax: chmod [OPTIONS] MODE FILE

MODE can be specified in 2 ways: a modification of existing permissions or completely overwriting the old mode.

Considering the former, modifications can be done via general form WHO+/-WHAT, where WHO is owner/group/other and WHAT is r/w/x

eg. u+r gives user read permissions, g+wx gives group write and execute permissions

*Challenge 4: pwn.college{E4M5cjGxrcpNdaHJ_yBzE5AwXr3.dNzNyUDLwEDN1czW}*

```bash
$ chmod o+r /flag
$ cat /flag
```

*Challenge 5: pwn.college{wa4r_UL6jAZ3aKqDVIx3Z0Di5ai.dJTM2QDLwEDN1czW}*

To modify the access mode absolutely: each of the 3 character pairs are considered a 3 digit binary number (rwx, r = 4, w = 2, x = 1, - = 0). This set of 9 characters can thus be converted into a 3 digit octal number.

*Challenge 6: pwn.college{0CXN2ZBQCfFEmY4WQTCXiiluT6Y.dBTM2QDLwEDN1czW}*

Further, chmod can use '=' to set permissions as such: u=rw, a=rwx.

These assignments can be comma separated for the same command, eg. u=rw,g=r

*Challenge 7: pwn.college{U3GXrU_cnps0G415g3ofSfr_RT8.dNTM5QDLwEDN1czW}*

## SUID

When a non-root user needs elevated access to do a system task, the SUID or 'set user ID' permissions bit allows the user to run a program as the owner of that program's file

the SUID bit replaces the executable bit in the owner 3-character sequence. For example: -rwsr-xr-x, here s replaces the owner x, so any user with executable permissions will run this file as owner.

chmod can be used to set SUID bit with u+s

*Challenge 8: pwn.college{sVkjgPsuVxWqA0To4E_rFom2XG1.dNTM2QDLwEDN1czW}*


