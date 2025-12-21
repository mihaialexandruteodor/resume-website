---
title: LeetCode 8 Patterns
date: 2025-12-14
categories: [PROGRAMMING, INTERVIEW]
tags: [programming, leetcode]     # TAG names should always be lowercase
---

**Source:** 
{% include embed/youtube.html id='RYT08CaYq6A' %}

---

##  Two Pointers Pattern

###  Why Two Pointers Matter
* Optimizes problems involving linear data structures (arrays, strings, linked lists).
* Reduces time complexity from brute-force `O(n²)` to efficient `O(n)`.
* Useful in situations where you'd otherwise compare every combination of elements.

###  Types of Two Pointer Techniques

#### 1. Same Direction
* Pointers move left to right together.
* Ideal for scanning or processing data in a single pass.
* **Example: Fast & Slow Pointers**
    * Detecting cycles in linked lists.
    * Finding the middle of a list.
    * Slow pointer: moves 1 step.
    * Fast pointer: moves 2 steps.

#### 2. Opposite Directions
* One pointer starts at the beginning, the other at the end.
* Common in sorted arrays or for pair-finding tasks.
* **Example: Two Sum Problem**
    * Find two numbers that add to a target.
    * Adjust pointers inward based on current sum.
    * Efficiently eliminates unnecessary comparisons.

---

##  Sliding Window Pattern

###  What It Is
* A specialized form of the two pointers technique.
* Focuses on maintaining a dynamic range (window) of elements.
* Used to analyze contiguous subarrays or substrings.
* Adjusts window size dynamically as the data is processed.

###  How It Works
1.  **Two pointers define the window:** One marks the start, one marks the end.
2.  **Expand:** As the end pointer moves forward, the window expands to include more elements.
3.  **Shrink:** As conditions are violated or goals are met, the start pointer moves forward to shrink the window.

###  Key Use Cases
* Finding maximum/minimum of something in a subarray.
* Tracking running sums, averages, or character counts.
* Searching for longest/shortest substrings meeting certain criteria.

###  Example Problem: Longest substring without repeating characters
1.  Expand the window by moving the end pointer.
2.  If a duplicate character appears, move the start pointer forward until the duplicate is removed.
3.  Use a hashmap to track characters inside the window.

---

##  Binary Search

###  What It Is
* A search algorithm used to find a target in a sorted array.
* Variant of the two pointers technique using a left and right boundary.
* **Time complexity:** `O(log n)` — far more efficient than linear search `O(n)`.

###  How It Works
1.  Initialize two pointers: `left` at the start, `right` at the end.
2.  Find the middle: `mid = (left + right) // 2`.
3.  Compare `array[mid]` with target:
    * If equal → return success.
    * If `mid < target` → search the right half.
    * If `mid > target` → search the left half.
4.  Repeat until pointers converge or element is found.

###  More Than Just Numbers
Binary Search also works for:
* **Monotonic functions:** Consistently increasing or decreasing sequences.
* **True/False patterns:** When a condition changes from false to true in a list (e.g., finding the first `True` in a list of booleans).

###  Creative Use Case: Minimum in Rotated Sorted Array
* Even though it seems unsorted, a monotonic condition exists.
* If `element < last element` → it's in the rotated section.
* If `element > last element` → it's in the original sorted section.

---

##  Breadth-First Search (BFS)

###  What It Is
* Algorithm for exploring graphs or trees level by level.
* Starts from a given node and visits all immediate neighbors before moving to the next level.

###  Key Characteristics
* **Data Structure:** Queue (FIFO - First In, First Out).
* **Tracks Visited Nodes:** To prevent infinite loops.
* **Ideal for:** Shortest path in unweighted graphs, Level-order traversal.

###  BFS Template Overview
1.  Initialize a queue and a visited set.
2.  Add starting node to queue.
3.  While queue is not empty:
    * Remove front node.
    * Add to result.
    * Add unvisited neighbors to queue.

###  Example: Level-Order Traversal
**Input Tree:**
```
      3
     / \
    9  20
      /  \
     15   7
```

**Output:** `[[3], [9, 20], [15, 7]]`

---

##  Depth-First Search (DFS)

###  What It Is
* Traversal algorithm that explores as far down a branch as possible before backtracking.
* **Contrast:** BFS goes level-by-level (Queue); DFS dives deep (Stack/Recursion).

###  Key Characteristics
* **Data Structure:** Stack (or call stack via recursion).
* **Ideal for:** Exploring all paths, cycle detection, backtracking.

###  DFS Template (Recursive)
```python
    def dfs(node, visited):
        if not node or node in visited:
            return
        visited.add(node)
        for neighbor in node.neighbors:
            dfs(neighbor, visited)
```

###  Example Problem: Number of Islands
* **Goal:** Count connected groups of '1's in a grid.
* **Approach:** Iterate through grid. If '1' is found, increment count and run DFS to mark all connected '1's as '0' (visited).
```python
    def numIslands(grid):
        if not grid:
            return 0
        rows, cols = len(grid), len(grid[0])
        count = 0

        def dfs(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] == '0':
                return
            grid[r][c] = '0' # Mark as visited
            dfs(r+1, c)
            dfs(r-1, c)
            dfs(r, c+1)
            dfs(r, c-1)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    dfs(r, c)
                    count += 1
        return count
```

---

##  Backtracking

### 🔹 What Is Backtracking?
* Extension of DFS.
* Builds the solution space dynamically.
* Used in combinatorial problems where the full decision tree isn't known upfront.

###  Key Concepts
* Explore options one decision at a time.
* If a dead end or invalid path is reached, **backtrack** (undo last decision) and try another path.

###  Template Pattern
```python
    def backtrack(path, options):
        if end_condition(path):
            result.append(path[:])
            return
        for option in options:
            if is_valid(option):
                path.append(option)
                backtrack(path, updated_options)
                path.pop()  # backtrack
```

###  Example: Letter Combinations of Phone Number
**Input:** "23" -> **Output:** ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```python
    def letterCombinations(digits):
        if not digits:
            return []

        digit_map = {
            "2": "abc", "3": "def", "4": "ghi", "5": "jkl",
            "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"
        }
        result = []

        def dfs(index, path):
            if index == len(digits):
                result.append("".join(path))
                return
            for char in digit_map[digits[index]]:
                path.append(char)
                dfs(index + 1, path)
                path.pop()  # backtrack

        dfs(0, [])
        return result
```
* **Time Complexity:** `O(4ⁿ)` (Exponential).

---

##  Priority Queue (Heap)

###  When to Use
* Top K elements.
* K smallest or K largest values.

###  Min vs Max Heap
* **Min Heap:** Smallest element at root.
* **Max Heap:** Largest element at root.

###  The Counterintuitive Rule
* Find **K Smallest** → Use **Max Heap**.
    * *Why?* Keep heap size at K. If a new number is smaller than the largest (root), pop the root and insert the new one.
* Find **K Largest** → Use **Min Heap**.

###  Efficiency
* Access root: `O(1)`
* Insert/Remove: `O(log n)`

---

##  Dynamic Programming (DP)

###  Core Idea
* Break problem into overlapping subproblems.
* Solve each subproblem once and **store the result** (Memoization).

###  Two Main Approaches

#### 1. Top-Down (Memoization)
* Start from main problem, recurse down.
* Store results in dictionary/array.
* Essentially: **Backtracking + Caching**.

#### 2. Bottom-Up (Tabulation)
* Solve smallest subproblems first.
* Build up to larger problems using loops.
* Often more efficient (no recursion overhead).

###  Summary
* DP optimizes exponential brute-force solutions to polynomial time.
* Fundamental for problems involving optimal decisions, counting, and partitioning.
