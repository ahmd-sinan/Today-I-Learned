# Data Representation: ASCII, Unicode, and RGB 0️⃣1️⃣

**Date:** 11-Jan-2026  
**Category:** CS Fundamentals  

Today I explored how computers simply use zeros and ones (binary) to represent everything we see on a screen—from text and emojis to complex images and videos.

## 1. ASCII (Text Representation)
Computers map numbers to characters using standards like ASCII (American Standard Code for Information Interchange).

- **Mapping:**
    - `A` to `Z` = `65` to `90`
    - `a` to `z` = `97` to `122`
- **The "Case" Trick:** The difference between a capital letter and its lowercase version is exactly **32**.
    - $97 ('a') - 65 ('A') = 32$
    - This is useful because 32 is a power of 2 ($2^5$), meaning a computer only needs to flip a single bit to change case!

### Binary Example: Letter 'A'
The character 'A' is 65 in decimal. In binary, it looks like this:

| 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0   | **1** | 0   | 0   | 0   | 0   | 0   | **1** |

$64 + 1 = 65$

### The Message "HI!"
Using the ASCII chart, "HI!" is stored as three bytes:
- **H:** 72
- **I:** 73
- **!:** 33

## 2. Unicode (The Universal Standard)
ASCII was limited (mostly English). The **Unicode** standard expanded the number of bits used, allowing us to represent:
- Characters from almost all world languages.
- Mathematical symbols.
- **Emojis!** 😂 (even emojis are just patterns of 0s and 1s).

## 3. RGB (Color Representation)
Just like text, colors are represented by numbers. We use **RGB** (Red, Green, Blue).
- Each color channel is usually 1 byte (8 bits), meaning values range from **0 to 255**.
- A single pixel is a combination of these three numbers.

### Interpreting Data Contextually
The same binary data can be interpreted differently depending on the software reading it.

If we take the numbers from "HI!" (`72`, `73`, `33`):
- **Text Reader:** Sees the letters "HI!"
- **Image Reader:** Sees a pixel with `R:72`, `G:73`, `B:33`.
    - Since Red and Green are mixed, it produces a **Yellow** hue.
    - Since the numbers are low (closer to 0 than 255), it appears as a **dark, muddy yellow**.

## 4. Media (Images, Video, Music) 🎬
- **Images:** Simply a grid (collection) of many pixels, each with its own RGB values.
- **Videos:** A sequence of many images (frames) stored together and played in rapid succession (like a flipbook).
- **Music:** Sound waves are sampled and quantized into numeric values (bytes) representing frequency and amplitude.

---
**Key Takeaway:** Whether it is a letter, a color, or a sound, to the computer, it is all just information represented by binary states (0 and 1).