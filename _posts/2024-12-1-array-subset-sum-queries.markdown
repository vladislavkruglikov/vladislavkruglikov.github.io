---
layout: post
title:  "dp bits etc"
date:   2024-11-30
description: What is bitmask dp and what approaches ared used to solve such kind of problems. Also some useful algorithms such as efficient submask enumeration and bit operations. Examples of few leetcode problems and their solutions with proofs and code examples
---

Any indication of a bit position is counted from the right and advancing left. For example in $$10110$$ bit in position $$0$$ is not set and bit in positon $$1$$ is set the again set then not set and then last bit is set. I have written here stuff that seems to be enough to solve pretty hard problems

#### **Least significant bit LSB**

149 in decimal
1001010(1 <- LSB) in binary

In computing, the least significant bit (LSb) is the bit position in a binary integer representing the binary 1s place of the integer.

The LSb is sometimes referred to as the low-order bit or right-most bit, due to the convention in positional notation of writing less significant digits further to the right

#### **Most significant bit MSB**

149 in decimal
(1 <- MSB)0010101 in binary

Similarly, the most significant bit (MSb) represents the highest-order place of the binary integer

The MSB is similarly referred to as the high-order bit or left-most bit

#### **Left bit shift**

Bit left shift means that we shift all bits to the left by some number of times. For example

{% highlight python %}
number = 22           
print(f"{number:b}")       #   10110
print(f"{number << 1:b}")  #  101100
print(f"{number << 2:b}")  # 1011000
{% endhighlight %}

#### **Right bit shift Right bit shift is the same**

{% highlight python %}
number = 22           
print(f"{number:b}")       # 10110
print(f"{number >> 1:b}")  #  1011
print(f"{number >> 2:b}")  #   101
{% endhighlight %}

#### **Bits intersections**

To intersect bits in bit wise manner we use logical and operation. After the operation only bits that are set in both numbers will remain. All other bits will become unset

{% highlight python %}
number = 22
print(f"{number:b}")          # 10110
other = 26
print(f"{other:b}")           # 11010
print(f"{number & other:b}")  # 10010
{% endhighlight %}

#### **Bits union**

The same works with logical or

{% highlight python %}
number = 22
print(f"{number:b}")          # 10110
other = 26
print(f"{other:b}")           # 11010
print(f"{number | other:b}")  # 11110
{% endhighlight %}

#### **Check that bit is set**

Check that bit in position $$i$$ is set. For this reason we can create number with single bit set on position $$i$$ and then intersect it with original number. Then if $$i$$ position contained set bit in original number it will become set and otherwise not set

{% highlight python %}
number = 22
i = 1
print(f"{number:b}")             # 10110
print(f"{1 << i:b}")             #   100
print(f"{(1 << i) & number:b}")  #   100
{% endhighlight %}

And example where bit is unset

{% highlight python %}
number = 22
i = 3
print(f"{number:b}")             # 10110
print(f"{1 << i:b}")             #  1000
print(f"{(1 << i) & number:b}")  #     0
{% endhighlight %}

So basically we can check it that way

{% highlight python %}
number = 22
i = 1

if (1 << i) & number:
    print("Bit Is Set")
else:
    print("Bit Is Not Set")
{% endhighlight %}

Check that bit $$i$$ is set

{% highlight python %}
a = 21
print((1 << i) & a)
{% endhighlight %}

#### **Set a bit**

Suppose we have some bits and we want to mark some bit is set


{% highlight python %}
number = 20
i = 3
print(f"{number:b}")    # 10100
print(f"{(1 << i):b}")  #  1000
number |= (1 << i)
print(f"{number:b}")    # 11100
{% endhighlight %}

#### **Unset a bit**

Suppose we have some bits and we want to mark some bit to unset

{% highlight python %}
number = 20
print(f"{number:b}")      # 10100
not_number = ~number
print(f"{not_number:b}")  # -10101
{% endhighlight %}

This is not what we expected. This is because Python integers are unlimited so they cannot use two complement internally. So python uses custom system. This is because we did not alocate sort of 4 byte number or 32 bits or any other pre defined size. So if I give you number 1010 what is that in decimal? If it is a 4 bit number it is -6. If it is 5 bit or greater it is 10. In other programming languages this is not a problem, all datatypes have a very strictly defined size

https://www.reddit.com/r/learnpython/comments/12i38gt/is_there_a_way_to_see_what_the_real_binary_number/

https://stackoverflow.com/a/64405631/11678336

This is also a proof why this gives infinitely many ones

{% highlight python %}
number = 20
not_number = ~number
while not_number != 0:
  print(not_number % 2, end="")
  not_number >> 1
{% endhighlight %}

But anyway despite the fact that python prints it in the custom way under the hood it allows to do operations in normal way

So it works

{% highlight python %}
number = 20
i = 2
print(f"{(1 << i):b}")   #   100
print(f"{number:b}")     # 10100
number &= ~(1 << i)
print(f"{number:b}")     # 10000
{% endhighlight %}

#### **Inverse bits**

{% highlight python %}
number = 20
not_number = ~number
{% endhighlight %}

Or if we know how many bits we have we can employ substraction from $$2^n - 1$$
https://en.wikipedia.org/wiki/Two%27s_complement

{% highlight python %}
n = 5
number = 20
print(f"{number:b}")              # 10100
not_number = 2 ** n - 1 - number
print(f"{not_number:b}")          #  1011
{% endhighlight %}

It is dangerous to use ~ in dp bitmask. Suppose we have 8 elements in array
and we have used elements 0 4 and 5 which is represented as 110001 which is also 
the same as 0110001 and 00110001 and 000110001 and so on. Now consider 110001 and 
apply ~ to it. The python would return 1...1 001110 but since python thinks of infintetely many ones in the left it is going to behave weird on problem of finding all of the submasks

{% highlight python %}
used = 20  # 10100
print(f"{used:b}")
not_used = ~used
not_used_variant = not_used
while not_used_variant > 0:
  print(not_used_variant)
  not_used_variant = (not_used_variant - 1) & not_used
{% endhighlight %}

This algorithm does not print anything! Weird in the way that we suppose that inverse of
10100 is 01011 thus we have 3 ones and 2 ** 3 - 1 submasks let us now rewrite it

{% highlight python %}
used = 20  # 10100
print(f"{used:b}")
not_used = 2 ** 5 - 1 - used
not_used_variant = not_used
while not_used_variant > 0:
  print(not_used_variant)
  not_used_variant = (not_used_variant - 1) & not_used
{% endhighlight %}

and we go ours 2 ** 3 - 1 submasks! More on this algo in the following topics, for now just short interesting behaviour that shows that python's ~ is a weird and dangerous thing

#### **Sum Of All Subsets**

Suppose we have $$n$$ elements and therefore $$2^n$$ combinations of thoose elements. Basically for each element we decide to take it or not into the combination. Suppose that we have array of $$n$$ elements with different numbers and we recieve query to get sum of elements corresponsing to given indices where each index is unique


To enumerate all possible combinations we can iterate from $$0$$ to $$2^n$$ which means we will try every possible combination starting from $$00000000$$ to the $$11111111$$


{% highlight python %}
S = [0 for _ in range(1 << n)]
for c in range(1 << n):
  for i in range(n):
    if (1 << i) & c:
      S[c] += A[i]
{% endhighlight %}

https://codeforces.com/blog/entry/18169 here is impl aswell

(interesting fact, todo decide where to place) python's dict take up much more memory that lust, few tasks failed on memory limit of using cache in dp bitmask https://stackoverflow.com/a/60583658/11678336

#### **Dumb way to find all submasks**

Suppose we have mask 10100 and all of its submasks are 10100, 10000, 00100=100 how do we find them?

{% highlight python %}
mask = 20  # 10100
print(f"{mask:b}")
for submask_candidate in range(1, 2 ** 5): # skip empty element since 0000 is not a submask
  if submask_candidate | mask == mask: # submask_candidate did not add any extra bits
    print(f"{submask_candidate:b}")
{% endhighlight %}

#### **Smarter way to find all submasks via Submask enumeration**

Given a bitmask $$m$$ you want to efficiently iterate through all of its submasks that is masks $$s$$ in which only bits that were included in mask $$m$$ are set

Simple thus not efficient solution would be to enumerate all possible masks and check if particular mask is indeed a submask. Since submasks are such masks that have same set bits it does not make sence to consider masks with set bits on the left side from leftmost set bit which means we whould enumerate starting from number m. For example consider doing in in desc ordering. I will first take some number that does not have some interesting properties in the way that in contains pretty equal number of set and not set bits. For example 20

{% highlight python %}
m = 20
print(f"Initial mask {m=}, {m=:b}")
s = m
while s > 0:
  print(f"Start to check {s=}, {s=:b}")
  if s & m == s:
    print(f"Found submask {s=}, {s=:b}")
  s -= 1
{% endhighlight %}

That will produce

{% highlight python %}
Initial mask m=20, m=10100
Start to check s=20, s=10100
Found submask s=20, s=10100
Start to check s=19, s=10011
Start to check s=18, s=10010
Start to check s=17, s=10001
Start to check s=16, s=10000
Found submask s=16, s=10000
Start to check s=15, s=1111
Start to check s=14, s=1110
Start to check s=13, s=1101
Start to check s=12, s=1100
Start to check s=11, s=1011
Start to check s=10, s=1010
Start to check s=9, s=1001
Start to check s=8, s=1000
Start to check s=7, s=111
Start to check s=6, s=110
Start to check s=5, s=101
Start to check s=4, s=100
Found submask s=4, s=100
Start to check s=3, s=11
Start to check s=2, s=10
Start to check s=1, s=1
{% endhighlight %}

So we basically just traverse every possible mask. But we see that we pretty much have some sort of one out of 6 probability to encounter submask. Now let us check what happens with more interesting numbers such as 16 which has only 1 set bit which means that it has only 1 submask

{% highlight python %}
Initial mask m=16, m=10000
Start to check s=16, s=10000
Found submask s=16, s=10000
Start to check s=15, s=1111
Start to check s=14, s=1110
Start to check s=13, s=1101
Start to check s=12, s=1100
Start to check s=11, s=1011
Start to check s=10, s=1010
Start to check s=9, s=1001
Start to check s=8, s=1000
Start to check s=7, s=111
Start to check s=6, s=110
Start to check s=5, s=101
Start to check s=4, s=100
Start to check s=3, s=11
Start to check s=2, s=10
Start to check s=1, s=1
{% endhighlight %}

Now you see that we traverse so much extra masks in the way that they are not masks which leads us to the question that maybe there is some smarter way. But one more example consider number that has all bits set which is number 15

{% highlight python %}
Initial mask m=15, m=1111
Start to check s=15, s=1111
Found submask s=15, s=1111
Start to check s=14, s=1110
Found submask s=14, s=1110
Start to check s=13, s=1101
Found submask s=13, s=1101
Start to check s=12, s=1100
Found submask s=12, s=1100
Start to check s=11, s=1011
Found submask s=11, s=1011
Start to check s=10, s=1010
Found submask s=10, s=1010
Start to check s=9, s=1001
Found submask s=9, s=1001
Start to check s=8, s=1000
Found submask s=8, s=1000
Start to check s=7, s=111
Found submask s=7, s=111
Start to check s=6, s=110
Found submask s=6, s=110
Start to check s=5, s=101
Found submask s=5, s=101
Start to check s=4, s=100
Found submask s=4, s=100
Start to check s=3, s=11
Found submask s=3, s=11
Start to check s=2, s=10
Found submask s=2, s=10
Start to check s=1, s=1
Found submask s=1, s=1
{% endhighlight %}

Now you see that basicalyl every iteration we do we find some submask which is good

From what we have seen is that lower bound of the complexity of the algorithm highly depends on the distribution of set bits and if all bits are set we have to traverse all possible pairs $$2^k - 1$$ where $$k$$ is the number of set bits. We substract one because sequence of zeros is not a submask. But all possible ways to use or not use for every of $$k$$ positions

There is smarter algorithm. That can enumerate submask directly

{% highlight python %}
m = 20
print(f"Initial mask {m=}, {m=:b}")
s = m
while s > 0:
  print(f"Start to check {s=}, {s=:b}")
  print(f"Found submask {s=}, {s=:b}")
  s = (s - 1) & m
{% endhighlight %}

Again let us first consider 3 cases that we have seen before and then I will prove it. Start with 20

{% highlight python %}
Initial mask m=20, m=10100
Start to check s=20, s=10100
Found submask s=20, s=10100
Start to check s=16, s=10000
Found submask s=16, s=10000
Start to check s=4, s=100
Found submask s=4, s=100
{% endhighlight %}

We see some improve in terms of number of iterations. Proceed to 16

{% highlight python %}
Initial mask m=16, m=10000
Start to check s=16, s=10000
Found submask s=16, s=10000
{% endhighlight %}

That looks really nice. And what about 15

{% highlight python %}
Initial mask m=15, m=1111
Start to check s=15, s=1111
Found submask s=15, s=1111
Start to check s=14, s=1110
Found submask s=14, s=1110
Start to check s=13, s=1101
Found submask s=13, s=1101
Start to check s=12, s=1100
Found submask s=12, s=1100
Start to check s=11, s=1011
Found submask s=11, s=1011
Start to check s=10, s=1010
Found submask s=10, s=1010
Start to check s=9, s=1001
Found submask s=9, s=1001
Start to check s=8, s=1000
Found submask s=8, s=1000
Start to check s=7, s=111
Found submask s=7, s=111
Start to check s=6, s=110
Found submask s=6, s=110
Start to check s=5, s=101
Found submask s=5, s=101
Start to check s=4, s=100
Found submask s=4, s=100
Start to check s=3, s=11
Found submask s=3, s=11
Start to check s=2, s=10
Found submask s=2, s=10
Start to check s=1, s=1
Found submask s=1, s=1
{% endhighlight %}

That is not cool since it is worst case where each bit is set and we have to traverse all of the possible cases. To summarize the worst case is still the same but we have created much better lower bound. And in practice we are likely to have some common distribution of the numbers and we can look forward to get some perfomance boost anyway

Now. How does it work and why does it work. First let us say that we will enumerate all of the submasks in the descending order in terms of value of the number. Now I will prove that on each iteration we select the next element in our descending sequence which is largest among suppose we have mask $$101100$$ and we are at some path on submask $$101000$$ what is next submask in our descending path? Well obviously it is $$100100$$ which we get by removing rightmost set bit and then intersecting with our initial mask. It can be shown that it is the largest submask among submasks with smallest value. For example since it is rightmost we can not remove any smaller number. Now since we go in desc path and on each iteration select largest among next elements we have to visit all of the elements! Suppose it is not true which means we have not visited some element. Since we visit all elements in desc order it means that we have selected next element as not largest among smaller elements. But we said that we select largest. Contradiction works

Here is more exampels [cp-algorithms.com][cp-algorithms.com]

[cp-algorithms.com]: https://cp-algorithms.com/algebra/all-submasks.html

#### **Submasks of inverse**

Popular pattern where we have "used" bitmask, and on each iteration generate all pairs among free elements

{% highlight python %}
used = 20  # 10100
print(f"{used:b}")
not_used = 2 ** 5 - 1 - used  # 5 cuz i know it takes 5 bits
print(f"{not_used:b}")
for submask_candidate in range(1, 2 ** 5): # skip empty element since 0000 is not a submask
  if submask_candidate | not_used == not_used: # submask_candidate did not add any extra bits
    print(f"{submask_candidate:b}")
{% endhighlight %}

#### **Smart Submasks of inverse**

{% highlight python %}
used = 20
print(f"{used:b}")
not_used = 2 ** 5 - 1 - used
not_used_combination = not_used
print(f"{not_used:b}")
while not_used_combination > 0:
  print(f"{not_used_combination:b}")
  not_used_combination = (not_used_combination - 1) & not_used
{% endhighlight %}

#### **[698. Partition to K Equal Sum Subsets][698]**

First, notice that if sum of nums is not divisible by k then it is not possible to form k groups with equal integer sum, return false

Otherwise, task simplifies to the "Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are <del>all equal</del> equal sum(nums)//k"

In total we have k parts. For each part try every possible subset

{% highlight python %}
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        total_sum = sum(nums)
        if total_sum % k != 0:
            return False
        target_sum = total_sum // k

        # Sum Of All Subsets
        s = [0 for _ in range(1 << len(nums))]
        for c in range(1 << len(nums)):
            for i in range(len(nums)):
                if (1 << i) & c:
                    s[c] += nums[i]

        @cache
        def dp(i, used):
            if i == k - 1:
                not_used = 2 ** len(nums) - 1 - used
                return s[not_used] == target_sum
            
            # dumb submasks of inverse
            is_it_possible = False
            not_used = 2 ** len(nums) - 1 - used
            for not_used_combination in range(2 ** len(nums)):
                if (not_used_combination | not_used) != not_used:
                    continue
                if s[not_used_combination] == target_sum:
                    if dp(i + 1, used | not_used_combination):
                        is_it_possible = True
            return is_it_possible


        return dp(0, 0)
{% endhighlight %}

But this will give tle since we have used dumb way to enumerate submasks, we basically enumerate all possible masks and check if mask is submask, now replace to smart way and we are done

{% highlight python %}
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        total_sum = sum(nums)
        if total_sum % k != 0:
            return False
        target_sum = total_sum // k

        # Sum Of All Subsets
        s = [0 for _ in range(1 << len(nums))]
        for c in range(1 << len(nums)):
            for i in range(len(nums)):
                if (1 << i) & c:
                    s[c] += nums[i]

        @cache
        def dp(i, used):
            if i == k - 1:
                not_used = 2 ** len(nums) - 1 - used
                return s[not_used] == target_sum
            
            # Smart submasks of inverse
            is_it_possible = False
            not_used = 2 ** len(nums) - 1 - used
            not_used_combination = not_used
            while not_used_combination > 0:
                if s[not_used_combination] == target_sum:
                    if dp(i + 1, used | not_used_combination):
                        is_it_possible = True
                
                not_used_combination = (not_used_combination - 1) & not_used
            return is_it_possible


        return dp(0, 0)
{% endhighlight %}

And now passed. Generally here we used few patters. 
* [Go to Sum Of All Subsets](#sum-of-all-subsets) to efficiently get sum of combination in O(1)
* [Smart Submasks of inverse](#smart-submasks-of-inverse) to efficiently get submasks of not used elements (i.e free elements)

[698]: https://leetcode.com/problems/partition-to-k-equal-sum-subsets
