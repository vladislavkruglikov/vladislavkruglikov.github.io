---
layout: post
title:  "Bitmask dp"
date:   2024-11-30
description: What is bitmask dp and what approaches ared used to solve such kind of problems. Also some useful algorithms such as efficient submask enumeration and bit operations. Examples of few leetcode problems and their solutions with proofs and code examples
---

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

**[🍊 medium 698. Partition to K Equal Sum Subsets][698]**

???add tags and reference to popular techniques

We can deduce that the target sum of each partition must be equal to. Then for each partition try all possible combinations of elements

* Use mask sum queries and prepare array to quickly find sum of elemente of the array for any possible combination
* dp state (group_index, used). For each state try all possible combination among unused elements. Generate all submasks of inversee of used. For each submask check if its sum is equal to target sum and that the dp(group_index+1, updated_used) can be splitted

[Some Link]({% post_url 2024-11-30-welcome-to-jekyll %})

[1723]: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs
[1066]: https://leetcode.com/problems/campus-bikes-ii
[1986]: https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks
[1723]: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs
[2305]: https://leetcode.com/problems/fair-distribution-of-cookies
[1994]: https://leetcode.com/problems/the-number-of-good-subsets
