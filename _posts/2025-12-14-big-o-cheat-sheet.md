---
title: Big O cheat sheet
date: 2025-12-14
categories: [PROGRAMMING, INTERVIEW]
tags: [programming, big-o]     # TAG names should always be lowercase
---

# Big O Notation

## 📘 Big O Notation -- Summary & Concepts

### 💡 What is Big O?
* Big O describes how runtime (or space) scales with input size.
* It's used to evaluate the efficiency of algorithms.
* Think of it as a way to express "how slow or fast" an algorithm gets as the input grows.

### 🕊️ Pigeon vs Internet Analogy
**Scenario:** Data sent via Internet vs. Pigeon with USB stick (2009 South Africa).

**Observation:**
* Pigeon always takes about the same time (constant): `O(1)`
* Internet time grows with data size (linear): `O(n)`

**🔑 Lesson:** Big O ignores constants and focuses on growth rate with input size.

---

### 📌 Code Examples
* **Linear Search through array:** `O(n)`
* **Printing all pairs from array:** `O(n²)`
* **Mowing a square field:**
    * Area-based: `O(a)` where a = area
    * Side-length-based: `O(s²)` where s = side length

---

### 📏 4 Core Rules of Big O

#### 1. ➕ Additive Time
If two steps run one after another, add runtimes: → e.g., `O(a) + O(b)`
* **Example:** Walk through two different arrays.

#### 2. ❌ Drop Constants
Don't write `O(2n)` or `O(3n)` → just say `O(n)`.
* **Focus:** Growth rate, not exact steps.

#### 3. 🔤 Use Different Variables for Different Inputs
**Example:** Comparing elements in two arrays of size **a** and **b**.
* ❌ Incorrect: `O(n²)`
* ✅ Correct: `O(a * b)`

#### 4. 📉 Drop Non-Dominant Terms
If algorithm has `O(n + n²)` → simplify to `O(n²)`.
* The highest order term dominates.

---

