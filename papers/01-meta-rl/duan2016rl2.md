# RL²: Fast Reinforcement Learning via Slow Reinforcement Learning

Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel · arXiv 2016 · [paper](https://arxiv.org/abs/1611.02779)

An RNN policy that receives observation, action, reward and termination at every step and keeps its hidden state across episodes, so a slow outer RL loop shapes the recurrent weights into a fast inner learning algorithm. Adaptation is the hidden state itself, with no gradient step at deployment.
