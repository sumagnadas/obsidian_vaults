
| code | syscall name                                                                       | rdi          | rsi                    | rdx               | r10 | r8  | r9  |
| ---- | ---------------------------------------------------------------------------------- | ------------ | ---------------------- | ----------------- | --- | --- | --- |
| 0    | [[Instructions#read(int fd, char *buf, size_t count)\|read]]                       | int fd       | char \*buf             | size_t count      |     |     |     |
| 1    | [[Instructions#write(int fd, char *buf, size_t count)\|write]]                     | int fd       | char \*buf             | size_t count      |     |     |     |
| 2    | [[Instructions#open(char *path, int flags)\|open]]                                 | char \*path  | int flags              | umode_t mode      |     |     |     |
| 3    | [[Instructions#close(int fd)\|close]]                                              | int fd       |                        |                   |     |     |     |
| 41   | [[Instructions#socket(int domain, int type, int protocol)\|socket]]                | int domain   | int type               | int protocol      |     |     |     |
| 43   | [[Instructions#accept(int fd, struct sockaddr *addr, socklen_t *addrlen)\|accept]] | int fd       | struct sockaddr \*addr | socklen_t addrlen |     |     |     |
| 49   | [[Instructions#bind(int fd, struct sockaddr *addr, socklen_t addrlen)\|bind]]      | int fd       | struct sockaddr \*addr | socklen_t addrlen |     |     |     |
| 50   | [[Instructions#listen(int fd, int backlog)\|listen]]                               | int fd       | int backlog            |                   |     |     |     |
| 57   | [[Instructions#fork()\|fork]]                                                      |              |                        |                   |     |     |     |
| 60   | [[Instructions#Syscalls#exit(int ret_code)\|exit]]                                 | int ret_code |                        |                   |     |     |     |
