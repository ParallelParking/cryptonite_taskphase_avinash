# Processes and Jobs

The software computers use are broadly split into two kinds: operating system kernels, and processes.

When linux starts, it launches the init process, that in turn starts other processes, etc.

## 'ps'

https://www.shiksha.com/online-courses/articles/understanding-ps-command-in-linux/

Stands for 'process status', lists all active processes running in terminal without args.

Each process has a 'PID', a process identifier

Further, each process has a 'TTY', the terminal associated with that given process, a 'TIME' that is  the amount of CPU time used by each process and the 'CMD', which is the executable command used to start each process.

-e: argument to list *every* process that is currently running

-f: prints a full format output, including arguments etc.

Sometimes it is  necessary to use both of these together. -ef combines their functionalities.

### BSD syntax

a: lists processes for all users

u: lists processes in user readable format

x: lists processes not running in a terminal

these can be strung together, for example 'ps aux' does all 3.

Therefore, it can be seen that ps -ef and ps aux achieve the same things but there are some key differences.

PPID: displayed only by ps -ef, the PID of the process that launched the process in question, i.e. the parent process' PID

VSZ in ps aux: total size the process occupies in virtual memory (Virtual Set Size)

RSS in ps aux: Shows how much RAM utilized by process at time of command output (Resident Set Size)

STAT column in ps aux: represents current state of process. May be any of the following:

```bash
D    uninterruptible sleep (usually IO)
I    Idle kernel thread
R    running or runnable (on run queue)
S    interruptible sleep (waiting for an event to complete)
T    stopped by job control signal
t    stopped by debugger during the tracing
W    paging (not valid since the 2.6.xx kernel)
X    dead (should never be seen)
Z    defunct ("zombie") process, terminated but not reaped by its parent
```

Further, the additional character represents:

```bash
<    high-priority (not nice to other users)
N    low-priority (nice to other users)
L    has pages locked into memory (for real-time and custom IO)
s    is a session leader
l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
+    is in the foreground process group
```

https://askubuntu.com/questions/360252/what-do-the-stat-column-values-in-ps-mean

### Challenge 1:

*Challenge 1: pwn.college{YzfujJ3OoqNGsKha-obDtDRA6i-.dhzM4QDLwEDN1czW}*

```bash
$ ps aux | grep /challenge
root          69  0.0  0.0   4132  2560 ?        S    13:55   0:00 /challenge/13604-run-20731
hacker        94  0.0  0.0   4140  2240 pts/0    S+   13:55   0:00 grep --color=auto /challenge
$ /challenge/13604-run-20731
```

## 'kill'

terminates a process before it completes and terminates itself.

done by passing the PID of desired process to kill as arg to kill command

SIDENOTE: the 'sleep' command takes seconds as an argument and will just stay as a process for that amount of time, doing nothing.

*Challenge 2: pwn.college{YUGo2mpSpBvQtDFK3bSwLR3szzU.dJDN4QDLwEDN1czW}*

## Interrupting processes

to interrupt any application waiting on command line for input, doing ctrl+c. It will terminate the program allowing it to cleanly exit.

*Challenge 3: pwn.college{EmUENdXeJJi2VyMJXZJfQsMmOZj.dNDN4QDLwEDN1czW}*

## Suspending processes

Interrupts cleanly kill processes. This is an extreme measure. We can relegate a process to the background with ctrl+z, which is called suspending the process.

## How does my terminal read control inputs?

https://catern.com/posts/terminal_quirks.html

## 'fg'/'bg'

Resumes suspended processes.

*Challenge 4: pwn.college{09vcdNs5KI8w4IMUpoT_fqjqV3z.dZDN4QDLwEDN1czW}*

NOTE: fg stands for 'foreground'. It resumes a suspended process into the command line, whereas bg stands for 'background', which resumes the process without taking over the shell.

To note the actual difference between a suspended process and a backgrounded process: one will not proceed with any further execution in the background while the other will, further if no execution of the background process is possible (for eg. needs user input) it will put itself to sleep (i.e. waits) until it can continue.

*Challenge 5: pwn.college{02EzbvW5EUtWkTWrquUWnquRFBU.ddDN4QDLwEDN1czW}*

Similar to foregrounding a suspended process, a backgrounded process can also be brought to the foreground with the fg command.

*Challenge 6: pwn.college{09vcdNs5KI8w4IMUpoT_fqjqV3z.dZDN4QDLwEDN1czW}*

*Challenge 7: pwn.college{0jjOJNFRaannwOrJ2zlU_9QM0ks.dhDN4QDLwEDN1czW}*

To start a new process in an already backgrounded state, append an & to the end of the command

*Challenge 8: pwn.college{Iu8vicyqDr6J05IOQEWX3RTVTJe.dlDN4QDLwEDN1czW}*

## Exit code

Every process exits with an exit code when it finishes running and terminates. This can be outputted to check if the process succeeded in its functionality.

The process exit code of the most recently terminated command can be accessed via the special '?' variable, so 'echo $?' prints process exit code.

Typically, '0' exit code indicates successful execution whereas '1' indicates failure, however there may be more specific non-zero value to inicate specific failure modes.

*Challenge 9: pwn.college{UaGv9h8dcuISvtNZJzpZBhaDyGt.dljN4UDLwEDN1czW}*

