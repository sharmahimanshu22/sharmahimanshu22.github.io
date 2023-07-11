---
title: Discussion on Plane Sweep Algorithm to Find Intersections
layout: post
---

These are some thoughts after reading Section 2.1 of the book Computational Geometry : Algorithms and Applications

1. Maintain an event queue: At first look we think we need a priority queue. However, while adding events to priority queue, we need to check if that event is already present in the queue. This is relevant when two or more segments start at the same point or end at the same point or more than two segments intersect at common point. The standard implementation of a priority queue is based on heap which makes this operation inefficient. (Heap takes O(n) time to search for an element. Basically we have to check all the elements of the heap). The C++ std::priority_queue does not even provide the method "contains". Most of discussions on line sweep algorithm will omit this detail and assume prioirty queue to be the obvious data structure for event queue.
<br><br>
So it is better to use balanced binary search tree and do a DFS on it to get the events in order.


2. The data structure storing the  "status" at current sweep line is also stored as balancedd binary search tree. For any event we will look for the line segments associated with the event point "p" in this data structure. 
