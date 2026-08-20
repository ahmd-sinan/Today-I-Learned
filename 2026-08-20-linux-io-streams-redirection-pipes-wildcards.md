# Linux CLI: File Streams, I/O Redirection, Pipes & Wildcards 

**Date:** 2026-08-20

Today I learned how Linux manages data flow under the hood using file descriptors. I explored how to manipulate standard input/output streams, the architectural power of piping commands together in RAM, and how to efficiently search files using system databases and shell wildcards.

## Standard File Streams (The Big Three) 
When any command is executed in Linux, the operating system automatically opens three default data streams. Because of the "Everything is a File" philosophy, Linux tracks these streams using integer IDs called **File Descriptors**.

*   **Standard Input (`stdin` - File Descriptor `0`):** The stream where a program reads its input data. By default, this is hooked up to your physical keyboard, but it can easily be fed data from a file instead.
*   **Standard Output (`stdout` - File Descriptor `1`):** The stream where a program writes its normal, successful results. By default, this is printed to your terminal screen.
*   **Standard Error (`stderr` - File Descriptor `2`):** A completely separate output stream dedicated exclusively to warning and error messages. 
    *   *Industry Context:* Keeping `stderr` separate from `stdout` is a brilliant architectural design. It allows a DevOps engineer to save all the successful data to a database, while simultaneously sending all the error messages to a separate log file!

## I/O Redirection 
Redirection allows you to detach these file streams from your keyboard/screen and re-route them into files.

*   **Redirecting Input (`<`):** Forces a command to read from a file instead of waiting for you to type.
*   **Redirecting Output (`>`):** Captures the `stdout` of a command and writes it into a file (overwriting the file if it exists). Note: `1>` does the exact same thing since 1 represents `stdout`.
*   **Redirecting Errors (`2>`):** Because standard output redirection (`>`) only captures Descriptor 1, errors will still bleed onto your screen. You must explicitly redirect Descriptor 2 to catch them.

### Code Implementation Example (Redirection):
```bash
# 1. Basic Input and Output
$ sort < unsorted_list.txt > sorted_list.txt

# 2. Redirecting only the errors to a log file
$ find / -name "config" 2> error_logs.txt

# 3. Industry Standard: Redirecting EVERYTHING (Output + Errors) to the same file
# The '2>&1' syntax tells the shell: "Send stream 2 (stderr) to exactly the same place stream 1 (stdout) is going."
$ run_backup_script.sh > total_log.txt 2>&1

# Bash Shorthand for the above:
$ run_backup_script.sh >& total_log.txt
```

## Pipes (`|`) 
The core "Unix Philosophy" is to build small, simple programs that do one thing perfectly, rather than building massive, complicated applications. You connect these small programs using Pipes.

* **How it works:** The vertical bar `|` captures the `stdout` of the command on the left and literally "pipes" it in as the `stdin` for the command on the right.
* **The Architectural Benefit (Memory vs. Disk):**
    * **Efficiency:** No temporary files are ever written to your physical hard drive. The data flows directly through the system's RAM, bypassing the slow disk entirely.
    * **Concurrency:** The commands process simultaneously. The second command doesn't wait for the first to finish; it starts chewing on the data stream the exact millisecond the first command starts outputting it!

## Searching for Files (`locate`) 
When you need to find a file quickly across the entire system, searching the live hard drive can take minutes. Linux solves this with indexing.

* **`updatedb`:** A background utility (usually run automatically every night by a cron job) that scans your hard drive and builds a massive, highly optimized database of every file path. A root user can run this command manually to force a refresh.
* **`locate [string]`:** Searches the pre-built database instead of the live hard drive. It returns results almost instantly, but because it relies on the database, it won't find files you created 5 minutes ago (unless you run `updatedb` first).
    * Piping Example: `$ locate zip | grep bin` (Finds everything with "zip", then pipes that massive list into `grep` to filter out only the lines that also contain "bin").

## Wildcards / Globbing (Matching Filenames) 
Wildcards are special characters used to target multiple files at once.
Architectural Note: It is actually the Shell (Bash) that interprets these wildcards, not the command itself. Bash expands the wildcards into a list of matching files before handing that list over to the command!
* `?` (Question Mark): Matches exactly any single character.
    * `ls ba?.out` matches `bat.out` or `bar.out`, but not `ball.out`
* * (Asterisk): Matches any string of characters (including zero characters).
    * `ls *.out` matches literally any file ending in .out.
* `[set]` (Brackets): Matches any single character specified inside the brackets. You can use ranges.
    * `ls g[a-n]???` matches a 5-letter file starting with 'g', where the second letter is alphabetically between 'a' and 'n' (e.g., `gmake`).
* [!set] (Not in Set): Matches any single character except the ones inside the brackets.