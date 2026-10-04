+++
title = "Depth-first search"
author = ["felladog"]
date = 2021-07-30T18:04:00-05:00
lastmod = 2026-08-31T13:40:18-05:00
tags = ["AI Survey"]
categories = ["AI"]
draft = true
+++

---

-   References :

-   Questions :

---
Depth-first search always expands the deepest node in the frontier first. Depth-first search is not cost-optimal; it returns the first solution it finds, even if it is not cheapest.
Depth-first search has much smaller needs for memory. We don’t keep a reached table at all,
and the frontier is very small.

-   LIFO queue([Stack]({{< relref "20210730090814-data_structures.md#stack" >}})) is used to store the nodes.

It might get stuck in an infinite loop for cyclic or infinite state spaces.


## Depth-limited search {#depth-limited-search}

To keep depth-first search from wandering down an infinite path, we can use depth-limited search, a version of depth-first search in which we supply a depth limit, _l_, and treat all nodes
at depth _l_ as if they had no successors
The diameter of the state-space graph, gives us a better depth limit.

```latex
function DEPTH-LIMITED-SEARCH(problem, l) returns a node or failure or cutoff
  frontier ← a LIFO queue(stack) with NODE(problem.INITIAL) as an element
  result ← failure
  while not IS-EMPTY(frontier) do
   node ← POP(frontier)
   if problem.IS-GOAL(node.STATE) then return node
   if DEPTH(node) > l then
     result ← cutoff
   else if not IS-CYCLE (node) do
     for each child in EXPAND (problem, node) do
      add child to frontier
  return result
```

It returns one of three different types of values: either a solution node; or failure, when it has exhausted all nodes and proved there is no solution at any depth; or cutoff , to mean there might be a solution at a deeper depth than l.


## Iterative Deepening search {#iterative-deepening-search}

Iterative deepening search solves the problem of picking a good value for _l_ by trying all values: first 0, then 1, then 2, and so on—until either a solution is found, or the depth-limited search returns the failure value rather than the cutoff value.
Iterative deepening combines many of the benefits of depth-first and breadth-first search.

```latex
function ITERATIVE-DEEPENING-SEARCH(problem) returns a solution node or failure
  for depth = 0 to ∞ do
    result ← DEPTH-LIMITED-SEARCH(problem, depth)
    if result != cutoff then return result
```

Iterative deepening repeatedly applies depth-limited search with increasing limits. In general, iterative deepening is the preferred uninformed search method when the search state space is larger than can fit in memory and the depth of the solution is not known.
