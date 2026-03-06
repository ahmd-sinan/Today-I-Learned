# Image Representation & Pixel Manipulation 

**Date:** 2026-03-06

Today I learned how computers actually "see" and store images. It is not magic; it is just a massive grid of numbers (a bitmap). By changing those numbers, we can create our own image filters!

## 1. What is a Bitmap? 
At its core, an image is a grid of pixels (dots). A **bitmap** is literally a "map of bits."
* **Black & White (1-bit):** The simplest image uses 1 bit per pixel (`0` for black, `1` for white).
* **True Color (24-bit):** Formats like BMP, JPEG, or PNG use 24 bits per pixel to represent millions of colors.

## 2. The RGB Color Model 🔴🟢🔵
In a 24-bit image, those 24 bits are divided equally into 3 channels (8 bits each):
* **Red:** 8 bits
* **Green:** 8 bits
* **Blue:** 8 bits

Since 8 bits can hold values from `0` to `255` (or `0x00` to `0xff` in Hexadecimal), we mix these intensities to create any color.
* **Pure Red:** `0xff, 0x00, 0x00` (Max Red, no Green, no Blue).
* **Pure Black:** `0x00, 0x00, 0x00` (All lights off).
* **Pure White:** `0xff, 0xff, 0xff` (All lights at max brightness).

## 3. Images as 2D Arrays 
In C, a 24-bit BMP image can be represented as a 2D array of `struct`s. Each `struct` represents one pixel and holds three integers: `R`, `G`, and `B`.

```c
// Example of accessing a pixel in a 2D array
// image[row][column].color
image[0][0].Red = 255;   // Makes the top-left pixel fully red
```

## Building Image Filters (The Math) 
By looping through this 2D array, we can apply mathematical formulas to every single pixel to create filters.

### A. Grayscale (Black & White) 
To turn a colored image into varying shades of gray, the R, G, and B values must all be equal.
- **The Logic:** Calculate the average of the original R, G, and B values, and set all three to that average.

$Formula: Average = round((R + G + B) / 3.0)$

### B. Sepia (Old-Timey Photo) 
Sepia gives an image a reddish-brown, vintage look. It uses a specific weighted algorithm to calculate new colors based on the old ones.
- **The Catch:** The new calculated values might exceed 255. We must always cap the result at 255 so the integer doesn't overflow!

### C. Reflection (Mirror Image) 
To flip an image horizontally, we don't even need to change the colors. We just change their coordinates.
- **The Logic:** Loop through every row. Swap the pixel on the far left with the pixel on the far right, moving inward until you reach the middle.
- **Tool:** Requires a temporary placeholder variable to swap the struct data safely.

### D. Blur (Softening) 
Blurring an image means reducing the sharp contrasts between neighboring pixels.
- **The Logic (Box Blur):** To blur a single pixel, look at a 3x3 grid around it (itself + 8 neighbors). Calculate the average R, G, and B of all 9 pixels, and make that the new color for the center pixel.
- **Challenge:** Pixels on the edges and corners don't have 8 neighbors, so the algorithm must carefully check for the image boundaries!
---
**Key Takeaway:** An image is just a matrix of RGB values. "Filtering" is just applying a mathematical formula to every element in that matrix using nested for loops.