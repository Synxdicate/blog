# Chapter 3
Memory Allocation


Example:
```c
#include <stdio.h>
int main(int argc, char *argv[])
{
    int sum = 0, n = atoi(argv[1]);
    if (argc != 2)
    {
        return 1;
    }
    else
    {
        int *mem = calloc(n, sizeof(int));
        for (int i = 0; i < n; i++)
        {
            scanf("%d", &mem[i]);
        }
        for (int i = 0; i < n; i++)
        {
            sum += mem[i];
            printf("%d ", mem[i]);
        }
        printf("\n%d", sum);
        free(mem);
    }
}
```
