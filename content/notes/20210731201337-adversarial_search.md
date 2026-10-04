+++
title = "Adversarial Search"
author = ["felladog"]
date = 2021-07-31T20:13:00-05:00
lastmod = 2026-10-03T14:01:49-05:00
tags = ["AI Survey"]
categories = ["AI"]
draft = true
+++

---

-   References :
    -   [Book] Artificial Intelligence A Modern Approach, Stuart J. Russell and Peter Norvig, Chapter 3

-   Questions :

---

Searching approach for competitive environments like playing games. Optimality depends on the other agents.


### Things to consider in Multi agent environment {#things-to-consider-in-multi-agent-environment}

-   Consider the large number of agents in the aggregate as an economy, allowing us to do things like predict that increasing demand will cause cost to rise, without having to predict the action of any individual agent.
-   Consider adversarial agents as just a part of the environment — a part that makes the environment nondeterministic.
-   Explicitly model the adversarial agents with the techniques of adversarial game-tree search.


## Two Player Zero Sum games {#two-player-zero-sum-games}

The the most commonly studied game within AI are deterministic, two-player, fully observable, perfect information, zero-sum games. Example chess, go.


### zero sum {#zero-sum}

It means what is good for one player is just as bad for the other. The two player can be called MAX and MIN


### Defining the game {#defining-the-game}

-   **S0** : The initial state, which specifies how the game is set up at the start.
-   **TO-MOVE(s)** : The player whose turn it is to move in state s.
-   **ACTIONS(s)** : The set of legal moves in state s.
-   **RESULT(s, a)** : The transition model, which defines the state resulting from taking action a in state s.
-   **IS-TERMINAL(s)** : A terminal test, which is true when the game is over and false otherwise. States where the game has ended are called terminal states.
-   **UTILITY(s, p)** : A utility function (also called an objective function or payoff function), which defines the final numeric value to player p when the game ends in terminal state s. In chess, the outcome is a win, loss, or draw, with values 1, 0, or 1/2.2 Some games have a wider range of possible outcomes—for example, the payoffs in backgammon range from 0 to 192.


## Minimax Value {#minimax-value}

MAX wants to find a sequence of actions leading to a win, but MIN has something to say about it. This means that MAX’s strategy must be a conditional plan specifying a response to each of MIN’s possible moves.

Given a game tree, the optimal strategy can be determined by working out the minimax value of each state in the tree, which we write as MINIMAX(s).

\\( MINIMAX(s) = \begin{cases} UTILITY(s, MAX), & \text{if IS-TERMINAL(s)} \\\\ max\_{a \in Actions(s)} MINIMAX(RESULT(s, a)), & \text{if TO-MOVE(s) = MAX} \\\\ min\_{a \in Actions(s)} MINIMAX(RESULT(s, a)), & \text{if IS-TERMINAL(s) = MIN} \end{cases} \\)


## The minimax search algorithm {#the-minimax-search-algorithm}

We can turn minimax concept into a search algorithm that finds the best move for MAX by trying all actions and choosing the one whose resulting state has the highest MINIMAX value.

It is a recursive algorithm that proceeds all the way down to the leaves of the tree and then backs up the minimax values through the tree as the recursion unwinds.

```latex
function MINIMAX-SEARCH(game, state) returns an action
  player ← game.TO-MOVE(state)
  value, move ← MAX-VALUE (game, state)
  return move

function MAX-VALUE(game, state) returns a (utility, move) pair
  if game.IS-TERMINAL(state) then return game.UTILITY(state, player), null
  v, move ← −∞
  for each a in game.ACTIONS(state) do
    v2, a2 ← MIN-VALUE(game, game.RESULT(state, a))
    if v2 > v then
      v, move ← v2, a
  return v, move

function MIN-VALUE(game, state) returns a (utility, move) pair
  if game.IS-TERMINAL(state) then return game.UTILITY(state, player), null
  v, move ← +∞
  for each a in game.ACTIONS (state) do
    v2, a2 ← MAX-VALUE(game, game.RESULT(state, a))
    if v2 < v then
      v, move ← v2, a
  return v, move
```
