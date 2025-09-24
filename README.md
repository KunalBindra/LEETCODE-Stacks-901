# LEETCODE-Stacks-901
---

### Problem Recap

We want a class where:

* You call `next(price)`.
* It returns the **span** = number of consecutive days before today (including today) where price was **less than or equal to today’s price**.

So it’s like: "How many days in a row the stock was not more expensive than today?"

We’ll dry run it with this sequence of prices:

```
[100, 80, 60, 70, 60, 75, 85]
```

---

### Java Code in Simple Form

```java
class StockSpanner {
    private Stack<int[]> st;  // stack of [price, span]

    public StockSpanner() {
        st = new Stack<>();
    }

    public int next(int price) {
        int span = 1;

        while (!st.isEmpty() && st.peek()[0] <= price) {
            span += st.pop()[1]; // add previous span and remove it
        }

        st.push(new int[]{price, span}); // push current price and span
        return span;
    }
}
```

---

### Step-by-Step Dry Run

#### Start:

`st = []` (empty stack)

---

#### 1. Call `next(100)`

* `span = 1`
* Stack is empty → skip while loop.
* Push `[100, 1]`
* Return `1`

👉 **Answer = 1**
👉 `st = [[100, 1]]`

---

#### 2. Call `next(80)`

* `span = 1`
* Top = `[100, 1]`. Since `100 > 80`, skip while loop.
* Push `[80, 1]`
* Return `1`

👉 **Answer = 1**
👉 `st = [[100, 1], [80, 1]]`

---

#### 3. Call `next(60)`

* `span = 1`
* Top = `[80, 1]`. Since `80 > 60`, skip while loop.
* Push `[60, 1]`
* Return `1`

👉 **Answer = 1**
👉 `st = [[100, 1], [80, 1], [60, 1]]`

---

#### 4. Call `next(70)`

* `span = 1`
* Top = `[60, 1]` → `60 <= 70` → Pop it, add span.
  Now `span = 1 + 1 = 2`.
* Next top = `[80, 1]` → `80 > 70`, stop loop.
* Push `[70, 2]`
* Return `2`

👉 **Answer = 2**
👉 `st = [[100, 1], [80, 1], [70, 2]]`

---

#### 5. Call `next(60)`

* `span = 1`
* Top = `[70, 2]`. Since `70 > 60`, skip while loop.
* Push `[60, 1]`
* Return `1`

👉 **Answer = 1**
👉 `st = [[100, 1], [80, 1], [70, 2], [60, 1]]`

---

#### 6. Call `next(75)`

* `span = 1`
* Top = `[60, 1]` → `60 <= 75`, pop it. `span = 1 + 1 = 2`
* Top = `[70, 2]` → `70 <= 75`, pop it. `span = 2 + 2 = 4`
* Top = `[80, 1]` → `80 > 75`, stop loop.
* Push `[75, 4]`
* Return `4`

👉 **Answer = 4**
👉 `st = [[100, 1], [80, 1], [75, 4]]`

---

#### 7. Call `next(85)`

* `span = 1`
* Top = `[75, 4]` → `75 <= 85`, pop. `span = 1 + 4 = 5`
* Top = `[80, 1]` → `80 <= 85`, pop. `span = 5 + 1 = 6`
* Top = `[100, 1]` → `100 > 85`, stop.
* Push `[85, 6]`
* Return `6`

👉 **Answer = 6**
👉 `st = [[100, 1], [85, 6]]`

---

### Final Answers

For input `[100, 80, 60, 70, 60, 75, 85]`, the outputs are:

```
[1, 1, 1, 2, 1, 4, 6]
```

---

🔥 The stack stores "price and its span".
🔥 Each price only gets pushed and popped once → efficient O(n).

---
