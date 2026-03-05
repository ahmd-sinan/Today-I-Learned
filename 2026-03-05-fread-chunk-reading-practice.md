# Practice: Chunk Reading & File Pointers with `fread` 

**Date:** 2026-03-05

Today I practiced writing and reading custom `structs` to a binary file. I learned a crucial concept about how `fread` handles memory buffers and the internal file pointer.

## My Practice Code 
I wrote a program to take 3 students, save them to a `.bin` file, and read them back to calculate the average mark.

```c
#include <stdio.h>

struct Student {
    char name[20];
    int mark;
};

int main(void) {
    struct Student s[3];
    printf("Enter name and marks of 3 students: \n");
    for (int i = 0; i < 3; i++) {
        scanf("%s %d", s[i].name, &s[i].mark);
    }

    // --- WRITE PHASE ---
    FILE *fp = fopen("Students.bin", "wb");
    if (fp == NULL) return 1;
    
    fwrite(s, sizeof(struct Student), 3, fp);
    fclose(fp);

    // --- READ PHASE ---
    fp = fopen("Students.bin", "rb");
    if (fp == NULL) return 1;

    struct Student buffer[3];
    int count = fread(buffer, sizeof(struct Student), 3, fp);
    
    int sum = 0;
    for (int i = 0; i < count; i++) {
        printf("%d. Name: %s\tMark: %d\n", i+1, buffer[i].name, buffer[i].mark);
        sum += buffer[i].mark;
    }
    
    printf("Average Mark: %.2f\n", (float)sum / count);
    printf("Successfully written and read the file!\n");

    fclose(fp);
    return 0;
}
```
## The Core Concept: The File Pointer 
What if the file contains 10 students, but my buffer only holds 3?
```c
struct Student buffer[3];
```
```c
fread(buffer, sizeof(struct Student), 3, fp);
```
- Result: `fread` reads exactly what I asked for. It loads the first 3 students into my buffer. The remaining 7 stay on the hard drive.
- `fread` reads sequentially. After reading 3 students, the File Pointer automatically moves forward.

```Plaintext
[1][2][3] -> already read into buffer
[4][5][6][7][8][9][10] -> still inside file
 ^
 File pointer is now waiting here
```
If I call `fread` again, it will naturally pick up students 4, 5, and 6!

## The Professional Pattern: Block-by-Block 
Because the file pointer moves automatically, we can use a while loop to read massive files in small, safe chunks without running out of RAM.

#### The "Leftover" Problem:
If a file has 10 students, reading in chunks of 3 looks like this:
- Read 1: Gets 3     [1]2][3]
- Read 2: Gets 3     [4][5][6]
- Read 3: Gets 3     [7][8][9]
- Read 4: Gets 1     [10]     (Only 1 student left!)

#### The Solution:
This is why capturing the return value of fread is a very important detail. We must use the count returned by fread as our loop limit, not the buffer size!
```C
struct Student buffer[3];
int count;

// Keep reading until fread returns 0
while ((count = fread(buffer, sizeof(struct Student), 3, fp)) > 0)
{
    // Loop only 'count' times (safeguards against the final partial read)
    for(int i = 0; i < count; i++)
    {
        printf("Name: %s, Mark: %d\n", buffer[i].name, buffer[i].mark);
    }
}
```

## Real-World Applications 
This block-by-block reading technique isn't just for practice. It is exactly how professional systems process data larger than the available RAM:
* Databases: Loading records.
* Operating Systems: Managing virtual memory.
* Large File Processing: Video rendering or log analysis.
* Game Engines: Streaming level assets as the player walks around.

* **Key Takeaway:** Never assume your buffer is full. Always use the integer returned by `fread()` to know exactly how much valid data you just pulled from the file!