# Day 2

- [Quick Sort](#quicksort)
- [Dutch Flag Algo](#dutch-flag)
- [Priority Queue](#📒-priorityqueue-(heap))


## 📘 **QuickSort**

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
🔥 That’s what I’m talking about, Yahid. You just **leveled up**.
You nailed **Quick Sort** and **Counting**, and now you’re reaching for a new weapon: **Dutch National Flag**. Let’s forge it.

---

## Dutch Flag

### Problem:

Given an array with only 3 types of values — `0`, `1`, `2` — sort it **in-place** in **one pass**.

---

### 💡 Insight:

Only 3 possible values → instead of comparing, swapping randomly, or counting, we can **position them directly**.

Think of the array as three zones:

```
[0s | 1s | unknown | 2s]
 ↑     ↑       ↑     ↑
low  mid      ???   high
```

We **maintain 3 pointers**:

| Pointer | Purpose                          |
| ------- | -------------------------------- |
| `low`   | End of the 0s zone               |
| `mid`   | Current element under evaluation |
| `high`  | Start of the 2s zone             |

---

### 🔁 The Process (in words)

At every step, check `nums[mid]`:

1. **If it’s `0`**:

   * Swap with `nums[low]`
   * `low++`, `mid++`
     *(because 0 is now in the correct zone)*

2. **If it’s `1`**:

   * Leave it.
   * Just `mid++`
     *(1s go in the middle, so no move needed)*

3. **If it’s `2`**:

   * Swap with `nums[high]`
   * `high--`
     *(don't increment `mid` because we need to recheck what we just swapped in)*

---

### ⚠️ Why is this optimal?

* **O(n)** time: each element is visited at most once
* **O(1)** space: only 3 pointers
* **In-place**: no extra structures

---

### 🧠 Visualize It

Try dry running this on:

```java
[2, 0, 2, 1, 1, 0]
```

Initial:
`low = 0`, `mid = 0`, `high = 5`

* `nums[mid] = 2` → swap with `high`, now check mid again
* `nums[mid] = 0` → swap with `low`, then `low++`, `mid++`
* etc.

You'll see how the array **sorts itself** in one smooth pass.

---

Perfect. Let’s fix that **once and for all.** No more “I suck at Java heap stuff” from today.

Here’s a 🔥 **no-fluff, must-know Java Heap / PriorityQueue** cheatsheet — tuned for **LeetCode + interviews.**

---

## 📒 PriorityQueue (Heap)

### 🧠 Basic Concept:

* `PriorityQueue` is a **min-heap by default**
* To build a **max-heap**, you use a custom comparator.

---

### ✅ 1. **Min-Heap (Default)** — Smallest comes first

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.add(5);
minHeap.add(1);
minHeap.add(3);

while (!minHeap.isEmpty()) {
    System.out.print(minHeap.poll() + " ");  // Output: 1 3 5
}
```

---

### ✅ 2. **Max-Heap** — Largest comes first

```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
maxHeap.add(5);
maxHeap.add(1);
maxHeap.add(3);

// Output: 5 3 1
```

---

### ✅ 3. **Heap of Custom Objects** (e.g., element with frequency)

```java
class Node {
    int num;
    int freq;

    Node(int num, int freq) {
        this.num = num;
        this.freq = freq;
    }
}

// Max-Heap: higher freq comes first
PriorityQueue<Node> heap = new PriorityQueue<>((a, b) -> b.freq - a.freq);
```

🔁 For Min-Heap: use `a.freq - b.freq`

---

### ✅ 4. **Alternative: Heap with Map.Entry (no Node class needed)**

```java
Map<Integer, Integer> freqMap = new HashMap<>();
for (int num : nums)
    freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);

// Max-Heap of map entries
PriorityQueue<Map.Entry<Integer, Integer>> heap =
    new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());
heap.addAll(freqMap.entrySet());
```

---

### 💡 Quick Tips:

| 🔧 Problem Type                        | ✅ Use This Approach                          |
| -------------------------------------- | -------------------------------------------- |
| K smallest/largest elements            | Min-heap of size K or max-heap of N          |
| Frequency-based (Top K frequent)       | Max-heap with map counts                     |
| Real-time processing / streaming top K | Min-heap of size K (drop low priority)       |
| Custom sorting                         | Use `PriorityQueue<>((a, b) -> customLogic)` |

---

### 🔁 Bonus – Reverse Order for Strings (lexicographic)

```java
PriorityQueue<String> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

---

## 🧪 Practice Drill

> ❗ Practice using:
>
> * `PriorityQueue<Integer>`
> * `PriorityQueue<Node>` with comparator
> * `PriorityQueue<Map.Entry<K, V>>`

Write small snippets to:

* Push/pop
* Sort by custom fields
* Keep top K using size condition

---

## 🚀 Mission Now

> 🔥 Code the Top-K Frequent Elements
> Use **Map + Max-Heap**
> (try both `Node` and `Map.Entry` styles)
> And ask me to review — I’ll point out edge-case traps if any.

You master this, **you never say "I suck at Java heap" again.**
Let’s go.
