
| code | syscall name                                                   | rdi          | rsi        | rdx          | r10 | r8  | r9  |
| ---- | -------------------------------------------------------------- | ------------ | ---------- | ------------ | --- | --- | --- |
| 0    | [[Instructions#read(int fd, char *buf, size_t count)\|read]]   | int fd       | char \*buf | size_t count |     |     |     |
| 1    | [[Instructions#write(int fd, char *buf, size_t count)\|write]] | int fd       | char \*buf | size_t count |     |     |     |
| 2    | [[Instructions#open(char *pathname, int flags)\|open]]         | char \*path  | int flags  |              |     |     |     |
| 60   | [[Instructions#Syscall#exit(int ret_code)\|exit]]              | int ret_code |            |              |     |     |     |
