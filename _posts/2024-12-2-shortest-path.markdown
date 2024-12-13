---
layout: post
title:  "shortest path"
date:   2024-12-2
description: Notes on many patterns used to solve problems and come up with solution to new problems that can also be seen as a building blocks for shortest path related problems
---

Notes [dijkstra](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

#### 🫑 **[1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort)**

Construct graph from given data i.e consider each cell as a vertex and connect it with adjacement cells. Run dijkstra algorithm from top left vertex to the bottom right vertex

General notes

* Construct graph from data and run dijkstra

#### 🫑 **[743. Network Delay Time](https://leetcode.com/problems/network-delay-time)**

Notice that minimum time it takes to all nodes to get a signal is actually means that we have to wait for the most slow signal to arrive. So check arrival time to all nodes and then select maximum. For this run dijkstra to findout all times for all nodes and select max

#### 🫑 **[1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability)**

Since dijkstra finds minimum but we need to find maximum replace maximization of multiplication with minimization of division and then return 1/answer

#### 🫑 **[505. The Maze II](https://leetcode.com/problems/the-maze-ii)**

Easy task but pain to code. Raw dijkstra, just build the right graph

#### 🍑 **[787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops)**

small optimiziation that you do not need to visit nodes that use more flights then used in node and spent price more then has been spent already

#### 🫑 **[1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance)**

Run dijkstra from each person, filter by distances check if it satisfies the question

#### 🌶️ **[3377. Digit Operations to Make Two Integers Equal](https://leetcode.com/problems/digit-operations-to-make-two-integers-equal)**

Solid. kinda represent the whole thing as a graph and then traverse it. small optimization, as soon as u get to the target node stop traverse since it means u will never make the result better, do not waster time for other nodes

