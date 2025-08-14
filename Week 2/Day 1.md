- [Container with most water](#container-with-most-water)
- [Minimum boats to save people](#boats-to-save-people)
- [Trapping rain Water](#trapping-rain-water)
- [Contains Duplicate II](#contains-duplicate-ii)
- [Help choose patter](#help)

## Container with Most Water

### Two Pointer

```Java
  public int maxArea(int[] heights) {
      int l = 0;
      int r = heights.length-1;
      int maost_water = 0;
      while(l < r){
	  	int width = r-l;
		int height = Math.min(heights[l], heights[r]);
		most_water = Math.max(most_water, width*height);
		if(heights[l] < heights[r]){
			l++;
		}else{
			r--;
		}
	  }
	  return most_water;
  }
  ```
 ---
  
  ### Optimised a bit
  ---
  
  ```Java
  	public int maxArea(int[] heights) {
        int l = 0;
        int r = heights.length-1;
        int maost_water = 0;
		
        while(l < r){
  	  		int width = r-l;
  			int height = Math.min(heights[l], heights[r]);
  			most_water = Math.max(most_water, width*height);
			
			// w' * h' < w*h for every h' <= h because w' is less than w for every increment in l
			
			while(l < r && heights[l] <= h){
				l++;
			}
			while(l < r && heights[r] <= h){
				r--;
			}
		}
		return most_water;
	}
```


## Boats to Save People

##### Medium

- You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.

- _Return the minimum number of boats to carry every given person._

Example 1:


Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
Example 2:

Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
Example 3:

Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
 

Constraints:

1 <= people.length <= 5 * 104
1 <= people[i] <= limit <= 3 * 104

##### Approach

 ---

 ## Approach

 1. **Sort the array**

    * Sorting ensures we can always try to pair the lightest person with the heaviest person.
    * If the heaviest cannot pair with the lightest, they cannot pair with anyone else (since everyone else is heavier than the lightest).

 2. **Two-pointer greedy strategy**

    * Start `l` at the lightest (index `0`) and `r` at the heaviest (last index).
    * If `people[l] + people[r] <= limit`, they share a boat → increment `l` and decrement `r`.
    * Otherwise, the heaviest (`r`) goes alone → decrement `r`.
    * Increment `boats` every loop iteration (since we always use one boat per step).

 3. **Why this is optimal**

    * By pairing the heaviest possible person with the lightest, we minimize unused space per boat.
    * Sorting + two pointers guarantees we never leave a lighter person unpaired when they could have fit.
    * This avoids the waste of brute-force pairing and is O(n log n) due to sorting.

 ---

 ## Dry Run Example

 **Input:**

 ```
 people = [3, 2, 2, 1], limit = 3  
 ```

 **Sorted:**

 ```
 [1, 2, 2, 3]  
 ```

 **Steps:**

 * `l = 0 (1)`, `r = 3 (3)` → `1 + 3 > 3` → Boat with `3` alone → `boats = 1`, `r--` → `r = 2`
 * `l = 0 (1)`, `r = 2 (2)` → `1 + 2 <= 3` → Pair them → `boats = 2`, `l++`, `r--` → `l = 1`, `r = 1`
 * `l == r` → One person left (`2`) → Boat alone → `boats = 3`

 **Output:** `3`

 ---
 

##### Code

###### Readable:

```Java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int boats = 0;
        int l = 0;
        int r = people.length-1;
        while(l <= r){
            if( l == r){
                boats++;
            }
            if(people[l]+people[r] == limit){
                l++;
                r--;
                boats++;
            }else{
                r--;
                boats++;
            }
        }
        return boats;
    }
}
```
###### Less Lined
```Java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int boats = 0;
        int l = 0;
        int r = people.length-1;
        while(l <= r){
            if(people[l]+people[r] == limit){
                l++;
            }
            r--;
            boats++;
        }
        return boats;
    }
}
```


## Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

[image](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

Here’s your **GitHub-ready notes** for **Trapping Rain Water**:

---

## Trapping Rain Water – Two Pointer Approach

**Intuition:**
Water trapped at each index depends on the *tallest bar to its left* and the *tallest bar to its right*. The water level is `min(maxLeft, maxRight) - height[i]` if that’s positive.
Instead of precomputing two arrays (O(n) extra space), we can use **two pointers** and keep track of `lmax` and `rmax` on the fly, saving space.

---

### Approach (Two Pointers, O(1) space, O(n) time)

1. **Initialize**:

   * `l` = 0, `r` = n-1 (start and end pointers)
   * `lmax = height[l]`, `rmax = height[r]`
   * `water = 0`

2. **While l <= r**:

   * Update `lmax` and `rmax` with current heights.
   * If `lmax < rmax`:

     * Water trapped at `l` = `lmax - height[l]`
     * Move `l` rightwards.
   * Else:

     * Water trapped at `r` = `rmax - height[r]`
     * Move `r` leftwards.

3. **Why it works**:

   * The smaller of `lmax` and `rmax` is the limiting height for trapping water.
   * By moving the pointer with the smaller bound, we ensure any trapped water is correctly calculated without missing taller bars on the other side.

---

### Example Dry Run

**Input:** `[0,1,0,2,1,0,1,3,2,1,2,1]`

| l   | r   | lmax | rmax | Water Added | Total Water |
| --- | --- | ---- | ---- | ----------- | ----------- |
| 0   | 11  | 0    | 1    | 0           | 0           |
| 1   | 11  | 1    | 1    | 0           | 0           |
| 2   | 11  | 1    | 1    | 1           | 1           |
| 3   | 11  | 2    | 1    | 0           | 1           |
| ... | ... | ...  | ...  | ...         | ...         |
| End |     |      |      |             | **6**       |

**Output:** `6`

---

#### Code

```Java
class Solution {
    public int trap(int[] height) {
        int l = 0;
        int r = height.length-1;
        int lmax = height[l];
        int rmax = height[r];
        int water = 0;
        while(l <= r){
            if(height[l] > lmax ){
                lmax = height[l];
            }
            if(height[r] > rmax){
                rmax = height[r];
            }
            if(lmax < rmax){
                water += (lmax - height[l]);
                l++;
            }else{
                water += (rmax - height[r]);
                r--;
            }
        }
        return water;
    }
}
```



## Contains Duplicate II

---

## **Contains Duplicate II — Anti-Template Thinking**

### **Key Trap I Fell Into**

* I defaulted to the *classic sliding window* mental template:

  * Maintain `l` and `r` pointers.
  * Move both as window slides.
* But here:

  * The window size is **fixed** at `k`.
  * We don’t need to *manually move* `l` in a while loop.
  * `r` naturally moves in a for-loop.
  * `l` can be *computed* as `i - k` when the window gets too big.

This is a common mistake when overfitting “sliding window” from muscle memory rather than adapting to the problem’s constraints.

---

### **Why This Problem Is Different**

* We are **only checking for duplicates in the last k elements**.
* The “window” doesn’t have dynamic size — it’s capped at `k`.
* So instead of:

  ```java
  while (windowTooBig) removeLeft();
  ```

  we do:

  ```java
  if (set.size() > k) set.remove(nums[i - k - 1]);
  ```
* This shifts the window automatically without manually moving `l`.

---

### **Correct Approach (Short)**

1. Iterate `i` from `0` to `n-1`.
2. Before adding `nums[i]`, check if it’s in the set → return `true` if yes.
3. Add `nums[i]` to the set.
4. If set size exceeds `k`, remove `nums[i - k]`.
5. Finish loop → return `false`.

---

### **Mental Checklist to Avoid Template Trap**

* Ask: **Is window size fixed or dynamic?**
* If fixed → no need for while loop / manual pointer shifting.
* If dynamic → use classic `while` shrink pattern.
* Always adapt the skeleton to match constraints — **don’t let muscle memory write code before your brain has adapted the pattern**.

---

If you keep this mental switch in check, you won’t waste time fighting the wrong “pattern” in interviews again.

---

```Java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (set.contains(nums[i])) {
                return true; // found duplicate within window
            }
            
            set.add(nums[i]);
            
            if (set.size() > k) {
                set.remove(nums[i - k]); // shrink from left
            }
        }
        
        return false;
    }
}
```

## Help
Here’s your **micro flowchart** for deciding *fixed vs dynamic sliding window*:

---

**🧠 Sliding Window Pattern Detection**

```
Start
 ↓
Do I need to look at a range of elements?  
 └─ No → Not a sliding window problem, stop.
 ↓ Yes
Is the window size fixed?  
 ├─ Yes → "Fixed Window" pattern  
 │     • Iterate with single for-loop  
 │     • Use index math to remove oldest element when i > k  
 │     • No while loop for shrinking
 │
 └─ No → "Dynamic Window" pattern  
       • Use while loop to shrink until condition is met  
       • l and r move independently  
       • Condition usually depends on sum, count, or distinctness
 ↓
Implement with correct data structure (HashSet, HashMap, Deque, etc.)
```

---

**🔥 Key Mental Shortcuts**

* **Fixed** → `for` loop, remove `nums[i - k]` when needed.
* **Dynamic** → `while` loop to shrink until valid.
* Always **say out loud** in your head:

  > “Am I maintaining a range of exactly `k` elements, or a range that changes with the data?”

---

If you burn this flow into your head, you'll shave **10–30 seconds** off every window problem — and more importantly, avoid wasting brainpower forcing the wrong skeleton.

---

**package these fixed/dynamic patterns** into one quick-reference snippet you can keep in my notes! That way I’ll always see the difference instantly.

