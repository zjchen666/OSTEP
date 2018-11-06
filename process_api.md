
## Homework
Q1: Write a program that calls fork(). Before calling fork(), have the main process access a variable (e.g., x) and set its value to something (e.g., 100). What value is the variable in the child process? What happens to the variable when both the child and parent change the value of x? 

Output: 
```
child process 100
child process 101
parent process 100
parent process 102
```

Code:
```cpp
#include <unistd.h>
#include <stdio.h>

int main(void)
{
    int val = 100;
    int rc = fork();
    if (rc < 0) {
        printf("fork failed\n");
    } else if (rc == 0) {
        printf("child process %d\n", val);
        val = 101;
        printf("child process %d\n", val);
    } else {
        printf("parent process %d\n", val);
        val = 102;
	printf("parent process %d\n", val);
    }	
    return 0;
}

Q2 Write a program that opens a ﬁle (with the open() system call) and then calls fork() to create a new process. Can both the child and parent access the ﬁle descriptor returned by open()? What happens when they are writing to the ﬁle concurrently, i.e., at the same time? 

Output:
   parent write overwrite child write.
   
```cpp
#include <unistd.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <errno.h>

const char* filename = "/home/chenzj/code/linux_study/test";

int main(void)
{
    int rc = fork();
    int fd = open(filename, O_WRONLY|O_CREAT, 0644);
    ssize_t size = 0;

    if (rc < 0) {
	    printf("fork failed\n");
    } else if (rc == 0) {
        char s[] = "111111";
        size = write(fd, s, sizeof(s));
        close(fd);
        printf("child process %d, %d\n", size, errno );
        //execlp("/bin/ls", "./");
    } else {
        wait();
        char s[] = "222";
        size = write(fd, s, sizeof(s));
        printf("parent process %d, %d\n", size, errno );
        close(fd);
    }
	
    return 0;
}
```
