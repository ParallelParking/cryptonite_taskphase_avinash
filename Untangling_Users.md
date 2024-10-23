# Untangling Users

Full list of users on a linux system stored in /etc/passwd

It outputs a long list of all users on the linux system, with a lot of info in each row separated by ':'s

Each row contains the username, an 'x' as placeholder for the password, the numerical User ID, the default group ID, long form user details, the home directory and the default shell.

Apart from the user account, there is a root account, a nunch of them for historical reasons, some service accounts to support various installed software, utility accounts etc.

## 'su'

Standsd for 'switch user'. The most common application of this is when you want to switch to root user, which you may want to do for a number of reasons.

It is not typically used to elevate access annymore, but is an elegant utility.

su is a SUID binary, i.e. it has 's' set in the x bit of the owner access permissions thing in ls -l /usr/bin/su

su can start a root shell, however it will not do this without getting the root password off the user.

However, modern systems rarely have root passwords and different mechanisms are used to grant admin access.

*Challenge 1: pwn.college{Qi06WwxtvIvIOirICnSXj7bUx12.dVTN0UDLwEDN1czW}*

su starts a root shell when not provided any arguments, however it can also be used to switch to any other user when that username is provided as argument.

*Challenge 2: pwn.college{ELR4BktbaPElJTAL4wcUWzze6Tk.dZTN0UDLwEDN1czW}*

---

We noted earlier that in the passwd file, the password field actually contained just an 'x' as a placeholder. So where is the password then? It is stored in a file that can only be read by root user called /etc/shadow

It is a very similarly structured file, but here instead of an 'x' the password field contains a one way encrypted string of the user's password.

If the password field contains a * or a !, it functionally means that password login on that account is disabled.

When a password is entered, it is encrypted and matched against the respective encrypted password in /etc/shadow. If the strings match, password is correct.

Backups of /etc/shadow are often stored with insufficient protection on file servers etc. causing data leaks. A capable hacker can use these to crack passwords, even with the encryption.

## John the Ripper

Free and open source software in linux that can crack easy to guess passwords via brute force. takes the /etc/shadow file as an argument

*Challenge 3: pwn.college{IsjWs9vIiexl35VYUs857gwZlk7.ddTN0UDLwEDN1czW}*

## 'sudo'

Stands for 'superuser do'. Instead of dealing with complications arising from having a root user. Where su launches a new shell as root user, sudo runs that specific command as root.

Obviously, there are restrictions on what commands a user caan ru as root. These authorizations a user has are defined in /etc/sudoers

*Challenge 4: pwn.college{AkXnj5Ju1kcBM1KPZ6z1RCRLfJk.dhTN0UDLwEDN1czW}*


