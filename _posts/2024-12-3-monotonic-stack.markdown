---
layout: post
title:  "monotonic stack"
date:   2024-12-3
description: Notes on monotonic stack
---

Notes, foundation basically means that ideas from task persist among many other tasks and kinda crucual'ish u know...

#### [foundation] 🫑 **[496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i)**

NGE = next greater element

Consider some index i, then its NGE is first value to the right that is bigger. but how do we get that value? well just go form right to left and update maximum value u have seen so far. if you have seen value that is bigger then last value seen then it means that any value to the left will first ecounter this new value rather then next value. if you encounter value less then current value, the point that it actually might be NGE for some other value so add this to stack. so add this to the stack, then when we see value that is bigger then our max it might be the case that we have seen value before and we will pop from stack untill we find bigger value or end up with empty stack & in the end do not forget to add this value to the stack also! 

#### 🫑 **[503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii)**

pretty much same idea as in 496 but just go 2 times over array

#### 🌱 **[1762. Buildings With an Ocean View](https://leetcode.com/problems/buildings-with-an-ocean-view)**

u have to solve it in 2 min as warm up

#### 🫑 **[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures)**

basically we are asked for distance to NGE for each element. well, compute NGE, then compute distance or compute it in parallel

#### 🍑 **[962. Maximum Width Ramp](https://leetcode.com/problems/maximum-width-ramp)**

...

#### 🌶️ **[1504. Count Submatrices With All Ones](https://leetcode.com/problems/count-submatrices-with-all-ones)**

Start to think about how to enuerate all possible submatrices. well you can say that each submatrix ends on some row, has some height and starts in column l and ends in column r >= l

Now it makes sence to enumerate all matrices

```python
rows = len(mat)
columns = len(mat[0])
for row in range(rows):
    for left_column in range(columns):
        for right_column in range(left_column + 1, columns + 1):
            for height in range(1, row + 2):

            	# n^2
            	ok = True
                for C in mat[row + 1 - height:row + 1]:
                    if sum(C[left_column:right_column]) != right_column - left_column:
                        ok = False
                if ok:
                    s += 1

```

total complexity n^4 * n^2 = n^6 that is kinda sucks

but notice that we can precompute maximum number of ones from bottom to top for each row for each col like this in n^2

```python
counts = [[0 for __ in range(columns)] for _ in range(rows)]
for column in range(columns):
    counts[0][column] = mat[0][column]
for row in range(1, rows):
    for column in range(columns):
        if mat[row][column] == 0:
            counts[row][column] = 0
        else:
            counts[row][column] = counts[row - 1][column] + 1
```

and for every row which can be end of some rectange, select some left and right bounds that can be width of rectangle and then instead of trying all different possible heights we will do following. but first notice one thing. to select rectangle as valid all cells must be one. which means that for all heights must be one. but if there is some height that has lower number of ones then another we will be able only to built up heights up to the smaller one, since after then some columns wil contain zeros. so to calculate number of valid heights just get minimum in our precomputer array that says what is maximum consecutive ones up to that row

```python
for row in range(rows):
    for left_column in range(columns):
        for right_column in range(left_column + 1, columns + 1):
            # find max height such that all elements are ones
            # then add height to answer!
            # [1 0] 1
            # [1 1] 0
            # height is 1
            # what if i knew number of ones for each pos like
            # 2 1 => then I would just select minimum
            s += min(counts[row][left_column:right_column])

```

this reduced complecity to n^2+ n^3 = n^3 and much better now

Now if we think about what we do we will understand that for each row we enumerate all possible subarrays and for each subarray we find minimum height. Again, for each subarray we get minimum value and then sum it. Familiar? [907. Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums) which is done in linear time! sooo just copy paste method from that task

```python
def sumSubarrayMins(self, arr: List[int]) -> int:
    stack = []
    stack.append((0, arr[0]))
    ans = arr[0]
    for i in range(1, len(arr)):
        while len(stack) > 0 and arr[i] <= arr[stack[-1][0]]:
            stack.pop()
        
        if len(stack) > 0:
            r = ((i + 1) - (stack[-1][0] + 1)) * arr[i]
            ans += stack[-1][1] + r
            stack.append((i, stack[-1][1] + r))
        else:
            r = (i + 1) * arr[i]
            ans += r
            stack.append((i, r))
    
    return ans
```

and then use it


```python
rows = len(mat)
columns = len(mat[0])

counts = [[0 for __ in range(columns)] for _ in range(rows)]
for column in range(columns):
    counts[0][column] = mat[0][column]
for row in range(1, rows):
    for column in range(columns):
        if mat[row][column] == 0:
            counts[row][column] = 0
        else:
            counts[row][column] = counts[row - 1][column] + 1

for row in range(rows):
    s += self.sumSubarrayMins(counts[row])
return s
```

and now it is n^2 which is so cool! it is just as matrix size mn
