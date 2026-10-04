+++
title = "problem solving in AI"
author = ["felladog"]
date = 2021-07-28T21:45:00-05:00
lastmod = 2026-08-31T13:43:09-05:00
tags = ["AI Survey"]
categories = ["AI"]
draft = true
+++

---

-   References :
    -   [Book] Artificial Intelligence A Modern Approach, Stuart J. Russell and Peter Norvig, Chapter 3

-   Questions :

---

**Problem-solving [agent]({{< relref "20210726203718-ai.md" >}})** undertakes a computational process called **search** to consider a sequence of actions that form a path to a goal state. A sequence of actions forms a **path**, and a **solution** is a path from the initial state to a goal state. These agents use **atomic** representations. Often leads to the formulation of a model—an abstract mathematical description.

-   Search algorithms

    **Informed algorithms**, in which the agent can estimate how far it is from the goal, and **uninformed algorithms**, where no such estimate is available.

-   Four-phase problem-solving process
    1.  **Goal formulation**,
        Agents adopt the goal to goal state.
    2.  **Problem formulation**,
        The agent devises a description of the states and actions necessary to reach the goal—an abstract model of the relevant part of the world.
    3.  **Search**,
        Before taking any action in the real world, the agent simulates sequences of actions in its model, searching until it finds a sequence of actions that reaches the goal. Such a sequence is called a solution.
    4.  **Execution**,
        The agent can now execute the actions in the solution, one at a time.

In a fully observable, deterministic, known environment, the solution to any problem is a fixed sequence of actions.

-   **Open-loop system**,
    Agent ignores its percepts while it is executing the actions.
-   **Closed-loop system**
    Approach that monitors the percepts. If there is a chance that the model is incorrect, or the environment is nondeterministic, then the agent would be safer using this system.


## Search Problem {#search-problem}

Can be defined formally as follows :

-   A set of possible **states** that the environment can be in. We call this the **state space**
-   The **initial state** that the agent starts in.
-   A set of one or more **goal states**.
-   The **actions** available to the agent. Given a state s, `ACTIONS(s)` returns a finite set of actions that can be executed in s. We say that each of these actions is applicable in s.
-   A **transition model**, which describes what each action does. `RESULT(s,a)` returns the state that results from doing action a in state s.
-   An **action cost function**, denoted by `ACTION-COST(s,a,s')` gives the numeric cost of applying action a in state s to reach state s' . A problem-solving agent should use a cost function that reflects its own performance measure.

An **optimal solution** has the lowest path cost among all solutions.


### State Space {#state-space}

Consists of

-   Set of states
    -   Initial State, Goal state
-   Set of operators/acions
    -   Applying an operator/action to a state transforms it to another state in the state space i.e actions
    -   Not all operators are applicable to all states


## Search Algorithm {#search-algorithm}

A search algorithm takes a search problem as input and returns a solution, or an indication of failure.


### Search tree {#search-tree}

Some search algorithms form a search tree over the state space graph, forms various paths from the initial state, trying to find a path that reaches a goal state.
Each node in the search tree corresponds to a state in the state space and the edges in the search tree correspond to actions. The root of the tree corresponds to the initial state of the problem.


#### Distinction between state space and search tree {#distinction-between-state-space-and-search-tree}

The state space describes the (possibly infinite) set of states in the world, and the actions that allow transitions from one state to another. The search tree describes paths between these states, reaching towards the goal.


### Search [data structures]({{< relref "20210730090814-data_structures.md" >}}) {#search-data-structures--20210730090814-data-structures-dot-md}


#### For Node in the tree {#for-node-in-the-tree}

1.  `node.STATE` : the state to which the node corresponds;
2.  `node.PARENT` : the node in the tree that generated this node;
3.  `node.ACTION` : the action that was applied to the parent’s state to generate this node;
4.  `node.PATH-COST` : the total cost of the path from the initial state to this node


#### To Store frontier {#to-store-frontier}

Frontier is all the state to be considered next for expanding. To store these state we use some kind of [Queue]({{< relref "20210730090814-data_structures.md#queue" >}}) data structures. In search algorithms we use following three kinds of queues:

1.  A **priority queue** first pops the node with the minimum cost according to some evaluation function, f. It is used in **best-first search**.
2.  A **FIFO queue or first-in-first-out queue** first pops the node that was added to the queue first; we shall see it is used in **breadth-first search**.
3.  A **LIFO queue or last-in-first-out queue** (also known as a [Stack]({{< relref "20210730090814-data_structures.md#stack" >}})) pops first the most recently added node; it is used in **depth-first search**.

<!--list-separator-->

-  Operations on a frontier are

    -   IS-EMPTY(frontier) returns true only if there are no nodes in the frontier.
    -   POP(frontier) removes the top node from the frontier and returns it.
    -   TOP(frontier) returns (but does not remove) the top node of the frontier.
    -   ADD(node, frontier) inserts node into its proper place in the queue.


### Evaluating Search Algorithms Performance {#evaluating-search-algorithms-performance}

1.  **Completeness** : Is the algorithm guaranteed to find a solution when there is one, and to correctly report failure when there is not?
2.  **Cost optimality** : Does it find a solution with the lowest path cost of all solutions?
3.  **Time complexity** : How long does it take to find a solution? This can be measured in seconds, or more abstractly by the number of states and actions considered.
4.  **Space complexity** : How much memory is needed to perform the search?


#### Complexities of Search Algorithms {#complexities-of-search-algorithms}

<!--list-separator-->

-  For Explicit search graph

    The typical measure is the size of the state-space graph, `|V| + |E|`, where |V| is the number of vertices (state nodes) of the graph and |E| is the number of edges (distinct state/action pairs)

<!--list-separator-->

-  For Implicit search graph

    In many AI problems, the graph is represented only implicitly by the initial state, actions, and transition model. For an implicit state space, complexity can be measured in terms of **d**, the **depth** or number of actions in an optimal solution; **m**, the maximum number of actions in any path; and **b**, the **branching factor** or number of successors of a node that need to be considered.


### Best-First Search {#best-first-search}

Is used to choose a node from the frontier to expand next.

We choose a node, **n**, which has a minimum value for some evaluation function, **f(n)**. The node is returned if its state is a goal state otherwise apply EXPAND operation to generate child nodes. These child nodes get added to the frontier. The nodes are ordered in increasing order of cost.
The algorithm returns either an indication of failure or a node that represents a path to a goal.

```latex
function BEST-FIRST-SEARCH(problem, f) returns a solution node or failure
  node ← NODE(STATE=problem.INITIAL)
  frontier ← a priority queue ordered by f, with node as an element
  reached ← a lookup table, with one entry with key problem.INITIAL and value node
  while not IS-EMPTY(frontier) do
   node ← POP(frontier)
   if problem.IS-GOAL(node.STATE) then return node
   for each child in EXPAND(problem, node) do
    s ← child.STATE
    if s is not in reached or child.PATH-COST < reached[s].PATH-COST then
      reached[s] ← child
      add child to frontier
    return failure

function EXPAND(problem, node) yields nodes
  s ← node.STATE
  for each action in problem.ACTIONS (s) do
   s' ← problem.RESULT(s, action)
   cost ← node.PATH-COST + problem.ACTION-COST(s, action, s' )
   yield NODE(STATE=s', PARENT=node, ACTION=action, PATH-COST=cost)
```


## Uninformed Search Strategies {#uninformed-search-strategies}

It is given no clue about how close a state is to the goal(s).


### [Breadth-first search]({{< relref "20210730101847-breadth_first_search.md" >}}) {#breadth-first-search--20210730101847-breadth-first-search-dot-md}

When all actions have the same cost, an appropriate strategy is Breadth-first search. It is [Best-First Search](#best-first-search) where the evaluation function f(n) is the depth of the node—that is, the number of actions it takes to reach the node.

It allows **early goal test**, checking whether a node is a solution as soon as it is generated, rather than the **late goal test** that best-first search uses, waiting until a node is popped off the queue.


### [Depth-first search]({{< relref "20210730180414-depth_first_search.md" >}}) {#depth-first-search--20210730180414-depth-first-search-dot-md}

It could be implemented as a call to BEST-FIRST-SEARCH where the evaluation function f is the negative of the depth.


### [Uniform-cost search]({{< relref "20210730112128-uniform_cost_search.md" >}}) {#uniform-cost-search--20210730112128-uniform-cost-search-dot-md}

When actions have different costs, an obvious choice is to use best-first search where the
evaluation function is the cost of the path from the root to the current node.


### [Iterative Deepening search]({{< relref "20210730180414-depth_first_search.md#iterative-deepening-search" >}}) {#iterative-deepening-search--20210730180414-depth-first-search-dot-md}


### [Bidirectional Search]({{< relref "20210730184418-bidirectional_search.md" >}}) {#bidirectional-search--20210730184418-bidirectional-search-dot-md}

Bidirectional search simultaneously searches forward from the initial state and backwards from the goal state(s), hoping that the two searches will meet.


### Comparing uninformed search algorithms {#comparing-uninformed-search-algorithms}

{{< figure src="/ox-hugo/search_t.png" caption="<span class=\"figure-number\">Figure 1: </span>complexities." width="750" height="200" target="/blogs" >}}

The above algorithm do not check for Repeated States. Failure to detect repeated states can turn a linear problem into an exponential one.

Solutions for Repeated States:

1.  Do not create paths containing cycles
2.  Never generate a state generated before. Memory inefficient since we need to track the states generate.


## Informed(Heuristic) Search Strategies {#informed--heuristic--search-strategies}

Uses domain-specific hints about the location of goals—can find solutions more efficiently than an uninformed strategy. The hints come in the form of a heuristic function, denoted h(n)

`h(n) = estimated cost of the cheapest path from the state at node n to a goal state.`


### Greedy best-first search {#greedy-best-first-search}

Greedy best-first search is a form of best-first search that expands first the node with the lowest h(n) value—the node that appears to be closest to the goal—on the grounds that this is likely to lead to a solution quickly. So the evaluation function f (n) = h(n).


### A\* Search {#a-search}

best-first search that uses the evaluation function `f(n) = g(n) + h(n)` where,

-   g(n) is the path cost from the initial state to node n, and
-   h(n) is the estimated cost of the shortest path from n to a goal state, so we have

f(n) = estimated cost of the best path that continues from n to a goal.


#### Admissible heuristic {#admissible-heuristic}

It never overestimates the cost to reach a goal.
A heuristic h(n) is admissible if for every node n, h(n) ≤ h\*(n), where h\*(n) is the true cost to reach the goal state from n.

**Theorem**: If h(n) is admissible, A\* using TREE-SEARCH is optimal


#### Consistent heuristic {#consistent-heuristic}

A heuristic h(n) is consistent if, for every node n and every successor n' of n generated by an action a, we have: **h(n) ≤ c(n, a, n') + h(n')**

Every consistent heuristic is admissible. So,

**Theorem**: If h(n) is consistent, A\* using GRAPH-SEARCH is optimal

<!--list-separator-->

-  Triangle inequality

    A side of a triangle cannot be longer than the sum of the other two sides.

    If the heuristic h is consistent, then the single number h(n) will be less than the sum of the cost c(n, a, n' ) of the action from n to n' plus the heuristic estimate h(n').

    {{< figure src="/ox-hugo/tri_in_eq.png" caption="<span class=\"figure-number\">Figure 2: </span>triangle inequality." width="420" height="220" target="/blogs" >}}


#### Optimality of A\* {#optimality-of-a}

{{< figure src="/ox-hugo/a_contour.png" caption="<span class=\"figure-number\">Figure 3: </span>Map of Romania showing contours at f = 380, f = 400, and f = 420, with Arad as the start state." width="500" height="300" target="/blogs" >}}

The contours with uniform-cost search will be “circular” around the start state, spreading out equally in all directions with no preference towards the goal. With A\* search using a good heuristic, the g + h bands will stretch toward a goal state and become more narrowly
focused around an optimal path.

-   A\* expands nodes in order of increasing f value
-   Gradually adds "f-contours" of nodes
-   Contour i contains all nodes with \\( f \leq f\_i \\) where \\( f\_i < f\_{i+1} \\)

A\* is efficient because it prunes away search tree nodes that are not necessary for finding an optimal solution.


### Memory bounded search {#memory-bounded-search}

The main issue with A\* is its use of memory. Memory is split between the frontier and the reached states. Sometimes a state may be stored in two places: as a node in the frontier and as an entry in the table of reached states.


### Recursive best-first search(RBFS) {#recursive-best-first-search--rbfs}

RBFS resembles a recursive depth-first search, but rather than continuing indefinitely down the current path, it uses the f_limit variable to keep track of the f_value of the best alternative path available from any ancestor of the current node.
If the current node exceeds this limit, the recursion unwinds back to the
alternative path.

We remember the best f-value we have found so far in the branch we are deleting.

```latex
function RECURSIVE-BEST-FIRST-SEARCH(problem) returns a solution or failure
  solution, fvalue ← RBFS(problem, NODE(problem.INITIAL), ∞)
  return solution

function RBFS(problem, node, f_limit) returns a solution or failure, and a new f-cost limit
    if problem.IS-GOAL(node.STATE) then return node
    successors ← LIST(EXPAND(node))
    if successors is empty then return failure, ∞
    for each s in successors do // update f with value from previous search
      s.f ← max(s.PATH-COST + h(s), node.f))
    while true do
      best ← the node in successors with lowest f-value
      if best.f > f_limit then return failure, best.f
      alternative ← the second-lowest f-value among successors
      result, best.f ← RBFS(problem, best, min(f_limit, alternative))
      if result != failure then return result, best.f
```

RBFS uses only linear space of memory: even if more memory were available, RBFS has no way to make use of it because they forget most of what they have done.


### Heuristic Functions {#heuristic-functions}

How should we consider a heuristic functions and how the accuracy of a heuristic affects search performance.


#### For 8-puzzle {#for-8-puzzle}

-   h1 = the number of misplaced tiles (blank not included).
-   h2 = the sum of the distances of the tiles from their goal positions. Because tiles cannot move along diagonals, the distance is the sum of the horizontal and vertical distances—sometimes called the city-block distance or [Manhattan distance]({{< relref "20240303195425-manhattan_distance.md" >}}).

Both the above heuristic are admissible


#### Effect of heuristic accuracy on search performance {#effect-of-heuristic-accuracy-on-search-performance}

Ways to characterize the quality of a heuristic is

-   effective branching factor
-   effective depth
-   Dominance
    -   If h2(n) ≥ h1(n) for all n (both admissible) then h2 dominates h1. h2 is better for search as it is guaranteed to expand less nodes.
    -   It is generally better to use a heuristic function with higher values, provided it is consistent and that the computation time for the heuristic is not too long.


#### Generating heuristics from relaxed problems {#generating-heuristics-from-relaxed-problems}

A problem with fewer restrictions on the actions is called a relaxed problem.

The state-space graph of the relaxed problem is a supergraph of the original state space because the removal of restrictions creates added edges in the graph.Thus any optimal solution in the original problem is, by definition, also a solution in the relaxed problem; but the relaxed problem may have better solutions if the added edges provide shortcuts. Hence, the cost of an optimal solution to a relaxed problem is an admissible heuristic for the original problem.


## Local Search and Optimization Problems {#local-search-and-optimization-problems}

Local search algorithms operate by searching from a start state to neighboring states, without keeping track of the paths, nor the set of states that have been reached.

key advantages:

1.  they use very little memory; and
2.  they can often find reasonable solutions in large or infinite state spaces for which systematic algorithms are unsuitable.

Local search algorithms can also solve optimization problems, in which the aim is to
find the best state according to an objective function.(i.e. Find configuration satisfying constraints, e.g., n-queen problem)

If elevation corresponds to an objective function, then the aim is to find the highest peak - a global maximum — and we call the process hill climbing. If elevation corresponds to cost, then the aim is to find the lowest valley — a global minimum — and we call it gradient descent.


### Hill climbing search {#hill-climbing-search}

```latex
function HILL-CLIMBING(problem) returns a state that is a local maximum
 current ← problem.INITIAL
 while true do
   neighbor ← a highest-valued successor state of current
   if VALUE(neighbor) ≤ VALUE(current) then return current
   current ← neighbor
```

It keeps track of one current state and on each iteration moves to the neighboring state with highest value—that is, it heads in the direction that provides the steepest ascent

{{< figure src="/ox-hugo/hill_c.png" caption="<span class=\"figure-number\">Figure 4: </span>A one dimensional state space landscape ." width="500" height="250" target="/blogs" >}}

-   **Local maxima** : A local maximum is a peak that is higher than each of its neighboring

states but lower than the global maximum.

-   **Ridges** : Ridges result in a sequence of local maxima that is very difficult for greedy algorithms to navigate.
-   **Plateaus** : A plateau is a flat area of the state-space landscape. It can be a flat local maximum, from which no uphill exit exists, or a shoulder, from which progress is possible.


### Simulated annealing {#simulated-annealing}

A hill-climbing algorithm that never makes “downhill” moves toward states with lower value
(or higher cost) is always vulnerable to getting stuck in a local maximum.

```latex
function SIMULATED-ANNEALING(problem, schedule) returns a solution state
  current ← problem.INITIAL
  for t = 1 to ∞ do
    T ← schedule(t)
    if T = 0 then return current
    next ← a randomly selected successor of current
    ∆E ← VALUE(current) – VALUE(next)
    if ∆E > 0 then current ← next
    else current ← next only with probability e^{∆E/T}
```

This algorithm escapes the local maxima by allowing some "bad" moves but gradually decrease their frequency.
If T decreases slowly enough, then simulated annealing search will find a global optimum with probability approaching 1 (however, this may take VERY long).


### Local Beam Search {#local-beam-search}

The local beam search algorithm keeps track of k states rather than just one. It begins with k randomly generated states. At each step, all the successors of all k states are generated. If any one is a goal, the algorithm halts. Otherwise, it selects the k best successors from the complete list and repeats.
In a local beam search, useful information is passed among the parallel search threads.


### [genetic algorithms]({{< relref "20210811074451-genetic_algorithms.md" >}}) {#genetic-algorithms--20210811074451-genetic-algorithms-dot-md}


## [Adversarial Search]({{< relref "20210731201337-adversarial_search.md" >}}) {#adversarial-search--20210731201337-adversarial-search-dot-md}
