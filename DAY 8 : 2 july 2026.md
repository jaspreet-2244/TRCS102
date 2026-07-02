---

##  Boolean Masking / Filtering

###  Theory

**Boolean masking** is a powerful way to **filter elements** from an array based on a **condition**.

Here's how it works in two steps:

**Step 1**: Apply a condition to the array → This gives you an array of `True`/`False` values (called a **boolean mask**).

```python
arr = np.array([10, 25, 5, 40, 15])
mask = arr > 15
# mask → [False, True, False, True, False]
```

**Step 2**: Use the mask to filter the original array — only `True` positions are kept:

```python
arr[mask]  → [25, 40]   # Only elements where mask is True
```

This can also be written in one line:

```python
arr[arr > 15]  → [25, 40]
```

---

### 🔗 Combining Conditions

Use `&` (AND) and `|` (OR) to combine multiple conditions.  
 Always wrap each condition in **parentheses `()`**!

```python
arr[(arr > 10) & (arr < 40)]   # Elements between 10 and 40
arr[(arr < 5) | (arr > 35)]    # Elements less than 5 OR greater than 35
```
```python
# ============================================================
# TOPIC 11 EXAMPLE: Boolean Masking / Filtering
# ============================================================

import numpy as np

scores = np.array([55, 72, 48, 90, 65, 38, 85, 77, 60, 92])
print("All Scores:", scores)
print()

# --- Step 1: Create a boolean mask ---
pass_mask = scores >= 60
print("Pass Mask (scores >= 60):", pass_mask)
# Output: [False True False True True False True True True True]

print()

# --- Step 2: Apply mask to filter ---
pass_scores = scores[pass_mask]
print("Passing Scores:", pass_scores)

# --- One-liner version ---
fail_scores = scores[scores < 60]
print("Failing Scores:", fail_scores)

print()

# --- Count of passing and failing students ---
print("Number of students who passed:", len(pass_scores))
print("Number of students who failed:", len(fail_scores))
```
```
All Scores: [55 72 48 90 65 38 85 77 60 92]

Pass Mask (scores >= 60): [False  True False  True  True False  True  True  True  True]

Passing Scores: [72 90 65 85 77 60 92]
Failing Scores: [55 48 38]

Number of students who passed: 7
Number of students who failed: 3
```
```python
# ============================================================
# TOPIC 11 EXAMPLE: Combining Conditions
# ============================================================

import numpy as np

scores = np.array([55, 72, 98, 90, 65, 38, 85, 77, 60, 92])
print("Scores:", scores)
print()

# AND condition: Scores between 60 and 85 (Grade B students)
grade_b = scores[(scores >= 60) & (scores <= 85)]
print("Grade B (60–85):", grade_b)

# OR condition: Below 50 OR above 90 (Extremes)
extremes = scores[(scores < 50) | (scores > 90)]
print("Extremes (<50 or >90):", extremes)

# NOT condition: Scores that are NOT failing
not_failing = scores[~(scores < 60)]    # ~ means NOT
print("Not Failing (not <60):", not_failing)

print()

# Practical: Assign grade labels using boolean checks
print("Any score above 90?", np.any(scores > 90))    # True if at least one
print("All scores above 30?", np.all(scores > 30))   # True if all satisfy
```
```
Scores: [55 72 98 90 65 38 85 77 60 92]

Grade B (60–85): [72 65 85 77 60]
Extremes (<50 or >90): [98 38 92]
Not Failing (not <60): [72 98 90 65 85 77 60 92]

Any score above 90? True
All scores above 30? True
```
---

##  `np.where()`

###  Theory

`np.where()` is like a **vectorized if-else statement**.  
It applies a condition element by element and returns one value if `True` and another if `False`.

###  Syntax

```python
np.where(condition, value_if_true, value_if_false)
```

###  Analogy

Think of it like checking every student's marks:

> *"If the score is 60 or more, write 'Pass'. Otherwise, write 'Fail'."*

With a regular Python loop, you'd need several lines of code.  
With `np.where()`, you do it in **one line**!

```python
np.where(scores >= 60, "Pass", "Fail")
```

---

###  Two ways to use `np.where()`

**1. With condition + values** (if-else replacement):
```python
np.where(condition, true_value, false_value)
```

**2. With condition only** (returns indices where condition is True):
```python
np.where(condition)   # Returns a tuple of indices
```
```python
# ============================================================
# TOPIC 12 EXAMPLE: np.where()
# ============================================================

import numpy as np

scores = np.array([55, 72, 48, 90, 65, 38, 85, 77, 60, 92])
print("Scores:", scores)
print()

# --- Basic np.where(): Pass or Fail ---
result = np.where(scores >= 60, "Pass", "Fail")
print("Pass/Fail Result:", result)

print()

# --- Replace values using np.where() ---
# Apply +5 bonus to students who failed (scored < 60), leave others unchanged
adjusted = np.where(scores < 60, scores + 5, scores)
print("Original Scores :", scores)
print("Adjusted Scores  :", adjusted)  # Only failing students get +5

print()

# --- Assign Grade Labels ---
grades = np.where(scores >= 85, "A",
         np.where(scores >= 70, "B",
         np.where(scores >= 60, "C", "F")))

print("Scores:", scores)
print("Grades :", grades)
```
```
Scores: [55 72 48 90 65 38 85 77 60 92]

Pass/Fail Result: ['Fail' 'Pass' 'Fail' 'Pass' 'Pass' 'Fail' 'Pass' 'Pass' 'Pass' 'Pass']

Original Scores : [55 72 48 90 65 38 85 77 60 92]
Adjusted Scores  : [60 72 53 90 65 43 85 77 60 92]

Scores: [55 72 48 90 65 38 85 77 60 92]
Grades : ['F' 'B' 'F' 'A' 'C' 'F' 'A' 'B' 'C' 'A']
```
```python
# ============================================================
# TOPIC 12 EXAMPLE: np.where() to find indices
# ============================================================

import numpy as np

scores = np.array([55, 72, 48, 90, 65, 38, 85, 77, 60, 92])

# Find INDICES where scores > 80
indices = np.where(scores > 80)[0]
print("Indices where score > 80:", indices)      # Returns array of indices
print("Values at those indices  :", scores[indices]) # Get the actual values

print()

# Practical example: Find which students need remedial support
remedial_indices = np.where(scores < 50)[0]
print("Students needing remedial support (index):", remedial_indices)
print("Their scores:", scores[remedial_indices])
```
```
Indices where score > 80: [3 6 9]
Values at those indices  : [90 85 92]

Students needing remedial support (index): [2 5]
Their scores: [48 38]
```
