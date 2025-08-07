# Day 2

## 📘 **QuickSort Core Notes**

### 🔁 General QuickSort Template (Pseudocode)

```java
quickSort(arr, low, high):
    if (low < high):
        pivotIndex = partition(arr, low, high)
        quickSort(arr, low, pivotIndex - 1)
        quickSort(arr, pivotIndex + 1, high)
```

---

## ✂️ **Lomuto Partition Scheme**

### 🧠 **Concept**

* Picks **last element** as pivot.
* Partitions so that:

  * All elements < pivot → left side
  * Pivot is placed at its correct sorted position.

### 📌 Pseudocode:

```java
partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j = low to high - 1:
        if (arr[j] < pivot):
            i++
            swap(arr, i, j)
    swap(arr, i + 1, high)
    return i + 1
```

### 🧩 Key Ideas:

* `i` points to the **last known smaller** element.
* Swap `i+1` with `pivot` at the end to place pivot in correct spot.

### ✅ Works well with:

* Simple logic
* Stable sort requirement (with mods)

### ❌ Drawbacks:

* Not efficient on duplicates or skewed data
* Can cause worst-case O(n²) if pivot is poor (e.g. already sorted array)

### 🧠 Mental Model:

* Think of a **“< pivot zone”** growing to the left of `i`
* `j` scouts forward, and expands that zone

---

## ⚔️ **Hoare Partition Scheme**

### 🧠 **Concept**

* Picks **first element** as pivot.
* Partitions so that:

  * Left side: ≤ pivot
  * Right side: ≥ pivot
* Does **not guarantee** pivot lands at its final position.

### 📌 Pseudocode:

```java
partition(arr, low, high):
    pivot = arr[low]
    i = low - 1
    j = high + 1
    while (true):
        do i++ while (arr[i] < pivot)
        do j-- while (arr[j] > pivot)
        if (i >= j):
            return j
        swap(arr, i, j)
```

### 🧩 Key Ideas:

* `i` moves right, `j` moves left.
* When out-of-place elements found on both sides, swap.
* Stop when they cross.
* **Return `j`** as the new pivot boundary.

### ✅ Strengths:

* Fewer swaps than Lomuto
* Handles duplicates better
* Great for randomized or large datasets

### ❌ Drawbacks:

* Harder to implement correctly
* Doesn’t return pivot’s final position

### 🧠 Mental Model:

* Two gladiators (`i` and `j`) pushing inward toward the pivot.
* Swap wrong-side elements as they fight their way in.

---

## 📝 ✨ **Quick Comparison Table**

| Feature            | Lomuto            | Hoare                  |
| ------------------ | ----------------- | ---------------------- |
| Pivot chosen from  | `arr[high]`       | `arr[low]`             |
| Return value       | Final pivot index | Any index in partition |
| Swaps              | More              | Fewer                  |
| Handles duplicates | Poorly            | Better                 |
| Easy to implement  | ✅                 | ❌ (more error-prone)   |
| Recursion Ranges   | `p+1`, `p-1`      | `p`, `p+1`             |

---

## 🧠 Yahid’s Personal Drill List

**Once a week, revise with:**

1. 🧾 Dry run both schemes on:

   * `[-5, 0, 2, -1, 3, 2]`
   * `[5, 4, 3, 2, 1]`
   * `[1, 2, 3, 4, 5]`
   * `[3, 3, 3, 3, 3]`

2. ✍️ Re-write both `partition()`s from scratch — no peeking.

3. 🧪 Explain to yourself:

   * Why `i` and `j` move that way
   * Why swaps are necessary
   * When to stop the loop and what to return

---

## 📍 Revision Shortcut (Sticky Notes for Wall)

> “Lomuto is linear — `i` walks left to right. Hoare is two-sided — `i` and `j` fight from both sides.”

> “Lomuto returns pivot's true place. Hoare returns a wall between ≤ and ≥ pivot.”

---

If you want, I can also generate a **PDF version** of these notes for your Notion or GitHub vault — let me know.

You're not blanking out anymore — now you're **equipped**.
Next level: **apply this understanding** in real problems and move to the next sorting beast.
