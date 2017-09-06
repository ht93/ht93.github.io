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
    4. MCTS with policy and value network

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
    1. RL policy network weights $$\rho$$ is initialized to the same values of SL policy network, $$\rho=\sigma$$.
    2. play GO games between the current policy network $$p_{\rho}$$ and a randomly selected previous iteration of the policy network.
    3. At the end of the game, update the network for all previous move $$p_{\rho}(a_t\vert s_t)$$ where t=1:T by $$\Delta\rho\propto\frac{\partial \log p_{\rho}(a_t\vert s_t)}{\partial\rho}z_t$$ where $$z_t$$ is outcome of the game (+1 for winning, -1 for losing).
* Performance: 80% winning rate against SL policy network, 85% against strongest open-source Go program Pachi (previous state-of-art supervised CNN only got 11% wining rate against Pachi)

### Value network
* Objective: position evaluation of any given state `s`
* Task: estimate a value function $$v^p(s)$$ that predicts the outcome from position `s` of games played by using policy p for both players
* Input: state-outcome pairs $$(s,z)$$
* Output: estimated value function $$v^p(s)=\mathbb{E}[z_t\vert s_t=s,a_{t...T}~p]$$
* Network: single prediction output value network $$v_\theta(s)$$ with similar architecture to the previous two policy network and weight $$\theta$$ that $$v_\theta(s)\approx v^{p_\rho}(s)\approx v^*(s)$$
* Training: use state-outcome pairs (s,z) from both KGS dataset and self-play data set with SGD to minimize the MSE between the predicted value $$v_\theta(s)$$ and the corresponding z, ($$\Delta\theta\propto\frac{\partial v_\theta(s)}{\partial\theta}(z-v_\theta(s))$$)
* Performance: 0.226 and 0.234 MSE on training and testing set respectively, near RL policy network performance but 15,000 times less computation.

### MCTS (Searching) with policy and value networks
* Objective: Determine next move action
* Input: state `s`
* Output: estimated optimal move `a`
* Algorithm: The network would run multiple times of simulation for the same input state `s`. The simulation is traversing the tree from the root state (input state `s`). For one simulation (Figure 3):
    1. For the node with children, branch to the child that $$a_t=argmax_a(Q(s_t,a)+u(s_t,a))$$ where $$u(s_t,a))\propto\frac{P(s,a)}{1+N(s,a)}$$
    2. For the leaf node, it may be expanded by SL policy network $$p_{\sigma}$$ and the output probabilities are stored as prior probabilities P for each action.
    3. At the end of a simulation, the leaf node is evaluated in two ways: using the value network $$v_\theta$$; and by running a rollout to the end of the game with the fast rollout policy $$p_{\pi}$$. The outcome is a leaf function $$V(s_L)=(1-\lambda)v_\theta(s_L)+\lambda(z_L)$$.
    4. Update action values $$Q(s,a)=\frac{1}{N(s,a)}\sum_{i=1}^n1(s,a,i)V(s_L^i)$$ where $$N(s,a)=\sum_{i=1}^n1(s,a,i)$$ ($$1(s,a,i)$$ indicates whether an edge (s, a) was traversed during the ith simulation. $$s_L^i$$ is the leaf node from the ith simulation.)
    

