# Practice: Data Pipelines with `fscanf` & `fprintf` 

**Date:** 2026-03-11

Today I completed a massive 5-program practice session focusing on reading, filtering, and copying data between files. 

### 1. Basic Array Population 
**Goal:** Read integers from `data.txt` and print them to the screen.

** Input (`data.txt`):**
```text
10 20 35 55 80
```

**Code:**
```c
#include <stdio.h>

int main() {
    int n[20];
    int i = 0;

    FILE *fp = fopen("data.txt", "r");
    if (fp == NULL) return 1;

    // Read until fscanf fails to match 1 integer, or we hit array limit
    while (fscanf(fp, "%d", &n[i]) == 1 && i < 20) {
        printf("number %d: %d\n", i+1, n[i]);
        i++;
    }
    
    fclose(fp);
    printf("File read successfully!\n");
    return 0;
}
```

### 2. Data Filtering (Evens Only) 
Goal: Read from `input.txt`, filter out odd numbers, and write ONLY even numbers to `output.txt`

**Input (`input.txt`):**
```text
2 5 10 22 53 17 41 34 90
```

**Code:**
```C
#include <stdio.h>

int main() {
    int n[20];
    FILE *in = fopen("input.txt", "r");
    if (in == NULL) return 1;

    int i = 0;
    printf("Even numbers are: \n");
    while (fscanf(in, "%d", &n[i]) == 1 && i < 20) {
        if (n[i] % 2 == 0) {
            printf("%d\t", n[i]);
        }
        i++;
    }

    FILE *out = fopen("output.txt", "w");
    if (out == NULL) {
        fclose(in);
        return 1;
    }

    // Write only the even numbers to the output file
    for (int j = 0; j < i; j++) {
        if (n[j] % 2 == 0) {
            fprintf(out, "%d\t", n[j]); 
        }
    } 

    fclose(in);
    fclose(out);
    printf("\nFile read and written successfully!\n");
    return 0;
}
```

**Output (`output.txt`):**
```text
2	10	22	34	90
```

### 3. On-the-Fly Math (Sum & Average) 
Goal: Read a file of numbers, count them, and calculate the running sum and average.

**Input (`data.txt`):**
```text
10 12 33 40 50 60 85 10 18 10
```

**Code:**
```C
#include <stdio.h>

int main() {
    int n[20];
    int count = 0;
    FILE *fp = fopen("data.txt", "r");
    if (fp == NULL) return 1;

    int sum = 0;
    float avg = 0;
    printf("Elements of data.txt : \n");
    
    for(int i = 0; i < 20; i++) {
        while (fscanf(fp, "%d", &n[i]) != EOF) {
            printf("%d\t", n[i]);
            count++;
            sum += n[i];
            avg = ((float) sum / count);
        }
    }
    
    printf("\n\n");
    printf("data count: %d\n", count);
    printf("Sum of data: %d\n", sum);
    printf("Average of data: %.2f\n", avg);

    fclose(fp);
    printf("\nFile read Successfully!\n");
    return 0;
}
```

### 4. Multi-Type Processing (Student Grades) 
Goal: Read strings (name) and integers (marks), evaluate them, and write only passing students (>40) to a new file.

**Input (`students.txt`):**
```text
Alice 85
Bob 32
Charlie 45
David 18
Eve 92
```

**Code:**
```C
#include <stdio.h>

int main() {
    char name[10][20]; // Array of 10 strings
    int mark[10];

    FILE *file = fopen("students.txt", "r");
    if (file == NULL) return 1;

    FILE *out = fopen("Pass.txt", "w");
    if (out == NULL) return 1;

    int i = 0;
    // Reading two different data types at once!
    while (fscanf(file, "%s %d", name[i], &mark[i]) != EOF && i < 10) {
        printf("%s %d\n", name[i], mark[i]);

        if (mark[i] > 40) {
            fprintf(out, "%s %d\n", name[i], mark[i]);
        }
        i++;
    }

    fclose(file);
    fclose(out);
    printf("\nFiles processed successfully!\n");
    return 0;
}
```

**Output (`Pass.txt`):**
```text
Alice 85
Charlie 45
Eve 92
```

### 5. File Copying (The Whitespace Trap) 
Goal: Copy text from `source.txt` to `copy.txt` word-by-word.

**Input (`source.txt`):**
```text
C programming is powerful
File handling is important
```

**Code:**
```C
#include <stdio.h>

int main() {
    FILE *source = fopen("source.txt", "r");
    if (source == NULL) return 1;

    FILE *copy = fopen("copy.txt", "w");
    if (copy == NULL) return 1;

    char words[100];
    while (fscanf(source, "%s", words) != EOF) {
        fprintf(copy, "%s ", words);
    }

    fclose(source);
    fclose(copy);
    printf("File copied successfully!\n");
    return 0;
}
```

**Output (`copy.txt`):**
```text
C programming is powerful File handling is important
```

## Final Outcomes: What I Learned Today
- The Ultimate Loop Condition: The safest way to read files is using the `while` loop combined with `fscanf`'s return value. Whether I use `!= EOF or == 1` (number of expected items), this prevents the program from crashing if the file is shorter than my array limit.

- Dual File Pointers: I can have multiple file pointers open at the exact same time (e.g., `FILE *in` and `FILE *out`). This allows me to act like a filter—sucking data out of one file, checking it with an `if` statement, and pushing it immediately into another.

- On-the-Fly Processing: As seen in the Even Numbers and Students programs, I don't necessarily need to store everything in a massive array before making decisions. I can evaluate variables the exact millisecond they are read from the file.

- The `%s` Whitespace Quirk: In the 5th program, I learned a crucial side-effect of `fscanf`. The `%s` format specifier completely ignores spaces and newlines (`\n`). This is why my copied file ended up as one long, single line of text (C programming is powerful File handling is important) instead of keeping the original line breaks!