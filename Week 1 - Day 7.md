- [Rotate Array](#rotate)


Here’s a clean set of **Java `long` notes** you can drop into your GitHub DSA notes:

---


## **Java `long` – Key Points for Competitive Programming / DSA**

### **1. Size and Range**

* **64-bit signed integer**
* Range:

  $$
  -2^{63} \ \text{to} \ 2^{63} - 1
  $$

  $$
  -9{,}223{,}372{,}036{,}854{,}775{,}808 \ \text{to} \ 9{,}223{,}372{,}036{,}854{,}775{,}807
  $$
* Much larger than `int` (32-bit, $\pm 2.14 \times 10^9$).

---

### **2. When to Use `long` in DSA**

* **Possible overflow cases**:

  * Adding / subtracting numbers near the `int` limit.
  * Multiplying large values.
  * Summing many large integers.
  * Storing results of combinatorics (`nCr`, factorial, etc.).
* Common in problems with:

  * Large constraints (`10^9` range values).
  * Multi-term sums (e.g., 3Sum, 4Sum).
  * Weighted graphs with large weights.

---

### **3. Casting Rules**

* In Java, **arithmetic happens in the type of the operands**.
* If all operands are `int`, result is `int` (and can overflow).
* **Solution**: Cast **at least one operand** to `long` **before** the operation.

```java
long sum = (long) a + b + c; // Safe
```

* Without casting:

```java
int sum = a + b + c; // Risk of overflow
```

---

### **4. Common Pitfalls**

* **Late casting** still overflows:

```java
long sum = (a + b + c); // BAD if a,b,c are int → overflow before cast
```

* Must cast **before** the operation:

```java
long sum = (long) a + b + c; // GOOD
```

---

### **5. In Comparisons**

* Use casting when comparing to `target`:

```java
if ((long) nums[i] + nums[j] > target) { ... }
```

---

### **6. Space Impact**

* `long` uses 8 bytes vs `int`'s 4 bytes → slightly more memory.
* But for intermediate calculations, it's worth it for correctness.

---

**Rule of Thumb:**

> If numbers can go above $10^9$ and you're adding/multiplying more than two of them — **use `long`**.

---

I can also make you a **quick overflow demo snippet** so you can keep in your repo as proof of why `long` is needed.
Do you want me to include that?

---

# Rotate
Next Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

I have no idea how to do this except for brute force where you you do k%n and write logic to rotate once by storing last el in temp, and move every element one pos up and replace fist with temp

and call this k%n times

can you help me build on top of this logic ? into an optimised approach so i always remeber it

----

Alright — your brute force is **O(n × k)**, which is too slow when `n` and `k` are big.
We can **build on your idea** and get to an **O(n)** solution with **O(1) space**, and you'll never forget it if you see the pattern.

---

## **Key Observation**

Instead of shifting **one step at a time**, realize:

* Rotating right by `k` is the same as taking the **last `k` elements** and putting them in front, while pushing the rest back.
* You don’t need an extra array if you just **reverse sections** of the array.

---

## **The Reverse Trick**

Think of `"rotate"` as **reversing segments**:

**Steps:**

1. **Reverse the whole array**
2. **Reverse the first `k` elements**
3. **Reverse the remaining `n - k` elements**

---

### **Why this works**

Example:
`nums = [1, 2, 3, 4, 5, 6, 7], k = 3`

1. Reverse whole array → `[7, 6, 5, 4, 3, 2, 1]`
2. Reverse first `k` (`3`) elements → `[5, 6, 7, 4, 3, 2, 1]`
3. Reverse rest (`n - k = 4` elements) → `[5, 6, 7, 1, 2, 3, 4]`

Boom — rotated in **O(n)** time, **O(1)** extra space.

---

### **Code Template (Java)**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n; // handle k > n

        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++;
            right--;
        }
    }
}
```

---

### **How to Always Remember**

* Your brute force is "shift one by one".
* Upgrade mental model: **"Rotate = Reverse twice… actually thrice"**

  1. Reverse all.
  2. Reverse first `k`.
  3. Reverse the rest.
* If stuck, visualize with string rotation — the logic is identical.

---

If you want, I can give you a **memory hook diagram** so this reverse method sticks in your head permanently. That way in an interview you can recall it instantly.


