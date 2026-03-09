# Practice: Text Files with `fprintf()` 

**Date:** 2026-03-09

Today I put `fprintf()` into practice by writing three different programs. Instead of just printing data to the terminal, I wrote programs that dynamically generate and format actual `.txt` files based on user input.

### 1. The Friend Logger 🧑‍🤝‍🧑
This program takes a simple string input from the user and writes a formatted sentence into `friends.txt`.

```c
#include <stdio.h>

int main() {
    FILE *fp;
    char name[20];
    printf("Enter your best friend's name: ");
    scanf("%s", name);

    fp = fopen("friends.txt", "w");
    if (fp == NULL) {
        return 1;
    }

    // Using %s to inject the string directly into the file
    fprintf(fp, "My Best Friend is %s\n", name);

    printf("File written successfully!\n");
    fclose(fp);

    return 0;
}

```

### 2. The Array Exporter 
This program takes an unknown amount of integers, stores them in an array, and then iterates through the array to log each element line-by-line into `data.txt`

```C
#include <stdio.h>

int main() {
    FILE *fp;
    int n;
    printf("Enter number of data: ");
    scanf("%d", &n);

    // Variable Length Array (VLA)
    int num[n];
    printf("Enter %d integers: \n", n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &num[i]);
    }

    fp = fopen("data.txt", "w");
    if (fp == NULL) {
        return 1;
    }

    // Looping through the array and writing to the file
    for (int i = 0; i < n; i++) {
        fprintf(fp, "Element %d: %d\n", i+1, num[i]);
    }

    fclose(fp);
    printf("File written successfully!\n");

    return 0;
}
```
### 3. Multiplication Table Generator 
This program asks for a base number and generates a full 1-to-10 multiplication table, saving the calculated math directly into `table.txt`

```C
#include <stdio.h>

int main() {
    FILE *fp;
    int num;
    printf("Enter a number: ");
    scanf("%d", &num);

    fp = fopen("table.txt", "w");
    if (fp == NULL) {
        return 1;
    }

    // Writing a header line
    fprintf(fp, "Multiplication Table of %d\n", num);
    
    // Calculating and writing line-by-line
    for (int i = 1; i <= 10; i++) {
        fprintf(fp, "%d x %d = %d\n", num, i, num * i);
    }
    fclose(fp);

    printf("File written successfully!\n");
    return 0;
}
```
**Key Takeaway:** `fprintf()` is incredibly versatile. Because it uses the exact same format specifiers (`%d`, `%s`) as standard `printf()`, I can format file outputs exactly how I would format terminal outputs!