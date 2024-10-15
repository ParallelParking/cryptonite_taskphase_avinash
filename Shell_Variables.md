# SHELL VARIABLES

Variables in Bash don't have data types, but can contain a number, character, or string. There is no declaration process, just assigning a value to the reference implicitly creates one.

```bash
$ str="Hello world!"
$ echo $str
Hello world!
```

Note here that the '$' is crucial in retrieving the value stored in str. Think of it as the dereferencing operator in C.

Further, command substitution can be used in conjunction with $. For example 'echo $(ls)' accomplishes the same thing ls would, whereas echo ls just prints ls.

https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-5.html

## 'echo'

Prints out the argument. Can print variable values when var prepended with $.

*Challenge 1: pwn.college{wvK720ppSivfkXgf6bzMj6yxqHC.ddTN1QDLwEDN1czW}*

-------------------------------------------------

NOTE: unlike most programming languages, the bash script is *whitespace sensitive*. I cannot put spaces in places where it would improve readability.

NOTE: The operator '$' performs what is called a "variable expansion" or a "parameter expansion". I've written about this in the File Globbing markdown. This var expansion is the source of many vulnerabilities.

*Challenge 2: pwn.college{ATY79P27uvJxbfGzQDS9XSx_kme.dlTN1QDLwEDN1czW}*

More specifically tackling the whitespace sensitivity, to handle a situation where we want multiple words to be read as a single token, they need to wrapped into a string, i.e. say we want to do VAR=Hello World, it'll store Hello into VAR and try interpreting World. Instead, VAR="Hello World" performs intended action.

*Challenge 3: pwn.college{A7827aD20F65pkHEejwrZZwxUWy.dBjN1QDLwEDN1czW}*

## Variable locality

By default, variables remain within t he scope of the current shell process, and are not inherited by other shell processes.

Think of them as private instance variables in a superclass. These properties are not inherited by subclasses.

'sh' - Short command to invoke another shell process wihin our own shell

This is a safety procedure, you don't want sensitive contents stored in vars leaked to other processes.

For this reason, variables are given only to the processes that explicitly *need* them. This is done via a process known as *variable exporting*.

The process of variable exporting causes these variables to be passed as *environment variables* to any child processes.

NOTE: environment variables are any vars whose value is set outside the program.

https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa

This is done via 'export' keyword

```bash
$ VAR=1337
$ export VAR
$ sh
$ echo "VAR is: $VAR"
VAR is: 1337
```

Alternatively, lines 1 and 2 can be combined to be 'export VAR=1337'

*Challenge 4: pwn.college{Ii-L-XtTcHYozsMDaRCNMd5euzb.dJjN1QDLwEDN1czW}*

'env' command prints every exported variable set in she shell.

*Challenge 5: pwn.college{km4JFpu_5Z3oUArpBGtaTwW0hWf.dhTN1QDLwEDN1czW}*

### Solving challenge 5:

```bash
$ env | grep FLAG
```

## Command Substitution

As stated earlier, command substitution enables the storage of command output into a variable.

Further, it is old practice to use \`thing\` instead of (thing), however this is not recommended.

*Challenge 6: pwn.college{8YqEQ9wIwhfJYL4F-4wKNT2qOfe.dVzN0UDLwEDN1czW}*

### Solving challenge 6:

```bash
$ PWN=$(/challenge/run)
$ echo $PWN
```

## Reading input using 'read'

the -p "string" argument to read provides a prompt to the user.

```bash
$ read -p "Input: " VAR
Input: Hello!
$ echo $VAR
Hello!
```

As is to be expected, 'read' utilises the stdin channel.

*Challenge 7: pwn.college{0iBljXtFOzv9I4Z_MuucazYjdM-.dhzN1QDLwEDN1czW}*

NOTE: it is bad practice to run a whole other program in service of another when there exists a way to do it without this redundancy. An example is running cat to read the contents of a file into a variable.

An alternative to the cat problem is redirecting input via the '<' operator, doing 'read VAR < filename'

*Challenge 8: pwn.college{wlqWjtcH-wot78a4WsxeCZSlUNL.dBjM4QDLwEDN1czW}*


