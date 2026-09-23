# RL$^2$: Fast Reinforcement Learning via Slow Reinforcement Learning

Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel · arXiv 2016 (arXiv comment: 'Under review as a conference paper at ICLR 2017'; no proceedings publication found) · [paper](https://arxiv.org/abs/1611.02779)

An RNN policy that receives observation, action, reward and termination at every step and keeps its hidden state across episodes, so a slow outer RL loop shapes the recurrent weights into a fast inner learning algorithm. Adaptation is the hidden state itself, with no gradient step at deployment.
