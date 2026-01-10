# How Computer Memory (RAM) Actually Works
**Date:** 01-Jan-2026

Today I learned how RAM (specifically DRAM) stores data at the hardware level. It's not just "magic," it's physics!

## Key Concepts I Learned:
1. **The Building Blocks:**
   - A single bit of data (1 or 0) is stored in a **Memory Cell**.
   - Each cell contains **one Capacitor** and **one Transistor**.

2. **The Capacitor (The Bucket):**
   - The capacitor holds the electrical charge.
   - Charged = `1`
   - Empty = `0`
   - *Problem:* Capacitors leak energy over time (like a bucket with a tiny hole), so the computer has to constantly "refresh" them. That is why it is called **Dynamic** RAM.

3. **The Transistor (The Door):**
   - The transistor acts like a switch or a door.
   - It allows the computer to check the capacitor (read) or fill it up (write).
   - If the door is closed, the data stays safe inside the capacitor.

## Summary
Modern RAM sticks contain **billions** of these tiny capacitor-transistor pairs!