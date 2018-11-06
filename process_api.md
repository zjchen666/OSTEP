## Homework
Q1: Write a program that calls fork(). Before calling fork(), have the main process access a variable (e.g., x) and set its value to something (e.g., 100). What value is the variable in the child process? What happens to the variable when both the child and parent change the value of x? 

Output: 
child process 100
child process 101
parent process 100
parent process 102

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
