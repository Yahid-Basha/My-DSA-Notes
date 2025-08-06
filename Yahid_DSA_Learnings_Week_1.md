
# 📘 Yahid’s DSA Daily Learnings — Week 1

---

## ✅ 1. **Boyer-Moore Voting Algorithm** — *Think Like a Fighter*

```java
public int majorityElement(int[] nums) {
    int count = 0, candidate = 0;

    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += (num == candidate) ? 1 : -1;
    }

    return candidate;
}
```

### 🔍 Logic:
- **Candidate**: Potential majority element.
- **Count**: Balance scale of how dominant the candidate is.

### 💡 Key Insight:
- Same number → candidate gets stronger (`+1`)
- Different number → candidate loses ground (`-1`)
- Count zero → reset candidate

---

## ✅ 2. **Java HashMap Iteration**

```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "A");
map.put(2, "B");

// Keys
for (Integer key : map.keySet()) {
    System.out.println(key);
}

// Values
for (String value : map.values()) {
    System.out.println(value);
}

// Key-Value Pairs
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

---

## ✅ 3. **Java `Integer.valueOf()` — Not Cheating, Just Smart**

```java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 2));

// ⚠️ Wrong — removes index 2
list.remove(2); 

// ✅ Right — removes value 2
list.remove(Integer.valueOf(2)); 
```

### 🔹 Why It Matters:
- `list.remove(int)` → removes **index**
- `list.remove(Object)` → removes **value**
- `Integer.valueOf(2)` forces compiler to pick the correct overload.

---

## ✅ 4. **Java Substring — Slicing Strings**

```java
String str = "Yahid";

// Substring from index 1 to 4 (exclusive of 4)
String part = str.substring(1, 4);  // "ahi"
```

---

## ✅ 5. **Arrays.toString() — Print Arrays in Java**

```java
int[] arr = {1, 2, 3};
System.out.println(Arrays.toString(arr)); // [1, 2, 3]
```

---

## ✅ 6. **String to Character Array**

```java
String str = "code";
char[] chars = str.toCharArray();  // ['c', 'o', 'd', 'e']
```

---

## ✅ 7. **Sort Array in Java**

```java
int[] arr = {3, 1, 4, 2};
Arrays.sort(arr);  // arr becomes [1, 2, 3, 4]
```
