---
layout: post
title:  "Bitmask dp"
date:   2024-11-30
description: What is bitmask dp and what approaches ared used to solve such kind of problems. Also some useful algorithms such as efficient submask enumeration and bit operations. Examples of few leetcode problems and their solutions with proofs and code examples
---
Some bitmasks

**Submask enumeration**

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

**Mask sum queries**

Suppose we have non sorted array of numbers of small size. We will get queries sort of get sum of values on specific indicies. Possible zero or possible all. Basically we will be asked to answer sum of values for any combination of indices in the array

{% highlight python %}
def create_sums(array: list[int]) -> list[int]:
  sums = []
  for combination in range(2 ** len(array)):
    combination_sum = 0
    for array_index in range(len(array)):
      array_index_bit_is_set = (1 << array_index) & combination
      if array_index_bit_is_set:
        combination_sum += array[array_index]

    sums.append(combination_sum)
  return sums
{% endhighlight %}

Then we can query it like 

{% highlight python %}
array = [4, 3, 6, 1]
sums = create_sums(array)

combination = 9 # 1001 in binary meaning select first and last elements
print(sums[combination])
{% endhighlight %}

Returns

{% highlight python %}
5
{% endhighlight %}

**[1723. Find Minimum Time to Finish All Jobs][1723]**

Generally what we wanna do is the following. For each worker try all possible combinations of jobs he can make. Update used jobs such that other workers would not be able to do them. Do the same thing for next worker. Do that untill there are no jobs left or you have last worker which then has to do all of the remaining jobs (by definition all jobs have to be done and since only 1 worker is left he does not have choice)

Pseudo code might look like

{% highlight python %}
def minimum_maximum(start_worker_index: int, used_jobs_bitmask: int) -> int:
    if worker is last:
        total_sum = sum times among jobs that have not been done by any one. We can
        do that by using [MASK SUM QUERIES] and query sum array by inverse of used_jobs_bitmask
        since inverse would contain set bits of free jobs! so that we will get sum of all set bits meaning
        all free jobs

    if all jobs are done:
        return -1 such that when we will do max with that value it will never be choosed

    [SUBMASK ENUMERATION] for every possible possible combination of free jobs. We can enumerate submasks efficient way.
    if we inverse used_jobs_bitmask we get set bits for places where jobs are not taken. then we are 
    interested in combinations of thoose ones meaning sub masks if inverse of used_jobs_bitmask

        [MASK SUM QUERIES] get sum of time of jobs in the combination and set to total work time of current worker
        update used_jobs_bitmask with combination of jobs
        get maximum total work time among other workers minimum_maximum(start_worker_index++, updated used_jobs_bitmask)
        then maximum total time is maximum among current worker and other workers
        if that maximum total time is smaller then minimum update minimum
{% endhighlight %}

Now the code

{% highlight python %}
class Solution:
    def minimumTimeRequired(self, jobs: List[int], k: int) -> int:
        sums = self._create_sums(jobs)
        T = 2 ** len(jobs) - 1

        @cache
        def solve(worker_index: int, taken_jobs_bitmask: int) -> int:    
            if taken_jobs_bitmask == T:
                return -1
            if worker_index == k - 1:
                return sums[T - taken_jobs_bitmask]
            min_option = float("inf")
            m = T - taken_jobs_bitmask # inverse of taken_jobs_bitmask mearning set bits are free jobs
            selected_jobs = m
            while selected_jobs > 0: # take all submasks of free jobs!
                selected_jobs = m & (selected_jobs - 1)
                worker_time = sums[selected_jobs] # query sum by mask!
                if worker_time < min_option:
                    updated_taken_jobs_bitmask = taken_jobs_bitmask | selected_jobs
                    min_option = min(min_option, max(worker_time, solve(worker_index + 1, updated_taken_jobs_bitmask)))
            return min_option


        return solve(0, 0)

    def _create_sums(self, array: list[int]) -> list[int]:
        sums = []
        for combination in range(2 ** len(array)):
            combination_sum = 0
            for array_index in range(len(array)):
                array_index_bit_is_set = (1 << array_index) & combination
                if array_index_bit_is_set:
                    combination_sum += array[array_index]
            sums.append(combination_sum)
        return sums
{% endhighlight %}

Or more nicer code with iterator for submasks finding

{% highlight python %}
class Solution:
    def minimumTimeRequired(self, jobs: List[int], k: int) -> int:
        sums = self._create_sums(jobs)
        T = 2 ** len(jobs) - 1

        @cache
        def solve(worker_index: int, taken_jobs_bitmask: int) -> int:    
            if taken_jobs_bitmask == T:
                return -1
            if worker_index == k - 1:
                return sums[T - taken_jobs_bitmask]
            min_option = float("inf")
            for selected_jobs in self._submasks(T - taken_jobs_bitmask): # T - taken_jobs_bitmask -> free_jobs_bitmask
                worker_time = sums[selected_jobs]
                if worker_time < min_option:
                    updated_taken_jobs_bitmask = taken_jobs_bitmask | selected_jobs
                    min_option = min(min_option, max(worker_time, solve(worker_index + 1, updated_taken_jobs_bitmask)))
            return min_option
        

        return solve(0, 0)

    def _create_sums(self, array: list[int]) -> list[int]:
        sums = []
        for combination in range(2 ** len(array)):
            combination_sum = 0
            for array_index in range(len(array)):
                array_index_bit_is_set = (1 << array_index) & combination
                if array_index_bit_is_set:
                    combination_sum += array[array_index]
            sums.append(combination_sum)
        return sums

    def _submasks(self, mask: int) -> Generator[int, None, None]:
        s = mask
        while s > 0:
            yield s
            s = (s - 1) & mask
{% endhighlight %}

[1723]: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/description/

<!-- Given a bitmask  $m$ , you want to efficiently iterate through all of its submasks, that is, masks  $s$  in which only bits that were included in mask  $m$  are set. -->
