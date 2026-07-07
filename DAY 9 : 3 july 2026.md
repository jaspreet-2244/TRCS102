#  NumPy Essentials for Data Science 

---

##  Topic 1: `reshape()` — Changing the Shape of an Array

###  Theory

`reshape()` allows you to **change the shape** of an existing array — without changing its data.

Think of it like rearranging students in a classroom:  
- 12 students sitting in **1 row of 12** → same 12 students now in **3 rows of 4**.
- The students don't change, only their **arrangement** changes.

###  Syntax

```python
array.reshape(new_rows, new_cols)
```

###  Important Rule

The **total number of elements must remain the same** before and after reshaping.

```
Original: shape (12,) → 12 elements
Reshaped: shape (3, 4) → 3 × 4 = 12 elements 
Reshaped: shape (4, 4) → 4 × 4 = 16 elements  (Error!)
```

###  Using `-1` as a wildcard

You can use `-1` for one dimension — NumPy will **calculate it automatically**.

```python
arr.reshape(3, -1)   # NumPy figures out the number of columns
arr.reshape(-1, 4)   # NumPy figures out the number of rows
```
```python
# ============================================================
# TOPIC 1 EXAMPLE: reshape()
# ============================================================

import numpy as np

# Create a 1D array of 12 elements
arr = np.arange(1, 13)   # [1, 2, 3, ..., 12]
print("Original 1D Array:", arr)
print("Shape:", arr.shape)

print()

# Reshape to 3 rows and 4 columns
arr_2d = arr.reshape(3, 4)
print("Reshaped to (3, 4):")
print(arr_2d)
print("New Shape:", arr_2d.shape)

print()

# Reshape to 4 rows and 3 columns
arr_4x3 = arr.reshape(4, 3)
print("Reshaped to (4, 3):")
print(arr_4x3)

print()

# Reshape to 2 rows and 6 columns
arr_2x6 = arr.reshape(2, 6)
print("Reshaped to (2, 6):")
print(arr_2x6)
```
```
Original 1D Array: [ 1  2  3  4  5  6  7  8  9 10 11 12]
Shape: (12,)

Reshaped to (3, 4):
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
New Shape: (3, 4)

Reshaped to (4, 3):
[[ 1  2  3]
 [ 4  5  6]
 [ 7  8  9]
 [10 11 12]]

Reshaped to (2, 6):
[[ 1  2  3  4  5  6]
 [ 7  8  9 10 11 12]]
```
```python
# ============================================================
# TOPIC 1 EXAMPLE: Using -1 in reshape
# ============================================================

import numpy as np

arr = np.arange(1, 13)
print("Original:", arr)
print()

# Let NumPy figure out the columns automatically
a = arr.reshape(3, -1)    # 3 rows, NumPy decides columns (= 4)
print("reshape(3, -1) → shape:", a.shape)
print(a)

print()

# Let NumPy figure out the rows automatically
b = arr.reshape(-1, 6)    # NumPy decides rows (= 2), 6 columns
print("reshape(-1, 6) → shape:", b.shape)
print(b)

print()

# Convert 2D back to 1D using reshape
back_to_1d = a.reshape(-1)   # -1 collapses everything into 1D
print("Reshaped back to 1D:", back_to_1d)
```
```
Original: [ 1  2  3  4  5  6  7  8  9 10 11 12]

reshape(3, -1) → shape: (3, 4)
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]

reshape(-1, 6) → shape: (2, 6)
[[ 1  2  3  4  5  6]
 [ 7  8  9 10 11 12]]

Reshaped back to 1D: [ 1  2  3  4  5  6  7  8  9 10 11 12]
```
---

##  Topic 2: `flatten()` — Convert to 1D (Returns a Copy)

###  Theory

`flatten()` converts a **multi-dimensional array into a 1D array**.  
It is like **unfolding a 2D table** into a single row.

```
[[1, 2, 3],      →    [1, 2, 3, 4, 5, 6]
 [4, 5, 6]]
```

###  Key Feature: `flatten()` returns a **COPY**

This means any changes to the flattened array will **NOT affect** the original array.

```python
flat = arr.flatten()
flat[0] = 999   # Changes flat array only — original arr is SAFE
```

###  Syntax

```python
array.flatten()          # Default: row by row (C-order)
array.flatten(order='F') # Column by column (Fortran-order)
```
```python
# ============================================================
# TOPIC 2 EXAMPLE: flatten()
# ============================================================

import numpy as np

# 2D array
arr2d = np.array([[1, 2, 3],
                  [4, 5, 6],
                  [7, 8, 9]])

print("Original 2D Array:")
print(arr2d)
print("Shape:", arr2d.shape)

print()

# Flatten to 1D
flat = arr2d.flatten()
print("Flattened Array:", flat)
print("Shape after flatten:", flat.shape)

print()

# Prove it's a COPY — modifying flat doesn't affect original
flat[0] = 999
print("Modified flat[0] to 999")
print("flat array  :", flat)          # 999 is here
print("Original arr:")                # Original is unchanged!
print(arr2d)
```
```
Original 2D Array:
[[1 2 3]
 [4 5 6]
 [7 8 9]]
Shape: (3, 3)

Flattened Array: [1 2 3 4 5 6 7 8 9]
Shape after flatten: (9,)

Modified flat[0] to 999
flat array  : [999   2   3   4   5   6   7   8   9]
Original arr:
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```
---

##  Topic 3: `ravel()` — Convert to 1D (Returns a View)

###  Theory

`ravel()` also converts a multi-dimensional array to **1D**, similar to `flatten()`.  
But there is one **critical difference**:

| | `flatten()` | `ravel()` |
|-|-------------|----------|
| Returns | **Copy** of data | **View** of original data (when possible) |
| Memory | Uses **more** memory | Uses **less** memory |
| Modifying result | Does NOT affect original | **CAN affect** the original! |
| Speed | Slightly slower | Slightly faster |

###  Analogy

- **`flatten()`** is like **photocopying** a document and then editing the copy. Original is safe.
- **`ravel()`** is like **looking at the original** through a window. If you write on the window, it might affect what's behind it!

###  When to use which?

- Use `flatten()` when you want to **safely modify** without touching the original.
- Use `ravel()` when you want **memory efficiency** and don't need to modify.
```python
# ============================================================
# TOPIC 3 EXAMPLE: ravel() vs flatten()
# ============================================================

import numpy as np

arr2d = np.array([[10, 20, 30],
                  [40, 50, 60]])

print("Original 2D Array:")
print(arr2d)

print()

# --- ravel() example ---
raveled = arr2d.ravel()
print("Raveled Array:", raveled)

# Modify the raveled array
raveled[0] = 999
print("\nAfter modifying raveled[0] = 999:")
print("raveled :", raveled)      # Changed
print("Original:")               # Original ALSO changed! (view, not copy)
print(arr2d)

print()
print("💡 This is the key difference from flatten() — ravel() is a VIEW!")
```
```
# ============================================================
# TOPIC 3 EXAMPLE: ravel() vs flatten()
# ============================================================

import numpy as np

arr2d = np.array([[10, 20, 30],
                  [40, 50, 60]])

print("Original 2D Array:")
print(arr2d)

print()

# --- ravel() example ---
raveled = arr2d.ravel()
print("Raveled Array:", raveled)

# Modify the raveled array
raveled[0] = 999
print("\nAfter modifying raveled[0] = 999:")
print("raveled :", raveled)      # Changed
print("Original:")               # Original ALSO changed! (view, not copy)
print(arr2d)

print()
print("💡 This is the key difference from flatten() — ravel() is a VIEW!")
```
```python
# ============================================================
# TOPIC 3 EXAMPLE: Side-by-side comparison
# ============================================================

import numpy as np

arr = np.array([[1, 2, 3],
                [4, 5, 6]])

flat_copy = arr.flatten()   # COPY
rav_view  = arr.ravel()     # VIEW

print("flatten() result:", flat_copy)
print("ravel()   result:", rav_view)

print()
print("Both give same output — but behavior on modification is different!")
print("Use flatten() for safety. Use ravel() for speed.")
```
```
flatten() result: [1 2 3 4 5 6]
ravel()   result: [1 2 3 4 5 6]

Both give same output — but behavior on modification is different!
Use flatten() for safety. Use ravel() for speed.
```
---

##  Topic 4: Random Numbers — `np.random`

###  Theory

NumPy's `np.random` module allows us to **generate random numbers**.  
Random numbers are very useful in:
- Creating fake/sample data for testing
- Simulating real-world events (dice rolls, coin flips)
- Machine Learning (initializing weights, shuffling data)

###  Most Common Random Functions

| Function | What it does |
|----------|--------------|
| `np.random.rand(n)` | `n` random floats between **0 and 1** (uniform) |
| `np.random.rand(r, c)` | Random floats in shape `(r, c)` |
| `np.random.randint(low, high, size)` | Random **integers** between low and high-1 |
| `np.random.randn(n)` | `n` random floats from **normal distribution** (mean=0, std=1) |
| `np.random.choice(arr, size)` | Randomly pick elements from an array |
| `np.random.shuffle(arr)` | Shuffle array elements **in place** |

###  Analogy

- `rand()` is like spinning a wheel that can stop anywhere between 0 and 1.
- `randint()` is like rolling a die — you get whole numbers.
- `randn()` is like picking height of a random person — values cluster around the average.
```python
# ============================================================
# TOPIC 4 EXAMPLE: np.random — Basic Functions
# ============================================================

import numpy as np

# --- rand(): Random floats between 0 and 1 ---
# r1 = np.random.rand(5)
# print("rand(5)  — 5 random floats [0, 1):", r1)

# # # --- rand() 2D ---
# r2 = np.random.rand(6, 4)
# print("\nrand(3,4) — 3x4 random float matrix:")
# print(np.round(r2, 2))

# print()

# # --- randint(): Random integers ---
ri = np.random.randint(1, 100, size=8)   # 8 integers between 1 and 99
print("randint(1,100,8) — 8 random integers:", ri)

# # 2D randint
ri2d = np.random.randint(0, 32, size=(3, 3))
print("\nrandint 3x3 matrix:")
print(ri2d)

# print()

# --- randn(): Normal distribution ---
rn = np.random.randn(6)
print("randn(6) — 6 values from normal distribution:", np.round(rn, 3))
```
```
randint(1,100,8) — 8 random integers: [78 66 99 33 16 55 67 27]

randint 3x3 matrix:
[[11 21 13]
 [12 29 26]
 [20 15  5]]
randn(6) — 6 values from normal distribution: [-0.186 -0.462  1.305  0.265 -0.449  0.841]
```
```python
# ============================================================
# TOPIC 4 EXAMPLE: choice() and shuffle()
# ============================================================

import numpy as np

students = np.array(['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank'])
print("All Students:", students)
print()

# --- choice(): Pick random students ---
selected = np.random.choice(students, size=(3,4))
print("Randomly selected 3 students:\n", selected)

print()

# --- shuffle(): Shuffle the order ---
arr = np.arange(1, 11)     # [1, 2, 3, ..., 10]
print("Before shuffle:", arr)
np.random.shuffle(arr)     # Shuffles IN PLACE (modifies the array)
print("After shuffle :", arr)

print()

# --- Simulate a dice roll ---
dice_rolls = np.random.randint(1, 7, size=10)   # 10 dice rolls
print("10 Dice Rolls:", dice_rolls)
```
```
All Students: ['Alice' 'Bob' 'Charlie' 'Diana' 'Eve' 'Frank']

Randomly selected 3 students:
 [['Bob' 'Bob' 'Alice' 'Charlie']
 ['Eve' 'Diana' 'Diana' 'Charlie']
 ['Eve' 'Charlie' 'Alice' 'Alice']]

Before shuffle: [ 1  2  3  4  5  6  7  8  9 10]
After shuffle : [ 3  4  8  2 10  1  7  9  6  5]

10 Dice Rolls: [3 3 1 2 2 2 2 4 4 3]
```
---

##  Topic 5: Seed — Making Randomness Reproducible

###  Theory

Every time you run `np.random.rand()` or similar functions, you get **different numbers**.  
But in **data science and machine learning**, we often need the **same random numbers every time** — so that results are reproducible and comparable.

This is done using a **seed** — a starting number that controls the random number generator.

###  Syntax

```python
np.random.seed(42)    # Set seed to 42 (any integer works)
np.random.rand(5)     # Will ALWAYS give the same 5 numbers when seed=42
```

###  Analogy

Think of it like a **shuffled deck of cards**.  
Normally, every shuffle is different.  
But if you use a **magic word (seed)** before shuffling, you always get the **same shuffle** — like magic!

###  Why is seed important?

- **Reproducibility**: Share your code — others get the same results.
- **Debugging**: Easier to find bugs when outputs don't change each run.
- **Fairness**: ML model training comparisons are fair when data split is same.

The number `42` is commonly used by convention (a reference to *The Hitchhiker's Guide to the Galaxy*), but you can use any integer!

```python
# ============================================================
# TOPIC 5 EXAMPLE: np.random.seed()
# ============================================================

import numpy as np

# --- Without seed: Different every run ---
# print("=== Without Seed ===")
# print("Run 1:", np.random.randint(1, 100, 5))
# print("Run 2:", np.random.randint(1, 100, 5))
# print("(Notice: values are different each time you run!)")

# print()

# # # --- With seed: Same every run ---
# # print("=== With Seed = 42 ===")

np.random.seed(12)
print("Run 1:", np.random.randint(1, 100, 5))

np.random.seed(12)   # Reset seed before each run
print("Run 2:", np.random.randint(1, 100, 5))

np.random.seed(72)
print("Run 3:", np.random.randint(1, 100, 5))
# # print("(Notice: all runs give EXACTLY same numbers!)")

# # print()

# # # --- Practical: Reproducible sample data ---
# np.random.seed(0)
# student_scores = np.random.randint(40, 100, size=10)
# print("Reproducible student scores:", student_scores)
```
```
Run 1: [76 28  7  3  4]
Run 2: [76 28  7  3  4]
Run 3: [89 20 47 75 95]
```
---

##  Topic 6: Percentiles and IQR

###  Theory

**Percentile** tells you what value is at a certain **percentage position** in the sorted data.

For example:
- **25th percentile (Q1)**: 25% of the data is **below** this value.
- **50th percentile (Q2 / Median)**: 50% of the data is below this value.
- **75th percentile (Q3)**: 75% of the data is below this value.

###  IQR — Interquartile Range

**IQR = Q3 − Q1**

IQR tells us the **spread of the middle 50%** of the data.  
It is very useful for detecting **outliers** (extreme values that don't fit).

###  Real-Life Analogy

In a class of 100 students sorted by height:  
- **Q1 (25th percentile)**: Height of the 25th student — 25% are shorter.
- **Q3 (75th percentile)**: Height of the 75th student — 75% are shorter.
- **IQR** = Q3 - Q1 = the range of heights for the "middle group" of students.

###  Outlier Detection using IQR

Values that fall outside this range are considered **outliers**:
```
Lower Bound = Q1 − (1.5 × IQR)
Upper Bound = Q3 + (1.5 × IQR)
```
Any value below Lower Bound or above Upper Bound = **outlier**.
```python
# ============================================================
# TOPIC 6 EXAMPLE: Percentiles
# ============================================================

import numpy as np

scores = np.array([45, 52, 60, 63, 68, 72, 75, 80, 85, 90, 92, 95])
print("Scores:", scores)
print()

# Individual percentiles
p25 = np.percentile(scores, 25)    # Q1
p50 = np.percentile(scores, 50)    # Q2 / Median
p75 = np.percentile(scores, 75)    # Q3

print("25th Percentile (Q1)   :", p25)
print("50th Percentile (Q2)   :", p50, " ← This is the Median")
print("75th Percentile (Q3)   :", p75)


print()

# Multiple percentiles at once
q1, q2, q3 = np.percentile(scores, [25, 50, 75])
print(f"Q1={q1}, Q2={q2}, Q3={q3}")
```
```
Scores: [45 52 60 63 68 72 75 80 85 90 92 95]

25th Percentile (Q1)   : 62.25
50th Percentile (Q2)   : 73.5  ← This is the Median
75th Percentile (Q3)   : 86.25

Q1=62.25, Q2=73.5, Q3=86.25
```
```python
# ============================================================
# TOPIC 6 EXAMPLE: IQR and Outlier Detection
# ============================================================

import numpy as np

# Scores with outliers
scores = np.array([55, 60, 62, 65, 68, 70, 72, 75, 78, 80, 5, 150])
print("Scores (with outliers):", scores)
print()

# Calculate Q1, Q3
Q1 = np.percentile(scores, 25)
Q3 = np.percentile(scores, 75)
IQR = Q3 - Q1

print(f"Q1  = {Q1}")
print(f"Q3  = {Q3}")
print(f"IQR = Q3 - Q1 = {Q3} - {Q1} = {IQR}")

print()

# Outlier boundaries
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

print(f"Lower Bound = Q1 - 1.5*IQR = {lower_bound}")
print(f"Upper Bound = Q3 + 1.5*IQR = {upper_bound}")

print()

# Find outliers using boolean masking
outliers = scores[(scores < lower_bound) | (scores > upper_bound)]
normal   = scores[(scores >= lower_bound) & (scores <= upper_bound)]

print("Outliers found:", outliers)
print("Normal scores :", normal)
```
```
Scores (with outliers): [ 55  60  62  65  68  70  72  75  78  80   5 150]

Q1  = 61.5
Q3  = 75.75
IQR = Q3 - Q1 = 75.75 - 61.5 = 14.25

Lower Bound = Q1 - 1.5*IQR = 40.125
Upper Bound = Q3 + 1.5*IQR = 97.125

Outliers found: [  5 150]
Normal scores : [55 60 62 65 68 70 72 75 78 80]
```
---

##  Topic 7: Missing Values — `np.nan` and `np.isnan()`

###  Theory

In real-world data, some values are often **missing** — a student didn't appear for an exam, a sensor didn't record a reading, a survey question was skipped.  
NumPy represents missing values using a special value: **`np.nan`** (Not a Number).

###  What is `np.nan`?

- `nan` stands for **Not a Number**.
- It is a **float** value — so arrays with `nan` must be float type.
- Any arithmetic with `nan` returns `nan`:
  ```python
  np.nan + 5    → nan
  np.nan * 100  → nan
  ```

###  Detecting Missing Values — `np.isnan()`

`np.isnan(arr)` returns a **boolean array** — `True` wherever the value is `nan`.

###  Safe Aggregation with NaN-aware functions

Regular functions like `np.mean()` return `nan` if any element is `nan`.  
Use **NaN-safe functions** instead:

| Regular | NaN-safe version |
|---------|------------------|
| `np.sum()` | `np.nansum()` |
| `np.mean()` | `np.nanmean()` |
| `np.min()` | `np.nanmin()` |
| `np.max()` | `np.nanmax()` |
| `np.std()` | `np.nanstd()` |
| `np.median()` | `np.nanmedian()` |
```python
# ============================================================
# TOPIC 7 EXAMPLE: np.nan and np.isnan()
# ============================================================

import numpy as np

# Array with missing values (NaN)
scores = np.array([85.0, np.nan, 72.0, np.nan, 91.0, 60.0, np.nan, 78.0])
print("Scores with missing values:", scores)

print()

# --- Detect missing values ---
nan_mask = np.isnan(scores)
print("Is NaN mask  :", nan_mask)
print("Count of NaN :", np.sum(nan_mask))   # How many missing?

print()

# --- Problem with regular functions ---
print("np.mean() with NaN  :", np.mean(scores))    # Returns nan!

# --- NaN-safe functions ---
print("np.nanmean()        :", np.nanmean(scores))   # Ignores nan
print("np.nansum()         :", np.nansum(scores))
print("np.nanmin()         :", np.nanmin(scores))
print("np.nanmax()         :", np.nanmax(scores))
print("np.nanmedian()      :", np.nanmedian(scores))
print("np.nanstd()         :", np.round(np.nanstd(scores), 2))
```
```
Scores with missing values: [85. nan 72. nan 91. 60. nan 78.]

Is NaN mask  : [False  True False  True False False  True False]
Count of NaN : 3

np.mean() with NaN  : nan
np.nanmean()        : 77.2
np.nansum()         : 386.0
np.nanmin()         : 60.0
np.nanmax()         : 91.0
np.nanmedian()      : 78.0
np.nanstd()         : 10.72
```
```python
# ============================================================
# TOPIC 7 EXAMPLE: Replacing Missing Values
# ============================================================

import numpy as np

scores = np.array([85.0, np.nan, 72.0, np.nan, 91.0, 60.0, np.nan, 78.0])
print("Original Scores:", scores)

print()

# Strategy 1: Replace NaN with 0
filled_zero = scores.copy()
filled_zero[np.isnan(filled_zero)] = 0
print("NaN replaced with 0   :", filled_zero)

# Strategy 2: Replace NaN with the mean of available scores
filled_mean = scores.copy()
mean_val = np.nanmean(filled_mean)
filled_mean[np.isnan(filled_mean)] = mean_val
print("NaN replaced with mean:", np.round(filled_mean, 2))

print()
print(f"Mean used for replacement: {mean_val:.2f}")
print("💡 Replacing with mean is a common technique called 'Mean Imputation'")
```
```
Original Scores: [85. nan 72. nan 91. 60. nan 78.]

NaN replaced with 0   : [85.  0. 72.  0. 91. 60.  0. 78.]
NaN replaced with mean: [85.  77.2 72.  77.2 91.  60.  77.2 78. ]

Mean used for replacement: 77.20
💡 Replacing with mean is a common technique called 'Mean Imputation'
```
---

##  Topic 8: Sorting and Unique Values

###  Theory

###  Sorting with `np.sort()`

`np.sort()` returns a **new sorted array** (does not modify original).  
`arr.sort()` sorts **in place** (modifies the original).

```python
np.sort(arr)             # Sort ascending (default)
np.sort(arr)[::-1]       # Sort descending (reverse)
np.sort(arr2d, axis=0)   # Sort each column (vertically)
np.sort(arr2d, axis=1)   # Sort each row (horizontally)
```

###  `np.argsort()` — Get Sorted Indices

`np.argsort()` returns the **indices** that would sort the array.  
Very useful when you want to know the **rank order** of elements.

###  Unique Values with `np.unique()`

`np.unique()` returns an array of **unique (non-duplicate) values** in sorted order.

```python
np.unique(arr)                     # Unique values
np.unique(arr, return_counts=True) # Unique values + how many times each appears
```
```python
# ============================================================
# TOPIC 8 EXAMPLE: Sorting with np.sort()
# ============================================================

import numpy as np

scores = np.array([72, 45, 91, 60, 85, 38, 78, 55])
print("Original Scores:", scores)
print()

# Ascending sort (default)
asc = np.sort(scores)
print("Ascending  :", asc)

# Descending sort
desc = np.sort(scores)[::-1]
print("Descending :", desc)

# Original is unchanged when using np.sort()
print("Original   :", scores, "← unchanged!")

print()

# --- 2D Sorting ---
marks = np.array([[50, 30, 70],
                  [80, 10, 60],
                  [40, 90, 20]])
print("2D Array:")
print(marks)

print("\nSorted by rows (axis=1 — sort within each row):")
print(np.sort(marks, axis=1))

print("\nSorted by columns (axis=0 — sort within each column):")
print(np.sort(marks, axis=0))
```
```
Original Scores: [72 45 91 60 85 38 78 55]

Ascending  : [38 45 55 60 72 78 85 91]
Descending : [91 85 78 72 60 55 45 38]
Original   : [72 45 91 60 85 38 78 55] ← unchanged!

2D Array:
[[50 30 70]
 [80 10 60]
 [40 90 20]]

Sorted by rows (axis=1 — sort within each row):
[[30 50 70]
 [10 60 80]
 [20 40 90]]

Sorted by columns (axis=0 — sort within each column):
[[40 10 20]
 [50 30 60]
 [80 90 70]]
```
```python
# ============================================================
# TOPIC 8 EXAMPLE: argsort() and Ranking
# ============================================================

import numpy as np

scores = np.array([72, 45, 91, 60, 85])
names  = np.array(['Alice', 'Bob', 'Charlie', 'Diana', 'Eve'])

print("Students :", names)
print("Scores   :", scores)

# argsort gives indices in sorted order
sorted_idx = np.argsort(scores)
print("\nArgsort indices (ascending):", sorted_idx)

# # Use indices to rank students
print("\nStudents ranked from lowest to highest score:")
for rank, idx in enumerate(sorted_idx, start=1):
    print(f"  Rank {rank}: {names[idx]} — {scores[idx]}")

# # Top scorer
# top_idx = np.argmax(scores)
# print(f"\nTop Scorer: {names[top_idx]} with {scores[top_idx]} marks")
```
```
Students : ['Alice' 'Bob' 'Charlie' 'Diana' 'Eve']
Scores   : [72 45 91 60 85]

Argsort indices (ascending): [1 3 0 4 2]

Students ranked from lowest to highest score:
  Rank 1: Bob — 45
  Rank 2: Diana — 60
  Rank 3: Alice — 72
  Rank 4: Eve — 85
  Rank 5: Charlie — 91
```
```python
# ============================================================
# TOPIC 8 EXAMPLE: np.unique()
# ============================================================

import numpy as np

grades = np.array(['B', 'A', 'C', 'A', 'B', 'A', 'D', 'C', 'B', 'A'])
print("All Grades:", grades)

print()

# Get unique grades
unique_grades = np.unique(grades)
print("Unique Grades:", unique_grades)

# Get unique values AND how many times each appears
unique_vals, counts = np.unique(grades, return_counts=True)

print("\nGrade Distribution:")
for grade, count in zip(unique_vals, counts):
    print(f"  Grade {grade}: {count} students")

print()

# Works for numbers too
nums = np.array([4, 2, 7, 2, 9, 4, 4, 7, 1])
unique_nums, num_counts = np.unique(nums, return_counts=True)
print("Unique Numbers:", unique_nums)
print("Counts        :", num_counts)
```
```
All Grades: ['B' 'A' 'C' 'A' 'B' 'A' 'D' 'C' 'B' 'A']

Unique Grades: ['A' 'B' 'C' 'D']

Grade Distribution:
  Grade A: 4 students
  Grade B: 3 students
  Grade C: 2 students
  Grade D: 1 students

Unique Numbers: [1 2 4 7 9]
Counts        : [1 2 3 2 1]
```
## Practice Questions:
<img width="889" height="821" alt="image" src="https://github.com/user-attachments/assets/ccb0c901-e9cc-426c-838b-4b8030cd3a99" />

<img width="974" height="678" alt="image" src="https://github.com/user-attachments/assets/70993379-5a81-4c30-b556-bd8060a51670" />

<img width="933" height="769" alt="image" src="https://github.com/user-attachments/assets/b2af2a3a-57fe-4ec6-bcf3-d890f16905ba" />

<img width="968" height="817" alt="image" src="https://github.com/user-attachments/assets/0e113fb9-b808-4e77-b3d7-efec7e0363e3" />

<img width="960" height="803" alt="image" src="https://github.com/user-attachments/assets/94e10d65-366e-44ce-8407-1984a838a405" />

<img width="985" height="859" alt="image" src="https://github.com/user-attachments/assets/f9ed151b-e42c-4b0b-98b8-0f3fbd239090" />

<img width="981" height="651" alt="image" src="https://github.com/user-attachments/assets/6d0a6d79-fba5-4053-ab9c-d6d59ae213e6" />


