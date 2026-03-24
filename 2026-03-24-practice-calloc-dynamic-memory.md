# Practice: Contiguous Memory Allocation (`calloc`) 

**Date:** 2026-03-24

Today I practiced using `calloc()`. It is the cleaner sibling of `malloc()`. While `malloc` leaves old "garbage" values in memory, `calloc` automatically scrubs the memory clean and sets every bit to zero!

## 1. Single Pointer: The Zero Initialization 
**Goal:** Allocate memory for one integer using `calloc` and prove that it starts with a value of `0` before we assign anything to it.

**Input:** None

**Code:**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Allocate memory for one integer and initialize to zero
    int *ptr = (int*) calloc(1, sizeof(int));      

    if(ptr == NULL) {
        return 1;     // Exit with error code
    }
    
    // Should print 0 as calloc initializes memory to zero
    printf("Value before: %d\n", *ptr);        

    *ptr = 100;

    // Should print 100
    printf("Value after: %d\n", *ptr);          
    printf("Address of memory (Heap): %p\n", (void*)ptr);                   
    printf("Address of pointer itself (Stack): %p\n", (void*)&ptr);           

    free(ptr);
    ptr = NULL;

    return 0;
}
```

**🖥️ Terminal Output:**

```Plaintext
Value before: 0
Value after: 100
Address of memory (Heap): 0x562a1b3f82a0
Address of pointer itself (Stack): 0x7ffd9c8a2b48
```

## 2. Dynamic Array: Safe Sizing & Clean Slate 
**Goal:** Allocate a dynamic array, handle invalid size inputs (`n <= 0`), and print the array before user input to see the zeroes.

**Input:** Size `3`, Elements: `10`, `20`, `30`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter array size: ");
    scanf("%d", &n);

    if(n <= 0) {                // invalid size check
        printf("Invalid array size!\n");
        return 1;               // exit with error code
    }

    int *arr;
    // allocate memory for n integers and initialize to zero  
    // calloc(number of elements, size of each element)
    arr = (int*) calloc(n, sizeof(int));          

    if(arr == NULL) {
        return 1;
    }

    printf("Values stored in the array: \n");    // print initial values (should be zero)
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
    }

    printf("\n\nEnter array elements: \n");      // take user input to fill the array
    for(int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("\nArray values after input: \n");      // print values after user input
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
    }

    printf("\n\nAddress of heap memory: %p\n", (void*)arr);
    printf("Address of pointer itself (Stack): %p\n", (void*)&arr);

    free(arr);
    arr = NULL;

    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter array size: 3
Values stored in the array: 
0	0	0	

Enter array elements: 
10 20 30

Array values after input: 
10	20	30	

Address of heap memory: 0x562a1b3f82c0
Address of pointer itself (Stack): 0x7ffd9c8a2b50
```


## 3. Dynamic Array Logic: Divisible by 3 
**Goal:** Create a clean dynamic array, fill it with user numbers, and find/count all numbers perfectly divisible by 3.

**Input:** Size `5`, Elements: `3`, `7`, `9`, `12`, `14`

**Code:**

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n; 
    int count = 0;
    printf("Enter array size: ");
    scanf("%d", &n);

    if (n <= 0) {
        printf("Invalid array size!\n");
        return 1;
    }

    int *arr = (int*) calloc(n, sizeof(int));

    if(arr == NULL) {
        return 1;
    }

    printf("Enter array elements: \n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("Array elements are: \n");
    for(int i = 0; i < n; i++) {
        printf("%d\t", arr[i]);
    }

    printf("\n\nArray elements divisible by 3: \n");
    for(int i = 0; i < n; i++) {
        if(arr[i] % 3 == 0) {
            printf("%d\t", arr[i]);
            count++;
        }
    }
    printf("\nNumber of elements divisible by 3: %d\n", count);

    free(arr);
    arr = NULL;

    return 0;
}
```
**🖥️ Terminal Output:**

```Plaintext
Enter array size: 5
Enter array elements: 
3 7 9 12 14
Array elements are: 
3	7	9	12	14	

Array elements divisible by 3: 
3	9	12	
Number of elements divisible by 3: 3
```

## Final Outcomes: What I Learned Today
**`calloc` vs `malloc` (The Clean Slate):** I proved visually that while `malloc` leaves terrifying garbage values (like `-8493012`) in uninitialized arrays, calloc safely sets every single bit to `0`. This makes `calloc` slightly slower, but much safer if I forget to initialize a variable.

**Argument Syntax:** I learned that `calloc` takes TWO arguments: `(number_of_items, size_of_each_item)`. This is different from `malloc`, which just takes one big math equation `(number_of_items * size_of_each_item)`

**Robust Input Validation:** Adding `if (n <= 0)` is a game-changer. If a user types `-5` or `0` for an array size, `calloc` might behave unpredictably or crash. Catching that bad input before allocating memory is top-tier software design!