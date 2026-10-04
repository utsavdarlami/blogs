+++
title = "Breadth-first search"
author = ["felladog"]
date = 2021-07-30T10:18:00-05:00
lastmod = 2026-08-31T15:20:57-05:00
tags = ["AI Survey"]
categories = ["AI"]
draft = true
+++

---

-   References :

-   Questions :

---

The root node is expanded first, then all the successors of the root node are expanded next, then their successors, and so on. A first in first out [Queue]({{< relref "20210730090814-data_structures.md#queue" >}}) is used to store the nodes so that new nodes (which are always deeper than their parents) go to the back of the queue, and old nodes, which are shallower than the new nodes, get expanded first.

It always finds a solution with a minimal number of actions, because when it is generating nodes at depth d, it has already generated all the nodes at depth d−1, so if one of them were a solution, it would have been found

The memory requirements are a bigger problem for breadth-first search than the execution time.

```latex
function BREADTH-FIRST-SEARCH(problem) returns a solution node or failure
  node ← NODE(problem.INITIAL)
  if problem.IS-GOAL(node.STATE) then return node
  frontier ← a FIFO queue, with node as an element
  reached ← {problem.INITIAL}
  while not IS-EMPTY(frontier) do
   node ← POP(frontier)
   for each child in EXPAND(problem, node) do
    s ← child.STATE
    if problem.IS-GOAL(s) then return child
    if s is not in reached then
     add s to reached
     add child to frontier
   return failure
```


## figure showing bfs {#figure-showing-bfs}

{{< figure src="/ox-hugo/2026-08-31_15-19-25_screenshot.png" >}}
