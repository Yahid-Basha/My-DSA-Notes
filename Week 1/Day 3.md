### ✅ **DSA Notes – Day 3**

#### 📌 **Stock Buy/Sell II**

* **Concept:** Greedy approach — accumulate all increasing slopes (`profit += prices[i] - prices[i-1]` if positive).
* **Insight:** You can buy/sell on every profitable rise — no need to track valleys/peaks strictly.
* **Pattern:** No need to track global max; sum of all positive diffs = max profit.

#### ⚔️ **Boyer-Moore Voting Algorithm – Extended (n/3 majority)**

* **Concept:** At most two elements can occur more than ⌊n/3⌋ times.
* **Tool:** Modified Boyer-Moore:

  * Track **2 candidates** and their counts.
  * 1st pass: find potential candidates.
  * 2nd pass: verify actual frequencies.

#### 💡 **Implementation Edge Cases**

* **Validation pass** is **mandatory** — candidates from first pass may not actually cross threshold.
* Ensure not to **double-count** candidates (i.e., `if (candidate1 != candidate2)` check).

