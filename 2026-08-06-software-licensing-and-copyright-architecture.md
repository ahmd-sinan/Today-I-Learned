# Software Licensing & Copyright Architecture 

**Date:** 2026-08-06

**Category:** Software Engineering / Open Source
**Tags:** #OpenSource #Licensing #Copyright #MIT #GPL #GitHub

Today I learned the legal framework of software development. Writing the code is only half the battle; defining how the world is legally allowed to interact with, modify, and distribute that code is critical in enterprise software engineering.

## 1. The Default State: Standard Copyright 
*   **The Concept:** The moment you write an original line of code and save it to your hard drive, it is automatically protected by copyright. 
*   **The Industry Rule:** If you upload a repository to GitHub *without* attaching a specific license file, standard copyright laws apply. This means the code is "All Rights Reserved." Other developers can view the code, but they are legally forbidden from copying, modifying, or using it in their own projects.

## 2. Open Source Licenses 
To allow others to use your code, you must attach an Open Source License. The industry categorizes these into two main types: **Permissive** (do what you want) and **Copyleft** (viral sharing).

### A. The MIT License (Highly Permissive)
*   **What it means:** "You can do absolutely whatever you want with this code (use it, sell it, modify it), just don't sue me if it breaks, and you must include my original name/copyright notice in your app."
*   **Industry Context:** This is the most popular license in the world. Tech giants love it because they can grab your MIT-licensed library, put it inside their proprietary paid software, and make billions without having to share their own source code. (Frameworks like React and Vue are MIT licensed).

### B. The Apache License 2.0 (Permissive + Patent Protection)
*   **What it means:** Very similar to MIT (do what you want, no liability), but it includes strict legal clauses regarding software patents and trademark usage.
*   **Industry Context:** Used heavily by massive corporate open-source projects. For example, Google released Android under the Apache 2.0 license. It legally protects the creators if a rogue contributor tries to sneak patented code into the repository.

### C. The GNU GPL (General Public License) (Copyleft / Viral)
*   **What it means:** "You can use and modify this code freely, **BUT** if you distribute a program that uses my code, you are legally forced to make your entire program Open Source under the exact same GPL license."
*   **Industry Context:** This is a "viral" license. Enterprise companies are terrified of accidentally using GPL libraries in their proprietary codebases, because it could legally force them to publish their top-secret source code to the public! (Linux and Git are licensed under GPL).

## 3. Commercial vs. Open Source Software (FOSS) 
*   **Proprietary / Closed Source:** Software where the source code is hidden, encrypted, and compiled (e.g., Microsoft Windows, Adobe Photoshop, most AAA video games). You pay for a *license to use it*, not to own it.
*   **FOSS (Free and Open Source Software):** Software where the raw source code is freely available for anyone to inspect, modify, and improve (e.g., Linux Desktop environments, Blender, VS Code). 

## 4. How to Apply a License to Your Code 
Adding a license to your project is incredibly simple.
1.  Create a standard text file in the absolute root directory of your project (right next to your `README.md`).
2.  Name the file exactly `LICENSE` (all caps, no file extension).
3.  Paste the raw text of your chosen license (like the MIT License text) inside, change the copyright year, and type your name.
4.  Commit and push! GitHub will automatically read this file and display a badge on your repository showing what license you chose.