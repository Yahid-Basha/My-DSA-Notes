---

# ⏱ Time Complexity vs Constraints — Quick Reference

## The Rule of Thumb

* **Tiny data (n ≤ 100)** → O(n²) or even O(n³) may work — brute force is often safe.
* **Medium data (n ≤ 10⁵)** → Need O(n) or O(n log n). O(n²) will **likely TLE**. ([Medium][1], [GeeksforGeeks][2])
* **Large data (n \~10⁶+)** → Only O(n) or better. Even O(n log n) may struggle depending on constants ([GeeksforGeeks][2]).

---

## Why Constraints Matter

* They **limit the allowed operations** — you can't just "optimize later".
* They signal which algorithmic patterns work (e.g., sorting, prefix sums, two-pointers) ([AlgoCademy][3]).
* They guide your **time-space tradeoffs** — helps avoid TLEs and MLEs ([AlgoCademy][3]).

---

## Interview Strategy: Constraints → Approach

1. **Read constraints carefully**.
2. **Estimate complexity** (e.g., 10⁵ elements → O(n²) is a no-go).
3. **Pick approach**:

   * ⏳ Start with brute force (to validate correctness)
   * ⚙️ Then optimize if needed — hash maps, prefix sums, sliding windows, heaps, DP ([Medium][4]).
4. **Mention time complexity out loud** — shows maturity ([Level Up Coding][5]).

---

## Common Threshold Table

| n (Input Size) | Feasible Time Complexities      | Flags                |
| -------------- | ------------------------------- | -------------------- |
| n ≤ 100        | O(n²), O(n³)                    | Brute force ok       |
| n ≤ 10⁵–10⁶    | O(n log n), O(n), maybe O(n √n) | Need optimization    |
| n ≥ 10⁶        | O(n) or better                  | Strict time ceilings |

---

## Quick Tips for Interviews

* Say: **“First I'll write a brute force for correctness — then optimize if time allows.”** ([Medium][4], [Reddit][6])
* If constraints are missing, **ask clarifying questions** like: *“What’s the upper bound on n?”* ([Reddit][7])
* When listing approaches, **treat constraints like a filter**, not a blocker.

---

### TL;DR

Use constraints as your *deployment map*, not just a detail on the problem statement.
They guide the **choose/optimize code route**, turning guesswork into calculated strategy.

---

Let me know when you’re ready to wrap Day 3 or want the PDF of these notes.

[1]: https://medium.com/%40AlexanderObregon/how-to-analyze-patterns-and-choose-the-right-algorithm-for-dsa-problems-ff8d2055146e?utm_source=chatgpt.com "How to Analyze Patterns and Choose the Right Algorithm for DSA ..."
[2]: https://www.geeksforgeeks.org/dsa/knowing-the-complexity-in-competitive-programming/?utm_source=chatgpt.com "Knowing the complexity in competitive programming - GeeksforGeeks"
[3]: https://algocademy.com/blog/the-importance-of-reading-input-constraints-carefully-in-coding-challenges/?utm_source=chatgpt.com "The Importance of Reading Input Constraints Carefully in Coding ..."
[4]: https://medium.com/%40logic3110/mastering-problem-solving-the-underrated-power-of-brute-force-algorithms-c8cadb980a52?utm_source=chatgpt.com "Mastering Problem-Solving: The Underrated Power of Brute Force ..."
[5]: https://levelup.gitconnected.com/how-to-not-fail-a-coding-interview-an-insiders-guide-ffa893f3a840?utm_source=chatgpt.com "How to Not Fail a Coding Interview: An Insider's Guide"
[6]: https://www.reddit.com/r/cscareerquestions/comments/xwso1t/is_it_better_to_give_a_working_bruteforce/?utm_source=chatgpt.com "Is It Better To Give A Working Brute-Force Solution Or A Non ... - Reddit"
[7]: https://www.reddit.com/r/leetcode/comments/1fj4z5b/every_problem_has_a_cheat_code/?utm_source=chatgpt.com "Every Problem Has a Cheat Code : r/leetcode - Reddit"
