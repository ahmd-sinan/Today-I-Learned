# Audio Processing: The Anatomy of a WAV File

**Date:** 2026-02-07

Today I learned that a `.wav` file isn't magic—it is just a massive list of numbers. By understanding how these numbers are structured, I can write programs to manipulate sound (like changing volume) mathematically!

## 1. The Structure of a WAV File 
A standard WAV file consists of two distinct parts: the **Header** and the **Data**.

### A. The Header (Metadata) 
* **Size:** The first **44 bytes** of the file.
* **Purpose:** It acts like an "ID Card" for the file. It stores :
    * It is a WAVE file
    * It's file size
    * It's sample rate (e.g., 44,100 Hz)
    * It's size of each samples (e.g., 16-bit)
    - If open a wav file, header tells these all to Audio Player.
* **Important:** If we modify audio, we must **copy this header unchanged** to the new file so the player recognizes it.

### B. The Samples (The Sound) 
* **Location:** Everything *after* the first 44 bytes.
* **Format:** A sequence of **16-bit signed integers** (2 bytes each).
* **Representation:** Each number represents the amplitude (height) of the sound wave at a specific millisecond.

| Byte 0 - 43 | Byte 44 - 45 | Byte 46 - 47 | Byte 48 - 49 | ... |
| :---: | :---: | :---: | :---: | :---: |
| **HEADER** | Sample 1 | Sample 2 | Sample 3 | ... |
| (Metadata) | `int16_t` | `int16_t` | `int16_t` | ... |

## 2. The Logic: Changing Volume 
Since audio is just numbers, changing volume is just **multiplication**.

To scale the volume, we read each 16-bit sample, multiply it by a **factor**, and write it to the new file.

$$NewSample = OriginalSample \times Factor$$

### Examples:
* **Factor = 1.0:** No change (Original volume).
* **Factor = 2.0:** Values become larger $\rightarrow$ Wave amplitude increases $\rightarrow$ **Double Volume**. 🔊
* **Factor = 0.5:** Values become smaller $\rightarrow$ Wave amplitude decreases $\rightarrow$ **Half Volume**. 🔉

## 3. Implementation Logic in C 
1.  Open the input file (`input.wav`) and output file (`output.wav`).
2.  **Read & Copy Header:** Read the first 44 bytes from input and write them directly to output.
3.  **Loop Through Samples:**
    * Read one 16-bit integer (2 bytes) at a time.
    * Multiply that integer by the `factor`.
    * Write the result to the output file.
4.  Close files.

---
**Key Takeaway:** Digital audio is just a time-series graph of numbers. By doing simple math on those numbers, we can act like a DJ and alter the sound! 
