---
layout: post
title:  "bits xor dp bitmask submask"
date:   2024-11-30
description: Notes on many patterns used to solve problems and come up with solution to new problems that can also be seen as a building blocks. I tried to present all ideas from simple to hard ones. For each idea there is bunch of problems that rely on that idea
---

Notes on many patterns used to solve problems and come up with solution to new problems that can also be seen as a building blocks. I tried to present all ideas from simple to hard ones. For each idea there is bunch of problems that rely on that idea. I group similar problems that share the same idea and also some problems reference ideas from the past by anchor links so that I will not repeat the whole bunch of stuff under every problem solution text. Also there is no need to preserve order of reading since if you do not understand something you can go back and quickly refresh on some topic you forgot about. You do not have to read the material from bottom to the top. Some topics are independent of another ones but still I ordered them in the sorted by diffuculty order. Also it might be useful to read summary first that provides a clear view on what topics are described so that you can quickly navigate throw. In this note I provide resources and patterns that are sort of building blocks for popular problems that require operations on bits. But there are a lot of problems that are bit problems but still have anther solutions using dfs or math. For such alternative solutions I will probably keep references to the other notes where that solutions are more appropriate to list. Thus this note will have everything you need to tackle any possible problem that requires bit operations. Basically anything that interviewer will throw at you this note will be enough to get it

#### **Summary**

In the [Introduction](#introduction) agreements are presented for common language use that is must read to understand everything else

#### **Introduction**

Any indication of a bit position is counted from the right and advancing left. For example in $$10110$$ bit in position $$0$$ is not set and bit in positon $$1$$ is set the again set then not set and then last bit is set. Also I use zero index notation which means that rightmost bit is considered to have zero index

Refresher for [bits](https://en.wikipedia.org/wiki/Bit) and [bit numbering](https://en.wikipedia.org/wiki/Bit_numbering#Bit_significance_and_indexing) and [endianness](https://en.wikipedia.org/wiki/Endianness) and more [endianness](https://betterexplained.com/articles/understanding-big-and-little-endian-byte-order) and more [endianness](https://commandcenter.blogspot.com/2012/04/byte-order-fallacy.html)

#### **Additional Resources**

Competitive programming [resource](https://cp-algorithms.com/algebra/bit-manipulation.html) part about bit operations

#### **Endianness**

How to convert little endian to big endian. Suppose we have decimal number $$12531$$ which is in binary $$11000011110011$$ that if we split into 2 parts will be $$110000 \ 11110011$$ and pad left part with zeros such that it has length of $$8$$ will become $$00110000 \ 11110011$$ and now it is obvious that if we start with part $$11110011$$ that will be in decimal $$1\cdot2^7 + 1\cdot2^6 + 1\cdot2^5 + 1\cdot2^4 + 0\cdot2^3 + 0\cdot2^2 + 1\cdot2^1 + 1\cdot2^0 = 243$$ and then add $$0 0 1 1 0 0 0 0$$ the same way $$0\cdot2^{15} + 0\cdot2^{14} + 1\cdot2^{13} + 1\cdot2^{12} + 0\cdot2^{11} + 0\cdot2^{10} + 0\cdot2^9 + 0\cdot2^8 = 12288$$ and then sum them both to get $$243 + 12288 = 12531$$ exactly what we have initially. The way we calculated this called big endian in the way that big byte comes first. Big in the way that first byte reading from left to right is offsetted with $$2^7$$

Now if we would like to view that in the little endian notation that we would make big byte last. So we basically need to swap bytes and now we have $$11110011 \ 00110000$$ where $$11110011$$ will equal $$243$$ and $$00110000$$ will equal $$12288$$ that is $$12531$$ in total

It is fair to say that endianness cares about byte order. Now it is clear why some definations say that endianness is a order on which bytes are transmitted or read. It is literally the order in which we update the power of $$2$$ in our calculations in convertion from binary representation to decimal representation

#### **Significance**

There is notation for [significant figures](https://en.wikipedia.org/wiki/Significant_figures) and [this](https://portal.tpu.ru/SHARED/z/ZETA/2/Значащие%20цифры.pdf)

#### **Negative values**

Need some way to represent negative values such that computer can understand that. One way is to go with [twos complement](https://en.wikipedia.org/wiki/Two%27s_complement) and one more [cool](https://www.cs.cornell.edu/~tomf/notes/cps104/twoscomp.html) article

#### **Notes On Bit Operations In Python**





[Python specific Bitwise operations only make sense for integers. The result of bitwise operations is calculated as though carried out in two’s complement with an infinite number of sign bits.](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)

#### **XOR**

0 0 -> 0, 0 1 -> 1, 1 0 -> 1, 1 1 -> 0

* It outputs true whenever outputs are differnet
* It is addition modulo $$2$$ when using just single input
* when using multiple inputs such as 1110 XOR 1001 this is equivalent to addition without carry = 0111. It also means that addition of two n bit strings is identical to the standard vector of addition in the vector space (Z/2Z)^n

[wiki is good](https://en.wikipedia.org/wiki/Exclusive_or)

[nice exmaple about vector addiiotn in Z2](https://codeforces.com/blog/entry/68953)

[very good patterns](https://florian.github.io/xor-trick/)


Xor has Commutative property which means A XOR B = B XOR A
also 

Also A XOR A = 0

#### **XOR of even occurances**

$$a \oplus a \oplus b \oplus b \oplus c = c$$

Generally XOR of even occurances the same element gives 0

#### **XOR of even occurances application**

[136. Single Number](https://leetcode.com/problems/single-number)
just directly use XOR of even occurances property and xor all elements u will get single unique one

[268. Missing Number](https://leetcode.com/problems/missing-number) here we can not just use directly pattern from previous example. but notice that if we add to our XOR also a xor of all elements from 1 to n then we will double all elements except one elemnt that is missing and then xor will output this element!

from 268 use here in [3158. Find the XOR of Numbers Which Appear Twice](https://leetcode.com/problems/find-the-xor-of-numbers-which-appear-twice)

#### **XOR property**

If one builds an array bitmask with the help of the XOR operator, following bitmask ^= x strategy, the bitmask would keep only the bits that appear odd number of times

#### **expand on the fact of Z2 in xor to Z3 in [137. Single Number II](https://leetcode.com/problems/single-number-ii)**


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

if ((1 << i) & number):
    print("Bit Is Set")
else:
    print("Bit Is Not Set")
{% endhighlight %}

Check that bit $$i$$ is set

{% highlight python %}
a = 21
print((1 << i) & a)
{% endhighlight %}

#### **[191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits)**

#### **All combinatons**

How to generate all subsets of array for example. If you have 4 elements than there are $$2^4$$ combinations and each combination is some order of bits of length 4 which are all numbers that are strictly less then $$2^4$$

Generate all subsets. for each subset iterate over its bits. If bit is set means that we have to use that element

#### **[78. Subsets](https://leetcode.com/problems/subsets)**

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

#### **[190. Reverse Bits](https://leetcode.com/problems/reverse-bits)**

{% highlight python %}
class Solution:
    def reverseBits(self, n: int) -> int:
        l = 0
        r = 31
        while l < r:
            l_is_set = (n & (1 << l))
            r_is_set = (n & (1 << r))
            if l_is_set != r_is_set:
                if l_is_set:
                    n = n & (~(1 << l))
                    n = n | (1 << r)
                else:
                    n = n & (~(1 << r))
                    n = n | (1 << l)
            l += 1
            r -= 1
        return n
{% endhighlight %}

this is wrong since l_is_set = (n & (1 << l)) might bit set bit position diff from r_set thus the number it self is different why both have bits set this is why we need

{% highlight python %}
class Solution:
    def reverseBits(self, n: int) -> int:
        l = 0
        r = 31
        while l < r:
            l_is_set = (n & (1 << l)) > 0
            r_is_set = (n & (1 << r)) > 0
            if l_is_set != r_is_set:
                if l_is_set:
                    n = n & (~(1 << l))
                    n = n | (1 << r)
                else:
                    n = n & (~(1 << r))
                    n = n | (1 << l)
            l += 1
            r -= 1
        return n
{% endhighlight %}

https://leetcode.com/problems/reverse-bits/editorial/comments/739002

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

off topic just looks good iterator

{% highlight python %}
def _submasks(self, mask: int) -> Generator[int, None, None]:
  s = mask
  while s > 0:
    yield s
    s = (s - 1) & mask
{% endhighlight %}

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
                # last user must take all of the remaining since all elements must be used
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

#### **[1986. Minimum Number of Work Sessions to Finish the Tasks][1986]**

Pretty much the same task except we have to filter combinations by the extra constrain 
* sum of elements must be no bigger than sessionTime

{% highlight python %}
class Solution:
    def minSessions(self, tasks: List[int], sessionTime: int) -> int:
        n = len(tasks)
        S = [0 for _ in range(1 << n)]
        for c in range(1 << n):
            for i in range(n):
                if (1 << i) & c:
                    S[c] += tasks[i]


        @cache
        def dp(used_tasks: int) -> int:
            if used_tasks == (2 ** n - 1):
                return 0
            options = []
            unused_tasks = (2 ** n - 1) - used_tasks
            unused_tasks_combination = unused_tasks
            while unused_tasks_combination > 0:
                if S[unused_tasks_combination] <= sessionTime:
                    updated_used_tasks = used_tasks | unused_tasks_combination
                    option = 1 + dp(updated_used_tasks)
                    options.append(option)
                unused_tasks_combination = (unused_tasks_combination - 1) & unused_tasks
            return min(options)


        return dp(used_tasks=0)
{% endhighlight %}

#### **[2305. Fair Distribution of Cookies][2305]**

pretty much the same really

{% highlight python %}
class Solution:
    def distributeCookies(self, cookies: List[int], k: int) -> int:
        n = len(cookies)
        S = [0 for _ in range(1 << n)]
        for c in range(1 << n):
            for i in range(n):
                if (1 << i) & c:
                    S[c] += cookies[i]


        @cache
        def dp(kid, used_cookies) -> int:
            if kid == k - 1:
                not_used_cookies = 2 ** n - 1 - used_cookies
                return S[not_used_cookies]
            if used_cookies == 2 ** n - 1:
                return -1
            options = []
            not_used_cookies = 2 ** n - 1 - used_cookies
            not_used_cookies_combination = not_used_cookies
            while not_used_cookies_combination > 0:
                kid_total_cookies = S[not_used_cookies_combination]
                updated_used_cookies = used_cookies | not_used_cookies_combination
                rest_max_per_kid_cookies = dp(kid + 1, updated_used_cookies)
                option = max(kid_total_cookies, rest_max_per_kid_cookies)
                options.append(option)
                not_used_cookies_combination = (not_used_cookies_combination - 1) & not_used_cookies
            return min(options)
        

        return dp(0, 0)

{% endhighlight %}

#### **[1066. Campus Bikes II][1066]**

Just try to assign each worker to each possible bike, then try to do the same with all left workers and without that taken bike

{% highlight python %}
class Solution:
    def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> int:
        n = len(workers)
        m = len(bikes)
        
        
        @cache
        def _assignBikes(worker_index: int, left_bikes_bit_mask: int) -> int:
            if worker_index == n:
                return 0
            options = []
            for bike_index in range(m):
                bike_if_free = not (left_bikes_bit_mask >> bike_index) & 1
                if bike_if_free:
                    worker = workers[worker_index]
                    bike = bikes[bike_index]
                    distance_beterrn_worker_and_bike = self._distance(worker, bike)
                    new_left_bikes_bit_mask = left_bikes_bit_mask | (1 << bike_index)
                    option = distance_beterrn_worker_and_bike + _assignBikes(worker_index + 1, new_left_bikes_bit_mask)
                    options.append(option)
            
            return min(options)


        return _assignBikes(worker_index=0, left_bikes_bit_mask=0)

    def _distance(self, worker: list[int], bike: list[int]) -> int:
        return abs(worker[0] - bike[0]) + abs(worker[1] - bike[1])
{% endhighlight %}

similar [1947. Maximum Compatibility Score Sum][1947]

#### **[1723. Find Minimum Time to Finish All Jobs][1723]**

cmone is it hard?

{% highlight python %}
class Solution:
    def minimumTimeRequired(self, jobs: List[int], k: int) -> int:
        n = len(jobs)
        S = [0 for _ in range(1 << n)]
        for c in range(1 << n):
            for i in range(n):
                if (1 << i) & c:
                    S[c] += jobs[i]


        @cache
        def solve(worker_index: int, taken_jobs_bitmask: int) -> int:    
            if taken_jobs_bitmask == 2 ** n - 1:
                return -1
            if worker_index == k - 1:
                return S[2 ** n - 1 - taken_jobs_bitmask]
            min_option = float("inf")
            not_used = 2 ** n - 1 - taken_jobs_bitmask
            not_used_combination = not_used
            while not_used_combination > 0:
                worker_time = S[not_used_combination]
                if worker_time < min_option:
                    updated_taken_jobs_bitmask = taken_jobs_bitmask | not_used_combination
                    min_option = min(min_option, max(worker_time, solve(worker_index + 1, updated_taken_jobs_bitmask)))
                
                not_used_combination = (not_used_combination - 1) & not_used
            return min_option
        

        return solve(0, 0)
{% endhighlight %}

#### **[1994. The Number of Good Subsets][1994]**

finally some good fucking food. Constraints say that number magnitude is in [1, 30] which means number are quite small. By defenition the product must be represented by distinct prime numbers which means numbers such as 2 * 2 = 4 or 2 * 3 * 3 = 18 are bad for us. So create white list of such numbers which are 2, 3, 5, 6, 7, 10, 11, 13, 14, 15, 17, 19, 21, 22, 23, 26, 29, 30. Also notice that there are only that primes 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, there are only 10 of them, smells like bitmask

The general idea what we want to do is to create state (start_index, used_prime_numbers) and then for each start_index we can try to take or not take value.
* if we take value, then we need to make sure that it is inside list of good numbers. then we need to check that primes, that this number is composed from are not contained in used primes bitmask. then we can take this number, update used primes bitmask and move to the next number
* if we do not take value just keep used primes bitmask the same and move to the next number

also precomoute bitmask represention "used" where each bit represents that one of our 10 primes have been used

also do not forget ones, since they are not prime numbers and do not break our subarrays, then for each subaray we can include any combinatipon of ones (empty combination of ones is good too), basically combinatoric product rule and we gen (2 ** OnesCount) * SubArraysCount where 2 ** OnesCount is all combinations of take/not take one for every one

{% highlight python %}
class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
        prime_masks = []
        for number in range(0, 31):
            prime_mask = self._create_prime_mask(number)
            prime_masks.append(prime_mask)
        number_of_ones = nums.count(1)
        allowed_numbers = set([2, 3, 5, 6, 7, 10, 11, 13, 14, 15, 17, 19, 21, 22, 23, 26, 29, 30])
        MOD = 10 ** 9 + 7


        @cache
        def dp(index, used):
            if index == len(nums):
                return used > 0
            if used == 2 ** 10 - 1:
                return 1
            if prime_masks[nums[index]] & used != 0:
                return dp(index + 1, used)
            if nums[index] in allowed_numbers:
                return (dp(index + 1, used | prime_masks[nums[index]]) + dp(index + 1, used)) % MOD
            return dp(index + 1, used)

        return 2 ** number_of_ones * dp(0, 0) % MOD

    def _create_prime_mask(self, number: int) -> int:
        mask = 0
        prime_divisors = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        for i, prime_divisor in enumerate(prime_divisors):
            if number % prime_divisor == 0:
                mask |= (1 << i)
        return mask
{% endhighlight %}

but this gives memory limit, due to recursion stack since generally we have 10 ** 5 * 2 ** 10 states ~ 100M, tle would occur aswell i guess, just probably there were low amoutn of mem

Notice that number of elements if $$10^5$$ while numbers itself are 1 <= number <= 30 which means we have a lot of duplicates. maybe we can use it somehow? it turns out yes. consider we take some number X to out subarray, and then it means that we can not take any other copy of X, i mean we can not have two or three or more 2s or 3s because it will contradict the definition that states that product consits of distict primes. Soo it leads us to idea that we can iterate over unique values of number and basically compute number of subarrays that can be composed with this value and then multipy by the count of how many times this value occurs!

{% highlight python %}
import collections


class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
        prime_masks = []
        for number in range(0, 31):
            prime_mask = self._create_prime_mask(number)
            prime_masks.append(prime_mask)
        number_of_ones = nums.count(1)
        allowed_numbers = set([2, 3, 5, 6, 7, 10, 11, 13, 14, 15, 17, 19, 21, 22, 23, 26, 29, 30])
        MOD = 10 ** 9 + 7
        C = collections.Counter(nums)
        unique_numbers = list(set(nums))


        @cache
        def dp(index, used):
            if index == len(unique_numbers):
                return used > 0
            if used == 2 ** 10 - 1:
                return 1
            if prime_masks[unique_numbers[index]] & used != 0:
                return dp(index + 1, used)
            if unique_numbers[index] in allowed_numbers:
                return (C[unique_numbers[index]] * dp(index + 1, used | prime_masks[unique_numbers[index]]) + dp(index + 1, used)) % MOD
            return dp(index + 1, used)

        return 2 ** number_of_ones * dp(0, 0) % MOD

    def _create_prime_mask(self, number: int) -> int:
        mask = 0
        prime_divisors = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        for i, prime_divisor in enumerate(prime_divisors):
            if number % prime_divisor == 0:
                mask |= (1 << i)
        return mask
{% endhighlight %}

And now it passes

One more upgrade. Instead of creating large recursion stack and doing if inside function we can pre filter nums array. So we can filter entire nums such that we keep only numbers that are in whitelist. just do unique_numbers = list(set(nums) & allowed_numbers) and then u can move if that checks that in dp func. and code simplifies to


{% highlight python %}
import collections


class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
        prime_masks = []
        for number in range(0, 31):
            prime_mask = self._create_prime_mask(number)
            prime_masks.append(prime_mask)
        number_of_ones = nums.count(1)
        allowed_numbers = set([2, 3, 5, 6, 7, 10, 11, 13, 14, 15, 17, 19, 21, 22, 23, 26, 29, 30])
        MOD = 10 ** 9 + 7
        C = collections.Counter(nums)
        unique_numbers = list(set(nums) & allowed_numbers)


        @cache
        def dp(index, used):
            if index == len(unique_numbers):
                return used > 0
            if used == 2 ** 10 - 1:
                return 1
            if prime_masks[unique_numbers[index]] & used != 0:
                return dp(index + 1, used)
            return (C[unique_numbers[index]] * dp(index + 1, used | prime_masks[unique_numbers[index]]) + dp(index + 1, used)) % MOD

        return 2 ** number_of_ones * dp(0, 0) % MOD

    def _create_prime_mask(self, number: int) -> int:
        mask = 0
        prime_divisors = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        for i, prime_divisor in enumerate(prime_divisors):
            if number % prime_divisor == 0:
                mask |= (1 << i)
        return mask
{% endhighlight %}

#### **[2152. Minimum Number of Lines to Cover Points][2152]**

Rare task. The idea generally is to look at what points are free. loop over all posible pairs of points (n)(n-1), draw line with this points (can get a and b coefs using system equation from 2 points), add to used all points that additionaly lie on this line, return 1 (the line we draw) + draw_lines(updated_used_points)

#### **[2002. Maximum Product of the Length of Two Palindromic Subsequences][2002]**

Really cool one, pre fetch bitmasks for query operation on is palindrome queries

{% highlight python %}
class Solution:
    def maxProduct(self, s: str) -> int:
        n = len(s)
        # i need answer query question that combination is a palindrome
        # precompute that
        combination_to_palindrome_size = [0 for _ in range(1 << n)] # if size > 0 then palindrome exists
        for combination in range(1, 1 << n): # do not consider 
        # empty no set bits, since no chars is not a palindomr
            chars = []
            for i in range(n):
                if (1 << i) & combination:
                    chars.append(s[i])

            l = 0
            r = len(chars) - 1
            while l < r and chars[l] == chars[r]:
                l += 1
                r -= 1
            
            if l > r or chars[l] == chars[r]:
                combination_to_palindrome_size[combination] = len(chars)

        # len(s) <= 12
        # 2 ** 12 combinations
        # then iterative over in the most case 2 ** 12 combinations other
        # thus we have 2 ** 12 * 2 ** 12 = 16M cases
        # for each case check if elements are palindromes?
        a = 0
        for used_first in range(1, 1 << n):
            # check if used_first is palindrom
            used_first_is_palindrome = bool(combination_to_palindrome_size[used_first] > 0)
            if used_first_is_palindrome:
                unused_first = (1 << n) - 1 - used_first
                unused_first_variant = unused_first
                while unused_first_variant > 0:
                    # check that unused_first_variant is palindrom
                    unused_first_variant_is_palindrome = bool(combination_to_palindrome_size[unused_first_variant])
                    if unused_first_variant_is_palindrome:
                        # compute number of set bits in the each palindrom
                        first_size  = combination_to_palindrome_size[used_first]
                        second_size = combination_to_palindrome_size[unused_first_variant]
                        a = max(a, first_size * second_size)

                    unused_first_variant = (unused_first_variant - 1) & unused_first
        
        return a
{% endhighlight %}

#### **[2572. Count the Number of Square-Free Subsets][2572]**

We are not allowed to have divisors that are not square free. That means that we are not allowed to have any prime number in the factorization more then once. If we have some prime number more then once it means that our sum is divisibly by square of that prime number which contradicts the description

What we want to do is to keep track of used primes. Then for each element of the array we will decide whether we can take it or not based on the fact that primes that are contained inside new number have not occured in our used bit mask

We can precompute bit mask of used prime number for every number in advance such that we can reference it laster very fast

But notice that our algorithm that is used to create bit mask with used primes will assign the same bit mask for numbers $$2$$ and $$4$$ and even $$8$$ this is because they all have single prime number used that is $$2$$ but our bit mask does not account for the count. This means that we have to create list of bad numbers that we do not want to take. But notice that $$12$$ is also a bad number in the way that it contains $$4$$ which is bad. That means that we can not used any number that is divisible by $$4$$ and $$9$$ and $$16$$ and $$25$$

Also notice that we can use $$1$$ as many times as we want. That means that total number of combinations is given as $$WithoutOnesCombinations + OnlyUseOnesCombinations + ForEachOneCombinationUseEveryOtherCombination$$ so we need to precompute number of ones in order to do that aswell

{% highlight python %}
import collections


class Solution:
    def squareFreeSubsets(self, nums: List[int]) -> int:
        MOD = 10 ** 9 + 7

        prime_numbers = []
        for i in range(2, 31):
            for j in range(2, i):
                if i % j == 0:
                    break
            else:
                prime_numbers.append(i)
        
        value_to_prime_used = []
        for i in range(0, 31):
            mask = 0
            for j in range(len(prime_numbers)):
                if i % prime_numbers[j] == 0:
                    mask |= (1 << j)
            value_to_prime_used.append(mask)

        ones_count = nums.count(1)
        bad_numbers = []
        for num in range(1, 31):
            if num % 4 == 0 or num % 9 == 0 or num % 16 == 0 or num % 25 == 0:
                bad_numbers.append(num)
        bad_numbers.append(1)
        bad_numbers = set(bad_numbers)

        @cache
        def dp(i, used):
            if i == len(nums):
                return used > 0

            count = 0
            value = nums[i]
            value_prime_mask = value_to_prime_used[value]
            not_used = 2 ** 10 - 1 - used
            if (nums[i] not in bad_numbers) and (not_used | value_prime_mask == not_used):
                count = (count + dp(i + 1, used | value_prime_mask)) % MOD

            count = count + dp(i + 1, used)
            return count % MOD

        ans = dp(0, 0)
        return ((ans) + (2 ** ones_count - 1) + (ans * (2 ** ones_count - 1))) % MOD
 {% endhighlight %}

But this gives memory limit. Notice that we can not use any prime number more then once. Basically all of the pairs that we can have determined by all possible submasks of mask of ones of length $$10$$ since there are $$10$$ primes that are less then $$30$$ which means that instead of iterating over all elements we can iterate over unique elements and just update number of pairs multiplying by occurance count of each prime. Indeed suppose mask $$10010$$ is given which means that we have used $$3$$ and $$11$$ and in total we have $$5$$ occurances of $$3$$ and $$2$$ occurances of $$11$$ it is obvious that total number of occurances if $$5 \cdot 2$$ that is $$10$$

{% highlight python %}
import collections


class Solution:
    def squareFreeSubsets(self, nums: List[int]) -> int:
        MOD = 10 ** 9 + 7

        prime_numbers = []
        for i in range(2, 31):
            for j in range(2, i):
                if i % j == 0:
                    break
            else:
                prime_numbers.append(i)
        
        value_to_prime_used = []
        for i in range(0, 31):
            mask = 0
            for j in range(len(prime_numbers)):
                if i % prime_numbers[j] == 0:
                    mask |= (1 << j)
            value_to_prime_used.append(mask)
    
        ones_count = nums.count(1)
        bad_numbers = []
        for num in range(1, 31):
            if num % 4 == 0 or num % 9 == 0 or num % 16 == 0 or num % 25 == 0:
                bad_numbers.append(num)
        bad_numbers.append(1)
        bad_numbers = set(bad_numbers)
        C = collections.Counter(nums)
        unique_numbers = list(set(nums))


        @cache
        def dp(i, used):
            if i == len(unique_numbers):
                return used > 0
            count = 0
            value = unique_numbers[i]
            value_prime_mask = value_to_prime_used[value]
            not_used = 2 ** 10 - 1 - used
            if (unique_numbers[i] not in bad_numbers) and (not_used | value_prime_mask == not_used):
                count = (count + C[unique_numbers[i]] * dp(i + 1, used | value_prime_mask)) % MOD

            count = count + dp(i + 1, used)
            return count % MOD

        ans = dp(0, 0)
        return ((ans) + (2 ** ones_count - 1) + (ans * (2 ** ones_count - 1))) % MOD
{% endhighlight %} 

#### **[464. Can I Win][464]**

Nice task

#### **[136. Single Number][136]**

Nice task

Where to look for more tasks? Leetcode filter by tag bitmask, maybe search some lists on codeforces

#### todo

check that list https://leetcode.com/problem-list/bit-manipulation/
check that list https://leetcode.com/problem-list/bitmask/
https://leetcode.com/discuss/interview-question/3695233/all-types-of-patterns-for-bits-manipulations-and-how-to-use-it
https://leetcode.com/problems/sum-of-two-integers/solutions/84278/A-summary:-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently

looks like 20/80 done

[698]: https://leetcode.com/problems/partition-to-k-equal-sum-subsets
[1986]: https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks
[2305]: https://leetcode.com/problems/fair-distribution-of-cookies
[1066]: https://leetcode.com/problems/campus-bikes-ii
[1723]: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs
[1994]: https://leetcode.com/problems/the-number-of-good-subsets
[1947]: https://leetcode.com/problems/maximum-compatibility-score-sum
[2152]: https://leetcode.com/problems/minimum-number-of-lines-to-cover-points
[2002]: https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences
[2572]: https://leetcode.com/problems/count-the-number-of-square-free-subsets
[464]: https://leetcode.com/problems/can-i-win
[136]: https://leetcode.com/problems/single-number
