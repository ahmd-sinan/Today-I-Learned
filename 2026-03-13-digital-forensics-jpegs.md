# Digital Forensics: JPEG Signatures & Slack Space 🔍

**Date:** 2026-03-13

Today I learned how to identify file types from raw binary data and how hard drives actually allocate physical space. This is the foundation of data recovery!

## 1. Magic Numbers (File Signatures) 
How does a computer know a file is a photo and not a text document? It doesn't look at the `.jpg` extension (that can easily be faked!). Instead, it looks at the first few bytes of the file, known as its **Signature** or "Magic Numbers".

For a standard JPEG, the signature is a specific sequence of bytes:
* **Byte 1:** `0xff`
* **Byte 2:** `0xd8`
* **Byte 3:** `0xff`
* **Byte 4:** Either `0xe0`, `0xe1`, `0xe2`, `0xe3`, `0xe4`, `0xe5`, `0xe6`, `0xe7`, `0xe8`, `0xe9`, `0xea`, `0xeb`, `0xec`, `0xed`, `0xee`, or `0xef`. 

*(Notice a pattern in the 4th byte? In binary, its first four bits are always `1110`!)*

If I scan a raw memory card and find this exact sequence, I have almost certainly found the beginning of a photograph! 

## 2. FAT File Systems & Block Sizes 
Digital cameras usually initialize memory cards using a FAT (File Allocation Table) file system. 
* Hard drives and SD cards don't just write bytes randomly; they are divided into predefined chunks called **Blocks** (or clusters).
* A standard block size is **512 Bytes**.
* **The Rule:** The hardware *only* writes in full 512-byte units. Even if you have a file that is exactly 1 byte long, the computer still locks up an entire 512-byte block for it.

## 3. The Math Behind the Blocks 
If a camera saves a photo that is exactly 1 MB (1,048,576 Bytes), it will perfectly fill `2048` blocks:
* `1,048,576 ÷ 512 = 2048 blocks`

But what if the photo is just one byte smaller (`1,048,575 Bytes`)? 
* It *still* takes up `2048` blocks. 
* The first `1,048,575` bytes are the photo.
* The last `1` byte in that final block is empty.

## 4. Slack Space: The Forensic Goldmine 
That wasted space at the end of a block is called **Slack Space**. 
Because hardware writes in chunks, it doesn't always clean up the old data left behind in the slack space. Forensic investigators absolutely love slack space! When they analyze a criminal's hard drive, they often find remnants of old, deleted passwords, text messages, or partial files hiding in the slack space of completely innocent-looking files.

## 5. How Data Recovery Actually Works  
When you "delete" a photo on a camera, the OS doesn't erase the actual bytes. It just deletes the *index* that points to the photo, marking those 512-byte blocks as "Available."

To recover "deleted" photos, a program can:
1. Open the raw SD card data.
2. Read the card block-by-block (512 bytes at a time).
3. Check the first 4 bytes of every block.
4. If it matches `0xff 0xd8 0xff 0xe_`, start a new `.jpg` file and keep dumping 512-byte blocks into it until it hits another JPEG signature!