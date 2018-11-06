##Homework

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
        wait();
        //execlp("/bin/ls", "./");
    } else {
        wait();
        printf("parent process %d\n", val);
        val = 102;
	    printf("parent process %d\n", val);
    }	
    return 0;
}
```
