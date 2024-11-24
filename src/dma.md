# Dynamic Memory Allocation (maintaining)
`stdlib.h` ใช้ฟังก์ชัน 4 ตัวหลัก ๆ
1. `malloc()`
2. `calloc()`
3. `free()`
4. `realloc()`<br>

`malloc()` `calloc()` - return เป็น `void*`

## malloc
`malloc` หรือ memory allocation ใช้ dynamically allocate จะจองพื้นที่เฉยๆ (initialized) ดังนั้นข้อมูลจะเป็นอะไรก็ได้ **garbage value**
prototype: 
```c
void *malloc(size_t size);
```
> int = 4 bytes
```c
int* ptr = (int*)malloc(5*sizeof(int));
```
จอง memory `5*4` bytes ละก็เก็บ address byte แรก
ถ้าพื้นที่ไม่พอจะ return เป็น NULL pointer

garbage value

## calloc
`calloc` คล้ายกับ `malloc` แต่จะจองพื้นที่ละก็เซ็ตค่าทุก bit ที่จองให้เป็น 0
```c
ptr = (type*)calloc(n, size);
```

## free

`free` ทุกครั้งหลักเสร็จจากการใช้ allocated memory แล้วให้ de-allocated เพื่อกัน memory leak
```c
void free(void *ptr);
```

```c
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char* argv[]){
        int* ptr;
        int n = 0;
        scanf("%d",&n);
        ptr = (int*)malloc(n * sizeof(int));
        if(ptr == NULL){
                printf("false");
                exit(0);
        }
        else {
                printf("true\n");
                for(int i = 0;i < n; i++){
                        if( i == n ){
                                printf(" %d",ptr[i]);
                        }
                        printf("%d, ",ptr[i]);
                }
                printf("\n");
                free(ptr);
                return 0;
        }
        return 0;
}
/* fix code now */
```

## realloc

`realloc` re-allocation 
```c
void *realloc(void* ptr, size_t size);
```
เงื่อนไข
ถ้า `(size)` มีขนาดน้อยกว่าอันเดิม จะลดขนาดตามที่กำหนด
`ptr` เป็น NULL จะทำงานเป็น `malloc(size)` แทน
`ptr` เป็น 0 จะเป็นการ `free()`
```c
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char* argv[]){
        int* ptr;
        int n = 5;
        ptr = (int*)malloc(n * sizeof(int));
        if(ptr == NULL){
                printf("false");
                return 1;
        }
        printf("true\n");
        ptr[1] = 6;
        for(int i = 0;i < n; i++){
                printf("%d, ",ptr[i]);
        }
        printf("\n");
        int n2 = 10;
        int j = n + n2;
        ptr = (int*)realloc(ptr, j * sizeof(int));
        for(int i = 0;i < j+5; i++){
                printf("%d:%d, ",i,ptr[i]);
        }
        free(ptr);
        return 0;
}
```

ถ้า value มีขนาดใหญ่กว่า stack มันจะเก็บไว้ใน heap

```cpp
#include <iostream>
#include <vector>
int main(int argc, char* argv[]){
    if(argc != 2){
        return 1;
    }else{
        std::vector<int> mem = {};
        int a = atoi(argv[1]),sum={};
        for(int i = 0; i < a; i++){
            int n;std::cin>>n;
            mem.push_back(n);
        }
        for(int i:mem){
            sum+=i;
            std::cout << i << " ";
        }
        std::cout << "\n" << sum;
    }
}
```

```c
#include <stdio.h>
#include <stdlib.h>
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

