Helicopter Acrobatics
=====================

Q1: Why are time indices of the example trajectories treated as an unobserved quantity in the generative model?

A1: By treating time indices a learned parameter, the algorithm is able to time-align example trajectories when inferring the intended trajectory. This is useful in cases where the example trajectories are faster or slower than each other over certain segments.

Q2: Parameters for the simple helicopter dynamics model f(z) (Eq. 1) are learned from flight data collected in level, "normal" flight modes. However, the dynamics of the system in acrobatic flight modes may differ significantly. Explain how the dynamics model used by the LQR controller compensates for nonlinearities in the system when performing maneuvers. How is this done in a computationally efficient way?

A2: The model parameters in can be recomputed in different regimes, using data from the example maneuvers. The LQR controller can be adapted to use the regime-varying dynamics model depending on the helicopter's state, fz(z). While this would typically require a high dimensional lookup table across all regimes, the dynamics model parameters can instead be found as a function of time for each trajectory since the intended trajectory is know. Because of this, the LQR controller can use a time based interpolator for the nonlinear dynamics model, ft(z).

ACAS-Xu
=======

Q1: What are the two ways that ACAS-Xu accounts for complexities in collision scenarios that are not modeled in the MDP formulations. How does this help ACAS-Xu navigate these complexities?  

A1: 1) Online value function modification. This helps ACAS-Xu to not select actions that would be detrimental to the current collision avoidance scenario. Ex: To avoid uncooperative actions with a cooperating intruder. 

2) Minimax trees. These help ACAS-Xu to determine the global action that reduces the expected cost in a multi-intruder scenario. 

Q2: What is the main caveat in the positive test results (Hint: it has to do with the Well Clear region)?  What is a likely reason for this?

A2: Though ACAS-Xu does result in a lower pNMAC and SLoWC, it also results in a higher pLoWC. This is likely because ACAS Xu uses a probabilistic model of collision risk to issue DAA guidance rather than the geometric definition of LoWC.

AlphaDev
========

Q1: How did Alpha-Dev compare to Alpha-Dev-S for fixed and variable sorting algorithms? What impact did the buffer have on these comparisons?

A1: Alpha-Dev-S could not find an optimal solution for fixed sort unless the buffer was filled with a near-optimal solution that matched Alpha-Dev's performance and was more computationally efficient. For variable sort, Alpha-Dev was better in all cases.

Q2: How does the work with Alpha-Dev differ from other efforts for deep-learning code generating? 

A2: Alpha-Dev focuses on creating low-latency code, whereas other work focuses primarily on producing correct code.

NeuroBEM
========

Q1: List 1 Pro and Con for 3 different modeling methods presented in the paper.

A1: 
Simple Quadratic:
Pro:
Simple
Fast computations
Good enough for hover
Con:
Doesn't take [rotors moving through air, rotor-to rotor interactions, rotor-to body interactions] into account
Not good when not near hover

BEM:
Pro:
Accurately model single rotors at high wind velocities 
More accurate than quadratic models for more airflow conditions (Faster Flying, strong shear wind)
Con:
Does not account for any interaction between the flow tubes of different propellers or the frame. 
Needs induced velocity, which is hard to get. That's why Blade element theory is combined with momentum theory, to get the induced velocity.
Increased computation time from quadratic

Parametric Gray Box
Pros:
Can preform well (better than BEM and Quadratic)
Cons:
Requires human expert knowledge (Thus seems to be finicky)

CFD:
Pros:
Very accurate
Cons:
Large computation cost

NN:
Pro:
General model applicable to a lot of things
Can be more accurate than models
Con:
Needs a lot of training data
May over fit
Not robust

Model+NN:
Pro:
Robust characteristics from models
More accurate than model because of NN
Con:
More complex to set up than a single option
Still need training data

Q2: Why would adding a neural network to an already good analytical model help? Why wouldn't you just use a neural network without the model?

A2: The neural network would be able to approximate the residual terms (The components of the outputs that have been approximated away). This would improve the accuracy of the model without adding additional modeling complexity by finding equations that model the system more accurately.

By just using a neural network, you loose robustness. The NN is more likely to not produce accurate values outside the training set, while the model+NN would still produce accurate solutions.

Poaching Prevention with Game Theory
====================================

Q1: What do you think are the major limitations of PAWS-Initial?

A1: There are 4 limitations to it:

1) Ignored topographic information
2) Assumes animal density and relevant problem features at different locations are known and fixed (ignored any uncertainty in it)
3) Doesn't scale to large areas or finer discretization
4) Ignored patrol scheduling constraints

Q2: Can you briefly explain a minimax regret game?

A2: In a minimax regret game, each player considers the potential regrets associated with their choices. Each agent's goal is to minimize the maximum possible regret arising from their decisions while considering uncertainty. Players evaluate various strategies and choose the one that minimizes the maximum regret they might experience, recognizing that they may not have complete information about the outcomes. It's a way of making decisions that consider the worst-case scenario to reduce the potential for regret.

PPO
===

Q1: What is PPO's predecessor? Give two reasons why is PPO is preferred over its predecessor.

A1: PPO is the successor to TRPO.
Reasons PPO is preferable:
 * computational complexity
 * sample efficiency / obtains larger rewards
 * easier to understand code

Q2: What is the most common (generally most performant) policy loss used in PPO? What method is typically used to generate the advantage estimations used in this loss?

A2: PPO typically uses the clipped loss, where $L=E_t[min⁡(r_t A_t,clip(r_t,1−ϵ,1+ϵ) A_t)]$ and $r_t=π(a_t∣s_t)/π_{old}(a_t∣s_t)$ (stating the clipped loss is sufficient). Generalized advantage estimation (GAE) is typically used to calculate the advantages.

Guidance and Nav with RL Meta-Learning
======================================

Q1: What is the reward function a combination of and what form do the shaping rewards take?

A1: The reward function is a combination of shaping rewards and a terminal reward. The authors chose the shaping function to to take the form of a velocity field that maps the lander's position to a target velocity. 

Q2: How do the recurrent layers in the network architecture produce an adaptive agent? How does this differ from a non-recurrent network?

A2: The recurrent layers can evolve differently depending on the observations from the environment and actions output by the policy. The trained policies can capture potentially time varying information such as external forces using. After training, when the recurrent network's weights are frozen, the policy can continue to evolve in response to observations and actions taken. In contrast, a non-recurrent policy's behavior is fixed by the network parameters at test time. 

Soft Actor Critic
=================

Q1: Soft Actor Critic uses maximum entropy Reinforcement learning.

What kind of policies does max-ent RL prefer?

How does a soft policy (Max-Ent) improve training?

A1: Maximum entropy RL will prefer a high entropy (more random) policy than a lower entropy policy for similar policy performance.

Learning high entropy policies smooths learning, and helps avoid getting stuck in local minima.

Q2: Soft Actor Critic is an off-policy algorithm.

What is the advantage of training off-policy?

What drawbacks exist for learning off-policy? How can they be mitigated?

A2: Off-policy algorithms are more sample efficient in general.

Off policy algorithms can suffer from unstableness and catastrophic forgetting.

This can be mitigated by learning a soft policy, and using prioritized replay buffers.

DreamerV3
=========

Q1: How and why are free bits employed in the loss functions of DreamerV3?

A1: Free bits are employed by clipping the dynamics and representation loss below a certain value. By doing so, degenerate solutions where the dynamics are trivial to predict and do not contain enough information about the inputs are avoided.

Q2: What three networks does DreamerV3 consist of and what is the purpose of each part?

A2: - World model
The world model learns Compact representations of sensory inputs through autoencoding and enables planning by predicting future representations.

- Actor network
The actor network learns to choose actions that maximize returns while exploring sufficiently through entropy regularisation.

- Critic network
The critic learns to predict the expected value of the return distribution.

TD7
===

Q1: When using SALE embeddings, there is a state embedding $z^{s}$ and state-action embedding $z^{s,a}$ which are trained according to $L(f,g) = (z^{sa}-|z^{s'}|_{\times})^2$; what is the point of these two embeddings?

A1: By training a state embedding and a state-action embedding such that the error between the state-action embedding and the state embedding of the next state are minimized, the encodings capture the transition dynamics. These embeddings provide additional information to help the state-action value and policy networks to 'predict' the effect of taking a particular action.

Q2: What are the four components added to TD3 to comprise TD7? Which has the most impact on performance?

A2: TD7 consists of TD3 + SALE, LAP, policy checkpoints, and behavior cloning (for offline RL). SALE is the single component with the largest impact on performance.
