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
