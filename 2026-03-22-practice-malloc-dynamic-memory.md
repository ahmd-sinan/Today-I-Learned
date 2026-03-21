# Practice: Dynamic Memory Allocation (`malloc`) 

**Date:** 2026-03-22

Today I did a deep dive into `malloc()`. I moved away from fixed-size arrays and started requesting memory from the **Heap** dynamically while the program is running.

## 1. Single Pointer: Heap vs. Stack 
**Goal:** Allocate memory for a single integer, assign a value, and prove that the pointer and the data live in two completely different places in RAM.

**Code:**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr  = (int*) malloc(sizeof(int));

    if(ptr == NULL) {
        return 1;  // Exit with error code
    }

    *ptr = 100;
    // Dereferencing pointer to get value stored in Heap memory
    printf("The value is: %d\n", *ptr);                                 
    // Address of allocated memory
    printf("Address of memory (Heap): %p\n", (void*)ptr);           
    // Address of pointer variable itself
    printf("Address of ptr itself (Stack): %p\n", (void*)&ptr);     

    free(ptr);
    ptr = NULL;   // Avoid dangling pointer

    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
The value is: 100
Address of memory (Heap): 0x55bc1a2f92a0
Address of ptr itself (Stack): 0x7ffd8b9a1b48
```
## 2. Dynamic Array & The Garbage Truth 
**Goal:** Ask the user for an array size, allocate it, and print the raw memory before the user types anything to see the "garbage" values left behind by old programs.

**Input:** Size `3`, Elements: `10`, `20`, `30`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter the size of array: ");
    scanf("%d", &n);

    int *arr = (int*) malloc(n * sizeof(int));

    if(arr == NULL) return 1;

    printf("\nValues stored in the array (Garbage):\n");
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
    }

    printf("\n\nEnter values to store in array:\n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("\nValues stored in the array (User Input):\n");
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
    }

    printf("\n\nAddress of allocated memory (Heap): %p\n", (void*)arr);
    printf("Address of pointer variable itself (Stack): %p\n", (void*)&arr);

    free(arr);
    arr = NULL;
    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter the size of array: 3

Values stored in the array (Garbage):
0	1041239	-8493012

Enter values to store in array:
10 20 30

Values stored in the array (User Input):
10	20	30

Address of allocated memory (Heap): 0x558a9c1b22a0
Address of pointer variable itself (Stack): 0x7fff3a2b1c40
```

## 3. Dynamic Array Math: Sum 
**Goal:** Allocate a dynamic array and calculate the total sum of all elements.

**Input:** Size `4`, Elements: `5`, `10`, `15`, `20`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    int sum = 0;
    printf("Enter array size: ");
    scanf("%d", &n);

    int *arr = (int*) malloc(n * sizeof(int));
    if(arr == NULL) return 1;

    printf("Enter array elements: \n");
    for(int i = 0; i<n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("Array elements are: \n");
    for(int i = 0; i<n; i++) {
        printf("%d\t", arr[i]);
        sum += arr[i];
    }

    printf("\nSum of array is: %d\n", sum);
    free(arr);
    arr = NULL;
    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter array size: 4
Enter array elements: 
5 10 15 20
Array elements are: 
5	10	15	20	
Sum of array is: 50
```

## 4. Dynamic Array Logic: Maximum Element 
**Goal:** Find the largest element inside a dynamically allocated block of memory.

**Input:** Size `5`, Elements: `12`, `45`, `7`, `89`, `23`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter size of array: ");
    scanf("%d", &n);

    int *arr = (int*) malloc(n * sizeof(int));
    if(arr == NULL) return 0;

    printf("Enter array elements: \n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    int max = arr[0];
    printf("Array elements are: \n");
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
        if(arr[i] > max) {
            max = arr[i];
        }
    }
    printf("\nMaximum element in the array is: %d\n", max);
    free(arr);
    arr = NULL;
    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter size of array: 5
Enter array elements: 
12 45 7 89 23
Array elements are: 
12	45	7	89	23	
Maximum element in the array is: 89
```

## 5. Dynamic Array Logic: Count Evens 
**Goal:** Iterate through the dynamic array and count how many numbers are perfectly divisible by 2.

**Input:** Size `6`, Elements: `1`, `2`, `3`, `4`, `5`, `6`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    int evenCount = 0;
    printf("Enter size of array: ");
    scanf("%d", &n);

    int *arr = (int*) malloc(n * sizeof(int));
    if(arr == NULL) return 1;

    printf("Enter array elements: \n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("Array elements are: \n");
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
        if(arr[i] % 2 == 0) {
            evenCount++;
        }
    }
    printf("\nNumber of even numbers in the array is: %d\n", evenCount);

    free(arr);
    arr = NULL;
    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter size of array: 6
Enter array elements: 
1 2 3 4 5 6
Array elements are: 
1	2	3	4	5	6	
Number of even numbers in the array is: 3
```

## Final Outcomes: What I Learned Today
1. The Geography of RAM: I proved visually that the pointer variable (e.g., `ptr` or `arr`) lives on the Stack, but the actual data it points to lives far away on the Heap.

2. `malloc` does NOT clean up: By printing the array before storing values in Program 2, I saw random negative and huge numbers. This proved that `malloc` simply grabs raw memory and doesn't bother clearing out the old data (garbage values) left by previous programs!

3. Array Syntax on Pointers: Even though `arr` is technically a pointer (`int *arr`), I can still use the standard array bracket syntax (`arr[i]`) to easily interact with the sequential memory blocks.

4. The Dangling Pointer Defense: Whenever I call `free(arr);`, it gives the memory back to the OS, but `arr` still holds the address of that old memory! By adding `arr = NULL;` immediately after, I safely disarm the pointer so I don't accidentally try to use deleted memory later.