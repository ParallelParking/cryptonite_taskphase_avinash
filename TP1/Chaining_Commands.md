# Chaining Commands

## ;

Commands can be chained by separating them with a semi colon.

*Challenge 1: pwn.college{Mquak3wz_dM2C3L36ZlhkF-aImY.dVTN4QDLwEDN1czW}*

To avoid annoyance writing long sets of commands on a single command line, they can be written into a file called a shell script. They can be run by simply executing the file.

These shell scripts can be executed by passing them as an argument to the bash command, which causes the bash to read commands from the file instead of taking commands from user.

*Challenge 2: pwn.college{Yvm-r2_RqjXFFEne6wY9pGONkak.dFzN4QDLwEDN1czW}*

NOTE: all redirectors work with shell scripts. For example: bash script.sh > output; cat output will print the output given by the script.sh file.

*Challenge 3: pwn.college{Ut8h51cgM3b7REYXIy7mPGmUnUB.dhTM5QDLwEDN1czW}*

NOTE: The use of the bash command can be circumvented by changing the file's access mode to be an executable, in which case it can be run by simply mentioning its relative/absolute path

*Challenge 4: pwn.college{4UDuVfPiKN6q-2sUnJLrqbwkqQ1.dRzNyUDLwEDN1czW}*


