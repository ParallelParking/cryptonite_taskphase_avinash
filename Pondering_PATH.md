# Pondering PATH

There are 3 ways to invoke commands: via absolute/relative path, and via bare command name.

However, we know the bare command name is still invoking a program stored somewhere in the computer system. How does the computer know where to look for it?

PATH is an environmental variable that tells the shell which directories to search for executable files in response to commands issues by a user. It is the single most important env var.

A user's PATH consists of a series of colon separated absolute paths that are stored in plain text files. Whenever a user types in a command at the command line that is not built into the shell or does not include its absolute path, the shell searches these directories.

To get value of PATH, do env | grep PATH, or simply echo $PATH

each user may have different PATH, and root user gets a default PATH with some extra paths for programs not usually used by other users.

PATH can be changed both for a given terminal sessin and also permanently. To change temporarily, it is simply export PATH=$PATH:[programpath]

Further, these changes can be made permanent by editiing the user's .bash_profile file.

*Challenge 1: pwn.college{Y1DE9fVWl1GR5e6BNUfM2Kp7kp1.dZzNwUDLwEDN1czW}*

*Challenge 2: pwn.college{0oWqvprDFDABiyJU_DDP_COkX2y.dVzNyUDLwEDN1czW}*

*Challenge 3: pwn.college{ss0kTUMy7loEl9JOxvf7Ke7xEjq.dZzNyUDLwEDN1czW}*

Little bit of thinking on this!

```bash
$ touch win
$ vim win
$ export PATH=$PATH:/home/hacker
$ chmod 777 win
$ /challenge/run
```

*Challenge 4: echo -e '#!/bin/bash\ncat /flag' > ~/my_custom_bin/rm*

### Solving challenge 4:

I created a new rm file in my home directory, wrote a shell script on it to cat /flag, then prepended the home directory to my path variable. I then called /challenge/run. It found rm in my home before in the respective bin, executed it, and handed me the flag.
