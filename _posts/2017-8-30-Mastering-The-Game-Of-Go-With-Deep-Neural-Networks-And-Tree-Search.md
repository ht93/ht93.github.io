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
* Depth of the search may be reduced by position evaluation: truncating the search tree at state s and replacing the subtree below s by an approximate value function $$v(s)\approx v^*(s)$$ that predicts the outcome from state s.
* The breadth of the search may be reduced by sampling actions from a policy $$p(a\vert s)$$ that is a probability distribution over possible moves a in position s.
* Monte Carlo tree search (MCTS) uses Monte Carlo rollouts to estimate the value of each state in a search tree. 
* **However, prior work has been limited to shallow policies or value functions based on a linear combination of input features.**

### This paper approach:
* We use these neural networks to reduce the effective depth and breadth of the search tree: evaluating positions using a value network, and sampling actions using a policy network.
* Structure (Check Figure 1 in paper):
    1. Supervised Learning (SL) policy network $$p_{\epsilon}$$ directly from expert human moves
    2. Reinforcement Learning (RL) policy network $$p_{\rho}$$ that improves the SL policy network by optimizing the final outcome of games of selfplay.
    3. Value network $$v_{\theta}$$ that predicts the winner of games played by the RL policy network against itself.

### Notation:
* `s`: state (current state of the Go game, check Extended Data Table 2 in paper)
* `a`: moves (represent the legal moves)

### Supervised Learning (SL) policy network
* Task: predicting the probability of human experts next moves `a` given the current state `s`
* Input: state `s`
* Output: probability distribution of all legal moves `a`
* Structure: formed by Convolutional layer and rectifier nonlinearities, softmax as final output.
* Detailed network: 13 layer
* Data: 30 million state-action pairs from KGS Go server.
* Performance: `57.0%` using all input features, `55.7%` using only raw board position and move history as inputs.
* Time consumption: `3ms`
* Extra: a faster rollout policy $$p_{\pi}(a\vert s)$$ with `24.2%` and $$2\mu s$$

### Reinforcement Learning (RL) policy network
* Objective: improving the policy network by policy gradient reinforcement learning (RL)
* Network: RL policy network $$p_{\rho}$$ is identical in structure to SL policy network
* Training procedure: 
    1. RL policy network weights $$\rho$$ is initialized to the same values of SL policy network, $$\rho=\epsilon$$.
    2. play GO games between the current policy network $$p_{\rho}$$ and a randomly selected previous iteration of the policy network.
    3. At the end of the game, update the network for all previous move $$p_{\rho}(a_t\vert s_t)$$ where t=1:T by $$\Delta\rho\propto\frac{\partial \log p_{\rho}(a_t\vert s_t)}{\partial\rho}z_t$$ where $$z_t$$ is outcome of the game (+1 for winning, -1 for losing).
* Performance: 80% winning rate against SL policy network, 85% against strongest open-source Go program Pachi (previous state-of-art supervised CNN only got 11% wining rate against Pachi)


