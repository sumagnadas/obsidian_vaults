[[Wireshark]]
[[GDB]]
## Usage
There are always built-in shell commands so they can used in case of no other commands. Check them out for further reference.
* `echo` is a builtin and `echo *` can be used to print all the files' names.
### Redirection/piping
- `(FD_no)> (filename)` / `(command) <(FD_no)`
	- Redirects data from file descriptor `FD_no` to `filename` -> stdin(0), stdout(1), stderr(2)
	- By default, 1 for output and 0 for input (2nd case)
- `&>` -> redirect both `stderr` and `stdout` (syntax similar to 1st one except for the `&` part)
- `(FD_No)>& (FD_No_2)` -> Redirects data from file descriptor `FD_No` to file descriptor `FD_No_2`
### Named Pipes
In Linux, the way to do this is `<(command)` which gives a temporary named pipe, generally in the format of `/dev/fd/<num>`, which can be passed as a normal file, though it is not a file. Also this is the stdout of the command that gets returned

For the `stdin` of a command, we can do `>(command)`, which gives the named pipe/fake file version of the `stdin` of the `command`.

Creating a FIFO ([[Operating Systems#FIFO|Theory]]) -> `mkfifo` and then use it like a normal named pipe.

***Note: Use this only where necessary to not create confusions***
### Process Substitution vs Command Substitution
We can get the output of a command, in a variable using `$(command)` format(command substitution) while we get a named pipe when we want to get output via process substitution. Although similar fundamentally that we get the output of the command in both the methods, in command substitution, we get the output after the command/subshell has completed while in process substitution, we get the same, while the output is actually happening. 
### su (old and obsolete)
- `su` ->Switch user
- Has `suid` set to `root`
- Switches to root by default after getting the root password (which most computer don't have now and hence its mostly obsolete)
- Can be used to switch to other user using `su some-user` and then giving its password.
### chgrp
- for changing the group of an user 
- mostly for root user
### grep
- `-v` -> invert the match after it
- `-i` -> ignores case of search string
### tee
**Use:** T-splitter for piped data. Basically, we can pipe the data into files before sending it to the next command. It reads from `stdin` and sends the output to files AND `stdout`, instead of just the files.
You can get the output in a file AND send it to a command at the same time.

As we send the output to another file, we can also use a named pipe/fake file in place of it to pass it to another command, basically piping the `stdout` of one command to more than one different commands at the same time.
### tr
- General use -> give a character or set of characters which you want to replace and another set of characters which it will be replaced with. It basically associates and replaces the character with that in the second set.
  Eg: `tr A-Za-z a-zA-Z` swaps the cases
- `-d` -> Delete the characters specified
### John The Ripper
- Can be used to crack passwords from a leaked `/etc/shadow` file
### strace
- Usage -> `strace /path/to/program_executable`
- Used to trace system calls when a program is executed