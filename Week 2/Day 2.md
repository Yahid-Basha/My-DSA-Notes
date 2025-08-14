- [Longest Substring Without Repeating Characters](#Longest-Substring-Without-Repeating-Characters)
- [Buy And Sell Stock I](#Buy-And-Sell-Stock-I)
- [Longest Repeating Character Replacement](#Longest-Repeating-Character-Replacement)
- [Permutation in String](#Permutation-in-String)

## Longest Substring Without Repeating Characters

**Problem**
Given a string `s`, find the length of the longest substring without repeating characters.

---

### **Approach: Sliding Window + HashSet**

We maintain a moving window `[l, r]` that always contains unique characters:

1. **Expand right** (`r`) while the character at `r` is not in the set.

   * Add it to the set.
   * Update `maxLen` as `r - l + 1`.

2. **Shrink left** (`l`) if we hit a duplicate:

   * Remove `s.charAt(l)` from the set.
   * Move `l` forward until the duplicate is gone.

3. Repeat until `r` reaches the end.

---

### **Why This Works**

* The HashSet ensures O(1) duplicate checks.
* Every character is added and removed at most once → O(n) time complexity.
* The window always holds only unique characters, so its length is always a candidate for the maximum.

---

### **Pseudocode**

```java
int l = 0, r = 0, maxLen = 0;
Set<Character> set = new HashSet<>();

while (r < s.length()) {
    if (!set.contains(s.charAt(r))) {
        set.add(s.charAt(r));
        maxLen = Math.max(maxLen, r - l + 1);
        r++;
    } else {
        set.remove(s.charAt(l));
        l++;
    }
}
return maxLen;
```

---

### **Dry Run Example**

`s = "abcabcbb"`

| l                                   | r | set                                      | maxLen |
| ----------------------------------- | - | ---------------------------------------- | ------ |
| 0                                   | 0 | {a}                                      | 1      |
| 0                                   | 1 | {a,b}                                    | 2      |
| 0                                   | 2 | {a,b,c}                                  | 3      |
| 0                                   | 3 | duplicate 'a' → remove 'a' from set, l=1 |        |
| 1                                   | 3 | {b,c,a}                                  | 3      |
| ... and so on until r = s.length(). |   |                                          |        |

Final `maxLen` = **3**.

---

**Complexity**

* **Time:** O(n)
* **Space:** O(min(n, charset size)) — O(n) worst-case.

**Key Pitfalls Avoided**

* Use `while (r < s.length())` not `while (l < r && r < s.length())`.
* Length is `r - l + 1`, not `r - l`.
* Initialize `maxLen = 0` to handle empty strings.

---

Alright, here’s your **Buy and Sell Stock I** cheat sheet — short, crisp, and revision-ready:

---
## **Buy And Sell Stock I**
### **Buy and Sell Stock I (Max Profit with One Transaction)**

**Problem:**
Given prices of a stock on each day, find the maximum profit you can achieve from **one buy and one sell** (buy before sell).

---

### **Core Idea**

* Track the **minimum price** seen so far.
* For each day, calculate `profit = current_price - min_price_so_far`.
* Keep track of the **maximum profit** seen so far.

---

### **Algorithm Steps**

1. Initialize:

   * `min_price = prices[0]`
   * `max_profit = 0`
2. Loop through prices:

   * Update `min_price` if current price is lower.
   * Calculate profit: `prices[i] - min_price`
   * Update `max_profit` if profit is higher.
3. Return `max_profit`.

---

### **Time & Space**

* **Time:** O(n) — single pass.
* **Space:** O(1) — just a few variables.

---

### **Key Insights**

* Always **buy at the lowest point so far**, sell at the current point.
* If prices are always decreasing → profit = 0 (no transaction).
* It’s basically finding the **biggest rise after a drop**.

---

### **Java Notes**

```java
int minPrice = prices[0];
int maxProfit = 0;
for (int price : prices) {
    if (price < minPrice) minPrice = price;
    else maxProfit = Math.max(maxProfit, price - minPrice);
}
return maxProfit;
```

---

## **Longest Repeating Character Replacement**

---

## **Problem Summary**

You are given a string `s` and an integer `k`. You can replace at most `k` characters in the string.
Find the length of the longest substring that can be turned into **all the same letter** by replacing at most `k` characters.

---

## **Core Idea**

We want the **longest window** where:

```
window_length - max_frequency_in_window <= k
```

* `window_length` = size of the current sliding window
* `max_frequency_in_window` = the count of the most common character in the current window

If this condition is true, it means we can make the entire window the same character by changing at most `k` letters.

---

## **Approach**

**Type**: Sliding Window + Frequency Map

1. **Initialize**

   * A map/array to store character frequencies (`count`).
   * `maxFreq` to store **highest character count in the window**.
   * Two pointers: `left = 0`, iterate `right` over string.
   * `maxLength` to track the best result.

2. **Expand Window**

   * Add `s[right]` to frequency map.
   * Update `maxFreq` if needed.

3. **Shrink Window if Invalid**

   * If `(window_length - maxFreq) > k`,
     move `left` forward and update counts.

4. **Track Result**

   * At each step, `maxLength = max(maxLength, window_length)`.

---

## **Why `maxFreq` Works**

* We don’t care *which* character forms the longest valid substring — we just keep the count of the **most frequent one in the current window**.
* If `(window_length - maxFreq) <= k`, then the rest of the characters can be changed to match the most frequent one.

---

## **Complexity**

* **Time**: O(n) — each character visited at most twice.
* **Space**: O(1) — frequency array size fixed (26 letters).

---

## **Pseudo-code**

```
count = array[26] = {0}
maxFreq = 0
left = 0
maxLength = 0

for right in range(len(s)):
    count[s[right]] += 1
    maxFreq = max(maxFreq, count[s[right]])

    while (window_size - maxFreq) > k:
        count[s[left]] -= 1
        left += 1

    maxLength = max(maxLength, window_size)

return maxLength
```

---

## **Example Walkthrough**

`s = "AABABBA", k = 1`

| right            | char | count   | maxFreq | window\_size | Changes Needed | Valid? | maxLength |
| ---------------- | ---- | ------- | ------- | ------------ | -------------- | ------ | --------- |
| 0                | A    | A:1     | 1       | 1            | 0              | ✅      | 1         |
| 1                | A    | A:2     | 2       | 2            | 0              | ✅      | 2         |
| 2                | B    | A:2,B:1 | 2       | 3            | 1              | ✅      | 3         |
| 3                | A    | A:3,B:1 | 3       | 4            | 1              | ✅      | 4         |
| 4                | B    | A:3,B:2 | 3       | 5            | 2              | ❌      | —         |
| shrink left to 1 |      | A:2,B:2 | 3       | 4            | 1              | ✅      | 4         |
| ...              |      |         |         |              |                |        |           |

---

## **Key Pitfalls**

* ❌ Recomputing `maxFreq` every time window shrinks → unnecessary (keep track as max seen so far).
* ❌ Forgetting that `maxFreq` might be stale — but this is fine because an overestimated `maxFreq` only delays shrinking, never breaks correctness.

---
## **Permutation in String**

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if(s1.length() > s2.length()) return false;

        int l  = 0;
        int r = s1.length()-1;
        int [] f1 = freq(s1);
        while(r < s2.length()){
            String s = s2.substring(l,r+1);
            int [] f2 = freq(s);
            if(Arrays.equals(f1,f2)){
                return true;
            }
            l++;
            r++;
        }
        return false;
    }
    public int [] freq(String s){
        int [] fre = new int[26];
        for(int i = 0; i < s.length(); i++){
            fre[s.charAt(i)-'a']++;
        }
        return fre;
    }

}
```

