# Time Complexity of Sorting Algorithms ⏳

**Date:** 2026-01-20

Today I learned that we don't measure algorithm speed in seconds (because that depends on hardware). Instead, we measure it in **steps** relative to the input size ($n$).

## The Metrics 📏
* **Big $O$ (Upper Bound):** The "Worst Case" scenario. The maximum steps the algorithm might take.
* **Big $\Omega$ (Omega) (Lower Bound):** The "Best Case" scenario. The minimum steps if we get lucky (e.g., the list is already sorted).

## Comparison Table 📊

| Algorithm | Worst Case ($O$) | Best Case ($\Omega$) |
| :--- | :--- | :--- |
| **Selection Sort** | $O(n^2)$ | $\Omega(n^2)$ |
| **Bubble Sort** | $O(n^2)$ | $\Omega(n)$ |
| **Merge Sort** | $O(n \log n)$ | $\Omega(n \log n)$ |



## Breakdown 🧠

### 1. Selection Sort 🐢
* **Performance:** $O(n^2)$ and $\Omega(n^2)$.
* **Why?** It is "stubborn." Even if the list is already sorted, it still blindly scans through every element to find the smallest number. It does the same work no matter what.

### 2. Bubble Sort 🫧
* **Performance:** $O(n^2)$ but $\Omega(n)$.
* **Why?** In the worst case (reverse order), it is very slow.
* **The "Lucky" Case:** If the list is *already sorted*, Bubble Sort realizes it made no swaps in the first pass and stops immediately. That is why it is $\Omega(n)$ (Linear time).

### 3. Merge Sort 🚀
* **Performance:** $O(n \log n)$ and $\Omega(n \log n)$.
* **Why?** It splits the list in half repeatedly (Divide and Conquer). It is consistently fast and doesn't care if the list is sorted or messy—it always takes the same amount of steps.
* **Note:** $n \log n$ is significantly faster than $n^2$ for large datasets!

---
**Key Takeaway:** **Merge Sort** is generally superior for large lists because $n \log n$ grows much slower than $n^2$, but **Bubble Sort** has a unique advantage if the data is already mostly sorted.