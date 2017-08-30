---
layout: post
title: Mastering the game of Go with deep neural networks and tree search
---

Paper: [Nature](https://www.nature.com/nature/journal/v529/n7587/full/nature16961.html)  
For full paper, please Google yourself.

### Key idea:
We introduce a new approach to computer Go that uses ‘value networks’ to evaluate board positions and ‘policy networks’ to select moves.  

### Background knowledge:
* [Ladder (Go terminology)](https://en.wikipedia.org/wiki/Ladder_(Go))


### Previous approach:
* Depth of the search may be reduced by position evaluation: truncating the search tree at state s and replacing the subtree below s by an approximate value function $$v(s)\approxv^*(s)$$ that predicts the outcome from state s.
* The breadth of the search may be reduced by sampling actions from a policy p(a|s) that is a probability distribution over possible moves a in position s.
* Monte Carlo tree search (MCTS) uses Monte Carlo rollouts to estimate the value of each state in a search tree. 
* **However, prior work has been limited to shallow policies or value functions based on a linear combination of input features.**

### This paper approach:
* We use these neural networks to reduce the effective depth and breadth of the search tree: evaluating positions using a value network, and sampling actions using a policy network.
* Structure (Check Figure 1 in paper):
    1. Supervised Learning (SL) policy network $$p_{\epsilon}$$ directly from expert human moves
    2. Reinforcement Learning (RL) policy network $$p_{\pi}$$ that improves the SL policy network by optimizing the final outcome of games of selfplay.
    3. Value network $$v_{\theta}$$ that predicts the winner of games played by the RL policy network against itself.

### Notation:
* `s`: state (current state of the Go game, check Extended Data Table 2 in paper)
* `a`: moves (represent the legal moves)
