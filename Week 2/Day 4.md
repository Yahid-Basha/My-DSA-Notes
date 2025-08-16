## Index
- [Baseball Game](#baseball-game)
- [Valid Parentheses](#valid-parentheses)
- [Implement Stack Using Queues](#implement-stack-using-queues)
- [Implement Queue using Stacks](#implement-queue-using-stacks)

## Baseball Game   	
Postfix lik

## Valid Parentheses 
Seperating data from logic is essential for scalability, if else ladders are not scalable
find the false cases early on to not check further
for eg: adding a closing bracket at start ')'
        if the stack.top and curr doesn't match (cancel out each other '(' & ']' )

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        Map<Character, Character> map = new HashMap<>();
        map.put('(', ')');
        map.put('{', '}');
        map.put('[', ']');
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(map.containsKey(c)){
                stack.push(c);
            }else{
                if(stack.isEmpty()){
                    return false;
                }
                char curr = stack.peek();
                if(c == map.get(curr)){
                    stack.pop();
                }else{
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
}
```
## Implement Queue using Stacks   

-----

## DSA Notes: Implement Queue using Stacks (The Amortized Approach)

### The Problem

Implement a First-In-First-Out (FIFO) Queue using only two Last-In-First-Out (LIFO) Stacks. The core challenge is reversing the LIFO behavior into FIFO.

### The Brute-Force (Inefficient) Approach

The first instinct is to use one stack (`main`) for `push` operations. When a `pop` or `peek` is needed, the logic is:

1.  Pour all elements from `main` into a helper stack (`aux`), which reverses the order.
2.  Pop/Peek the top of `aux` (which is the front of the queue).
3.  Pour all elements **back** from `aux` to `main` to restore the state.

**Analysis**: This works, but it's brutally inefficient. Every `pop` or `peek` becomes an $O(n)$ operation, involving a massive number of unnecessary data movements.

-----

### The Breakthrough: From Brute-Force to Amortized Efficiency

The conversation revealed the flaw in the brute-force approach with one question:

> **Why are you in such a hurry to move the elements back?**

Once the elements are poured from the `input` stack to the `output` stack, the `output` stack is already in the perfect FIFO order. Moving them back is wasted work, only to be undone on the next `pop`.

**The Critical Insight**: Be "lazy." Don't do the expensive reversal work until the moment you absolutely need it. Leave the elements in the `output` stack.

-----

### The Amortized O(1) Approach

This gives the two stacks permanent, specialized roles: `input` and `output`.

  * **`push(x)`**: New elements are **always** pushed onto the `input` stack. This is a simple $O(1)$ operation.
  * **`pop()` / `peek()`**:
    1.  First, check if the `output` stack is empty.
    2.  If **`output` is not empty**, its top element is the front of the queue. Simply pop/peek from it. This is the **fast path**, an $O(1)$ operation.
    3.  If **`output` is empty**, this is the only time we do heavy lifting. We perform a one-time transfer of **all** elements from `input` to `output`. This single operation reverses the order and "restocks" the `output` stack. Then, we pop/peek from `output`.

**Complexity Analysis**: This is called **Amortized O(1) time complexity**. While a single `pop` operation can occasionally cost $O(n)$ (when the transfer happens), the cost of that expensive operation is spread out over many cheap, $O(1)$ `pop` operations. Each element is only pushed onto `input` once, transferred to `output` once, and popped from `output` once in its entire lifetime.

### Final Java Code

```java
class MyQueue {

    Stack<Integer> input;
    Stack<Integer> output;

    public MyQueue() {
        input = new Stack<>();
        output = new Stack<>();
    }
    
    public void push(int x) {
        input.push(x);
    }
    
    public int pop() {
        // Transfer from input to output only when output is empty
        if (output.isEmpty()) {
            while (!input.isEmpty()) {
                output.push(input.pop());
            }
        }
        return output.pop();
    }
    
    public int peek() {
        // Same logic as pop
        if (output.isEmpty()) {
            while (!input.isEmpty()) {
                output.push(input.pop());
            }
        }
        return output.peek();
    }
    
    public boolean empty() {
        return input.isEmpty() && output.isEmpty();
    }
}
```

-----

### The Bottom Line

> **Don't do repetitive work. Be lazy. By performing the expensive reversal operation only when absolutely necessary, you can turn a slow O(n) algorithm into a highly efficient Amortized O(1) one.**

## Implement Stack Using Queues 

Queues preserve order and hence you can't use two Queues to re shuffle it to organise it like stack
hence you have no other option but to trade between a faster pop or push

### Approach 1
faster push 
simple add to the Que

slower pop
poll from Queue and offer to Queue again, n-1 times and the last element you inserted comes to first after n-1 iterations of poll and offer


### Approach 2
slow push
add an item and then rotate until it comes to front
eg 
[1] -> [1,2] -> [2,1] -> [2,1,3] -> [3,2,1] -> add & rotate -> add & rotate (basically reverses the Queue)

faster pop
simple poll, cause Queue was reversed in push, you can do as mnay pops as needed cause it is entirely reversed in each pop not just one rotation 

### Rotations and Reverse are siblings for eg. you could solve rotate array n times problem by reversing two parts of same array



