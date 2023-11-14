DESPOT
======

Q1: What is the objective of Theorem 4.2 and for what conditions does it guarantee an optimal regularized policy?

A1: The objective here is to show that despite unbounded T-max, the anytime algorithm terminates in ﬁnite time with a near-optimal or optimal regularized policy. If epison_0=0, delta=0, and lambda>1, then the algorithm will terminate in finite time with an optimal regularized policy.

Q2: What are the 3 desirable properties of DESPOT algorithm listed by the authors? If there are theoretic guarantees for these properties please note the theorems.

A2: 1) It is asymptotically sound and complete - Theorem 4.2
2) It is robust against imperfect heuristics - Theorem 4.2
3) It is ﬂexible and allows easy incorporation of domain knowledge

MCVI
====

Q1: How are beliefs 'encoded' into a policy graph?

A1: Beliefs are encoded into a policy graph because at a certain belief b, there is a node in the policy graph that is the most valuable. This node can be found using the algorithm in the paper. Once found, the belief is 'encoded' into this most valuable node. 

Q2: How is the belief tree explored?

A2: Each node in the tree holds an upper and lower bound of what the optimal value of that belief is. To explore the tree, we start at the root node and descend the tree. We select the action that leads to the most value and the observation that leads to the greatest difference in bounds. After exploring, the bounds are updated, so the same selection criteria will likely choose a different path the next time, thus exploring. 

Discriminative Particle Filter RL
=================================

Q1: How does the DPFRL belief update differ from a standard particle filter?

A1: The DPFRL update substitutes both the transition distribution p(s|s_t-1, a_t) and the conditional observation distribution p(o_t, h_t) in the standard particle filter with learned function approximators.

Q2: What is an example of DPFRL performance degrading significantly?

A2: DPFRL loses a large amount of performance in the ChopperCommand natural flickering Atari game, demonstrating that irrelevant observation features are not completely solved by discriminative modeling.

FSSS
====

Q1: Name a feature/quality of FSSS that comes from UCT, and one that comes from Sparse Sampling?

A1: From UCT: Fast in practice by directing search towards high reward branches of tree
From SS: Efficiency guarantees

Q2: Compare the performance of FSSS and KWIK-FSSS to other MBRL algorithms in the paper.

A2: FSSS performs marginally worse than UCT in both experiments. As expected the computation time does not blow up for FSSS (like UCT), as opposed to SS which takes intractably long for larger problems.

Sparse Sampling
===============

Q1: What kind of MDP model is used in this paper and why?

A1: This paper uses a generative model to represent an MDP. A tabular representation is not feasible here since large or infinite state-spaces are considered, and a model with irreversible experience would not work since the algorithm needs to be able to sample states multiple times in order to generate their tree.

Q2: What is the complexity trade-off provided by the sparse sampling algorithm?

A2: The main positive of this algorithm is that the runtime is independent of state-space size. The main downside is that the runtime of this algorithm has an exponential dependence on the horizon time. The trade-off you have to make is thus which of these properties is more important for your application.

Sparse-PFT
==========

Q1: How is Sparse Sampling Omega used to bridge the gap between optimal Q values from POMDPs and their PB-MDP approximations? 

A1: The coupled optimality of Sparse Sampling Omega to the optimal Q values for POMDPs and their PB-MDP approximations proves that the error between the optimal Q values for POMDPs and their PB-MDP approximations is also bounded. 

Q2: Give a general explanation for how an arbitrary MDP planning algorithm's theoretical convergence grantees can extend to POMDPs. 

A2: It can be shown that the error between the error between optimal Q values for POMDPs and their PB-MDP approximations is bounded. Thus, when an MDP planning algorithm is applied to to a POMDP via its PB-MDP approximation, the resulting Q value estimates have error to the POMDP which is simply the linear combination of the error in the optimal Q values for POMDPs and the PB-MDP, and the theoretical convergence guarantees of the MDP algorithm. 

Alpha Zero
==========

Q1: What was the primary difference between AlphaGo Zero and AlphaGo?

A1: AlphaZero is a more generalized form of AlphaGo Zero but with a different state and action representation so that it can be more easily applied to different games.

Q2: How does the AlphaZero utilize neural networks?

A2: The AlphaZero uses neural networks to estimate the value of a leaf node in monte carlo tree search instead of using a Monte Carlo rollout estimate. It also trains a policy network that helps select actions during the tree search.

Q-Learning Efficiency
=====================

Q1: Suggest two ways to design model-free algorithms that are sample-efficient.

A1: The authors have proved that vanilla Q-learning with a UCB exploration policy and an additional bonus term leads to a sample-efficient algorithm.  They showed two different bonus terms for this, which can be obtained using the Hoeffding inequality and the Bernstein inequality.  The bonus term from Bernstein inequality is computationally more expensive to compute but leads to a sqrt(H) factor of improvement for the regret.

Q2: Briefly explain the Probably Approximately Correct (PAC) learning theory.

A2: The PAC learning theory helps analyze whether and under what conditions a learner L will probably output an approximately correct classifier. The learner is approximately correct if the error of L over a distribution D over the inputs is less than a small constant epsilon. The learner is probably approximately correct if L outputs the correct classifier with probability 1-delta where delta lies between 0 and 0.5

Q-Learning Convergence
======================

Q1: What RL algorithm was given its first convergence proof in this paper? Broadly, how was this proved?

A1: On-line TD(lambda) was given convergence criteria for the first time. It was shown to converge by proving the difference between the batch and on-line algorithms vanishes in the limit.

Q2: Some of the algorithms in this paper were previously shown to converge. Given this, what was the reason their convergence was covered in this paper?

A2: This paper introduced a general class of stochastic processes along with convergence criteria for these processes. Both Q-learning and TD(lambda) were shown to be special cases of this general class, and could be analyzed under the same framework.

POMDP Complexity
================

Q1: What does it mean for a problem to be in Nick's Class (NC)?

A1: Nick's Complexity class is the set of problems that can be solved in polylogarithmic time (O(log(n^c))) given n^k parallel computers for some constants c and k. Practically this means that the problem is efficiently parallelizable. NC is  a subset of P.

Q2: What does it mean for a problem to be PSPACE-complete and why is it significant that solving POMDPs is PSPACE complete?

A2: PSPACE is the set of problems that can be solved in polynomial amount of space of the size of the input. All problems in NP and P are assumed to subsets PSPACE. For a problem to be PSPACE complete it must be as hard as any problem in PSPACE. If a problem is PSPACE complete and one was able to come up with an a algorithm that required less than polynomial space then the entire complexity class would be collapsed. POMDPs being in PSPACE practically means that solving POMDPs is an extremally difficult problem, even more so than problems in NP which are notoriously hard. This explains why exactly solving POMDPs in intractable.
