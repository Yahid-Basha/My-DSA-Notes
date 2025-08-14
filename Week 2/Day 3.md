- [Minimum Size Subarray Sum](#Minimum-Size-Subarray-Sum)
- [Find K Closest Elements](#Find-K-Closest-Elements)
- [Minimum Window Substring](#Minimum-Window-Substring)
- [Sliding Window Maximum](#Sliding-Window-Maximum)

**Minimum Size Subarray Sum**

---

## **Key Idea**

Since all elements are **positive**, we can use a **sliding window** to find the smallest subarray with sum ≥ target.
We grow the right pointer to increase the sum, and shrink the left pointer *while* the sum still satisfies the condition — this guarantees the window length is minimal.

---

## **Algorithm**

1. Initialize:

   * `l = 0`, `sum = 0`, `minLen = nums.length + 1`
2. Iterate with `r` from 0 to n-1:

   * Add `nums[r]` to `sum`
   * While `sum >= target`:

     * Update `minLen = Math.min(minLen, r - l + 1)`
     * Subtract `nums[l]` from sum and increment `l`
3. Return `minLen` if updated, else return `0`.

---

## **Why This Works**

* **Positive integers** mean that once sum ≥ target, removing elements from the left is the only way to shorten the window without missing valid subarrays.
* The *while loop* ensures we don’t stop shrinking too early — every shrink is a chance to find a smaller valid subarray.

---

## **Common Traps (Your Mistakes)**

1. **Pausing `r` when overshoot happens**

   * ❌ You thought “stop moving right and move `l` once, then continue”
   * This misses cases where multiple left shrinks still satisfy the target and reduce length.
   * ✅ Correct: Always increment `r` in the outer loop; shrink repeatedly inside a `while(sum >= target)` loop.

2. **Thinking in terms of `l--`**

   * ❌ Mentally picturing `l--` when shrinking is backwards — you’re moving the left boundary **forward** (`l++`) to drop elements.
   * ✅ Always subtract `nums[l]` before incrementing `l`.

3. **Updating min length only once per overshoot**

   * ❌ You checked min length only when sum first hit ≥ target.
   * ✅ You must check after *every* shrink, because the minimum could happen at a smaller subarray inside the current window.

---

## **Dry Run Reminder**

Example: `nums = [2,3,1,2,4,3], target = 7`

* Correct approach finds `[4,3]` length=2.
* Your earlier logic skipped it because it didn’t fully shrink before moving `r`.

---

## **Complexity**

* **Time:** O(n) — each pointer moves at most `n` times.
* **Space:** O(1) — constant extra space.


---

## **Find K Closest Elements**

**Given:**

* Sorted array `arr` (integers)
* Integers `k` and `x`
* Return the `k` closest elements to `x`, sorted in ascending order
* Tie-breaker: if distances are equal, choose the smaller element

---

### **Key Observations**

1. **Sorted array** → We can use a two-pointer shrinking window instead of sorting by distance (avoids O(n log n) sorting).
2. Start with the full array as the window: `l = 0`, `r = n-1`.
3. While window size > `k`:

   * Compare `abs(arr[l] - x)` and `abs(arr[r] - x)`.
   * Remove the element that’s **farther** away.
   * On a tie (equal distances), remove from **right** → keep smaller number on the left (problem’s tie-breaker rule).
4. Remaining elements between `l` and `r` are the answer.

---

### **Final Approach**

```java
class Solution {
    public List<Integer> findClosestElements(int[] nums, int k, int x) {
        int l = 0;
        int r = nums.length - 1;

        while ((r - l + 1) > k) {
            if (Math.abs(nums[l] - x) > Math.abs(nums[r] - x)) {
                l++;
            } else {
                r--;
            }
        }

        List<Integer> list = new ArrayList<>();
        for (int i = l; i <= r; i++) {
            list.add(nums[i]);
        }
        return list;
    }
}
```

---

### **Pitfalls You Hit**

1. **Overcomplicated tie-breaker**

   * You wrote an extra `else if` for “right closer” and an `else` for “equal” when both led to `r--`.
   * **Fix:** Merge them — `else { r--; }` handles both. Cleaner, less error-prone.

2. **Tie-breaker interpretation**

   * You initially thought “equal distance → move `r` inward” by chance, but didn’t anchor it to the rule “keep smaller element.” You must *state the reasoning* in interviews.

3. **Result list assembly missing**

   * You forgot to actually collect and return the `k` elements. In a timed interview, that’s a completeness penalty.

4. **Boundary reasoning**

   * The correctness relies on the fact that the array is sorted — once a side is farther, it stays farther. This wasn’t verbalized initially, but should be mentioned to justify O(n) time.

---

### **Time & Space**

* **Time:** O(n) → each pointer moves inward at most `n` times.
* **Space:** O(k) for the output list.

---

### **Interview Soundbite**

> “I’ll solve this in O(n) using two pointers because the array is sorted. Start with the full array as a window and repeatedly remove the element farther from x. If equal distances, keep the smaller value by trimming from the right. This ensures minimal distance elements remain without extra sorting.”

---

### **Minimum Window Substring**

---

## **Approach**

**Goal:** Find the smallest substring of `s` that contains all characters of `t` (including duplicates).

1. **Frequency maps**

   * `need`: Stores required character counts from `t`.
   * `window`: Stores current counts of characters in the sliding window.

2. **Two pointers** (`l` and `r`) to represent the current window.

   * Expand `r` to include more characters.
   * Shrink `l` only when all requirements are satisfied.

3. **Tracking matches**

   * `required = need.size()` → number of distinct chars needed.
   * `have` → number of distinct chars currently satisfied (i.e., `window[c] == need[c]`).

4. **Updating result**

   * Every time `have == required`, check if current window is smaller than the best one so far.
   * Update `minLen` and `ans` if smaller.

5. **Shrinking logic**

   * Remove `s[l]` from the window.
   * If removing breaks the requirement, decrease `have`.

---

## **Common Pitfalls You Made**

1. **Wrong variable when accessing characters**

   * Used `s.charAt(i)` when the loop variable is `r`.

2. **Substring arguments reversed**

   * Wrote `substring(r, l+1)` instead of `substring(l, r+1)`.

3. **`window.put` misuse**

   * Missed the key argument in `window.put(...)`.

4. **NullPointerException risk**

   * Compared `window.get(c)` directly with `need.get(c)` without checking if `c` exists in `need`.

5. **Recomputing `have` incorrectly**

   * You tried to recompute it in the loop instead of incrementing/decrementing in sync with pointer movement.

---

## **Dry Run Example**

**Input:**
`s = "ADOBECODEBANC"`
`t = "ABC"`

**Step-by-step:**

```
need = {A:1, B:1, C:1}
required = 3
have = 0
window = {}

r=0 ('A'): window={A:1}, have=1
r=1 ('D'): window={A:1,D:1}, have=1
r=2 ('O'): window={A:1,D:1,O:1}, have=1
r=3 ('B'): window={A:1,D:1,O:1,B:1}, have=2
r=4 ('E'): ...
r=5 ('C'): have=3 → valid window (l=0..5), minLen=6 ("ADOBEC")
  Shrink l:
    remove 'A' → have=2, l=1

Continue expanding/shrinking...
Eventually best window = "BANC" (length 4).
```

---

## **Final Code**

```java
class Solution {
    public String minWindow(String s, String t) {
        Map<Character, Integer> need = new HashMap<>();
        Map<Character, Integer> window = new HashMap<>();
        
        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        
        int required = need.size();
        int have = 0;
        int l = 0;
        int minLen = Integer.MAX_VALUE;
        String ans = "";
        
        for (int r = 0; r < s.length(); r++) {
            char c = s.charAt(r);
            window.put(c, window.getOrDefault(c, 0) + 1);
            
            if (need.containsKey(c) && window.get(c).intValue() == need.get(c).intValue()) {
                have++;
            }
            
            while (have == required) {
                if (r - l + 1 < minLen) {
                    minLen = r - l + 1;
                    ans = s.substring(l, r + 1);
                }
                
                char leftChar = s.charAt(l);
                window.put(leftChar, window.get(leftChar) - 1);
                if (need.containsKey(leftChar) && window.get(leftChar) < need.get(leftChar)) {
                    have--;
                }
                l++;
            }
        }
        
        return ans;
    }
}
```
---

 ## **Sliding Window Maximum**

---

## **Approach (Deque Method)**

**Key Insight:**
Maintain a **monotonic decreasing deque** (stores **indices**, not values).

* **Front** of deque → index of current max element in the window.
* **Back** of deque → smallest values, which we pop when they are less than the current value.

**Steps:**

1. Iterate through each index `i` in `nums`.
2. **Clean front:** If `deque.front()` is out of the window (`i - deque.front() >= k`), pop it from front.
3. **Clean back:** While deque is not empty and `nums[i] >= nums[deque.back()]`, pop from back.
4. Push current index `i` into deque.
5. If we’ve processed at least `k` elements, add `nums[deque.front()]` to result.

---

## **Dry Run Example**

**Input:**
`nums = [1, 3, -1, -3, 5, 3, 6, 7], k = 3`

```
i=0: deque=[], clean front/back not needed
       deque=[0] (value=1)
i=1: nums[1]=3 >= nums[0]=1 → pop back
       deque=[1]
i=2: nums[2]=-1 < nums[1]=3 → keep
       deque=[1, 2]
       First window done → max = nums[1]=3
i=3: remove out-of-range? deque.front=1, 3-1<3 → no
     nums[3]=-3 < nums[2]=-1 → keep
       deque=[1, 2, 3]
       max=nums[1]=3
i=4: deque.front=1, 4-1>=3 → pop front (index 1 removed)
     nums[4]=5 > nums[3]=-3 → pop 3
     nums[4]=5 > nums[2]=-1 → pop 2
       deque=[4]
       max=nums[4]=5
...
Final result = [3, 3, 5, 5, 6, 7]
```

---

## **Mistakes You Usually Make**

1. **Storing values in deque** instead of indices → makes it hard to check window bounds.
2. **Wrong window removal condition**: Must be `i - deque.front() >= k`, not `>` or `==`.
3. **Not popping smaller/equal elements from back** → breaks monotonic property.
4. **Adding results too early** — only add when `i >= k - 1`.

---

## **Final Code (Java)**

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> deque = new LinkedList<>();
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            // 1. Remove out-of-window indices
            if (!deque.isEmpty() && deque.peekFirst() <= i - k) {
                deque.pollFirst();
            }
            
            // 2. Remove smaller elements from back
            while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {
                deque.pollLast();
            }
            
            // 3. Add current index
            deque.offerLast(i);
            
            // 4. Add current max to result when first window is ready
            if (i >= k - 1) {
                result.add(nums[deque.peekFirst()]);
            }
        }
        
        // Convert to int[]
        int[] ans = new int[result.size()];
        for (int i = 0; i < result.size(); i++) ans[i] = result.get(i);
        return ans;
    }
}
```

---

You want me to prep **Minimum Window Substring** notes in the same style *after* you solve it, so your repo has consistent high-quality documentation? That’ll make your revision much sharper.

