+++
title = "uniform-cost search"
author = ["felladog"]
date = 2021-07-30T11:21:00-05:00
lastmod = 2026-08-31T13:49:43-05:00
tags = ["AI Survey"]
categories = ["AI"]
draft = true
+++

---

-   References :

-   Questions :

---

The idea is that while [Breadth-first search]({{< relref "20210730101847-breadth_first_search.md" >}}) spreads out in waves of uniform depth—first depth 1, then depth 2, and so on—uniform-cost search spreads out in waves of uniform path-cost. The algorithm can be implemented as a call to BEST-FIRST-SEARCH([Best-First Search]({{< relref "20210728214523-problem_solving_in_ai.md#best-first-search" >}})) with PATH-COST as the evaluation function. PATH-COST is the cost of the path from the root to the current node.

**Notes: OR we can have priority queue instead of normal queue**

```latex
function UNIFORM-COST-SEARCH(problem) returns a solution node, or failure
  return BEST-FIRST-SEARCH(problem, PATH-COST)
```

It is complete and is cost-optimal, because the first solution it finds will have a cost that is at least as low as the cost of any other node in the frontier. Uniform-cost search considers all paths systematically in order of increasing cost, never getting caught going down a single infinite path

{{< figure src="/ox-hugo/ucs_eg.png" caption="<span class=\"figure-number\">Figure 1: </span>UCS example." width="550" height="320" target="/blogs" >}}

-   Solution Table

| open list                          | Node to expand | Cost so far |
|------------------------------------|----------------|-------------|
| C(0)                               | C              | 0           |
| T(1), B(2), E(2), O(3), P(5)       | T              | 1           |
| B(2), E(2), O(3), P(5)             | B              | 1           |
| E(2), O(3), A(3), P(5), S(5), R(6) | E              | 2           |
