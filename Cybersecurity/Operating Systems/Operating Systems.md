## Files (Unix based)
- `-` for regular file
- `d` for directory (directories are basically just special file)
- `l`  -> symbolic link (transparently points to another file/directory)
- `p` -> named pipe (something related to FIFO?)
- `c` -> character device file (represents a hardware device as a special file that produces/receives a data stream )
- `b` -> block device file (represents a H/W device that stores and loads blocks of data, such as a hard drive)
- `s` -> Unix socket (essentially a network socket encapsulated in a file)
## File descriptors and Named Pipes
---
Unix-based systems, such as Linux, follow the philosophy "everything is a file" i.e. the system provides file-based access to most resources, including the input/output of running processes.
This system can be used to pass, output of a running program to anything which takes in a file.
#### FIFO
This is kind of a named pipe except instead of creating a temporary one, this is persistent. The data doesn't persist and the read/write is blocked until both of the operations are online, such as if `echo` is outputting something, but no one is reading it, then it will be blocked and vice versa. 
This is good for creating complex data flows between commands.

## Processes  
---
In Linux, all the process are stored as a blob of data in `/proc` in a certain format which contains the real, effective and saved UID (difference?)\[Permission Model\], it's own PID, the file descriptors open for that process and the memory mappings of the program, like where the libraries and the program are loaded.
General structure:-
![[Pasted image 20260116111207.png]]
