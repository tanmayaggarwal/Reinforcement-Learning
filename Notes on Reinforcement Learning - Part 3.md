#####<div align="center">Introduction to Reinforcement Learning - Part 3</div>

<br />

###### Monte Carlo Methods ######
<br/>

The objective is to understand how an agent can use information from individual episodes to improve its strategy (i.e., identify the optimal policy $\pi$<sub>\*</sub>)

To truly understand the environment, the agent needs more episodes because:
1. The agent hasn't attempted each action from each state
2. The environment's dynamics are stochastic

Monte Carlo approach conceptualized:  
* When the agent has a policy in mind, it follows the policy to collect a lot of episodes. Then, for each state, to figure out which action is best, the agent can look for which action tended to result in the most cumulative reward.

Monte Carlo method helps identify a better policy relative to a bad policy (e.g., equiprobable random policy).

A Q-Table is a table that stores action-value function results (q<sub>$\pi$</sub>) for corresponding state-action pairs (s, a).

Note:  
* If the agent follows a policy for many episodes, we can use the results to directly estimate the action-value function corresponding to the same policy.
* The Q-table is used to estimate the action-value function.

**MC Prediction**

Prediction problem:  
Given a policy, how might the agent estimate the value function for that policy?

Note: the prediction problem can refer to both action-value function as well as the state-value function.

Monte Carlo (MC) approaches to the prediction problem are referred to as *MC prediction methods.*

There are two different versions of MC prediction, depending on how the special case is handled where - in a single episode - the same action is selected from the same state many times.

Every occurrence of a state in an episode is defined as a *visit* to that state-action pair. In the event that a state-action pair is visited more than once in an episode, there are two options:

*Option 1: Every-visit MC Prediction*
Average the returns following every visit to each state-action pair, in all episodes.

*Option 2: First-visit MC Prediction*
For each episode, we only consider the first visit to the state-action pair.

Notes:  
* Every-visit MC is noted to be biased, whereas first-visit MC is noted to be unbiased.
* Initially, every-visit MC has lower mean squared error (MSE), but as more episodes are collected, first-visit MC attains better MSE.


**Epsilon-Greedy Policies**

$\epsilon$-greedy policies overview:  

Greedy Policy always selects the greedy action.
In contrast, Epsilon-Greedy Policy most likely selects the greedy action.

An agent who follows an $\epsilon$-greedy policy has a (potentially unfair) coin at its disposal, with probability $\epsilon$ of landing heads.

After observing a state, the agent flips the coin.  
* If the coin lands tail (i.e., with probability 1-$\epsilon$), the agent selects the greedy action.
* If the coin lands heads, the agent selects an action uniformly at random from the set of available (non-greedy and greedy) actions.

As such, if $\epsilon$ = 0, the $\epsilon$-greedy policy is guaranteed to always select the greedy action.

If $\epsilon$ = 1, the $\epsilon$-greedy policy is equivalent to a equiprobable random policy.

As long as $\epsilon$ > 0, the agent has a non-zero probability of selecting any of the available actions.

**MC Control**

Control problem: Estimate the optimal policy.

Monte Carlo control method / algorithm:

If the agent alternates between the following two steps:  
* Step 1 ("policy evaluation"): use policy $\pi$ to construct the Q-Table
* Step 2 ("policy improvement"): improve the policy by changing it to be $\epsilon$-greedy with respect to the Q-Table

The agent will eventually obtain the optimal policy $\pi$<sub>\*</sub>

In summary, the Monte Carlo control method alternates between policy evaluation and policy improvement steps to recover the optimal policy $\pi$<sub>\*</sub>

**Exploration-Exploitation Dilemma**

A successful RL agent cannot act greedily at every time step (i.e., it cannot always *exploit* its knowledge). Instead, in order to discover the optimal policy, it has to continue to refine the estimated return for all state-action pairs (i.e., it has to continue to *explore* the range of possibilities by visiting every state-action pair).

The need to balance these two competing requirements is known as the Exploration-Exploitation Dilemma.

One potential solution is implemented by gradually modifying the value of $\epsilon$ when constructing $\epsilon$-greedy policies.

* At earlier time steps, when the agent knows relatively little about the environment's dynamics, it makes sense for the agent to favor exploration over exploitation (e.g., by using an equiprobable random policy by setting $\epsilon$ = 1)
* At later time steps, it makes sense to favor exploitation over exploration, where the policy gradually becomes more greedy with respect to the action-value function estimate. Setting $\epsilon$ = 0 yields the greedy policy.

**Greedy in the Limit with Infinite Exploration (GLIE)**

In order to guarantee that MC control converges to the optimal policy $\pi$<sub>\*</sub>, two conditions need to be met:  
* Every state-action pair s,a is visited infinitely many times, and
* The policy converges to a policy that is greedy with respect to the action-value function estimate Q

These two conditions are referred to as "Greedy in the Limit with Infinite Exploration (GLIE)"

These conditions ensure that:  
* The agent continues to explore for all time steps, and
* The agent gradually exploits more (and explores less)

Both of these conditions are met if:  
* $\epsilon$<sub>i</sub> > 0 for all time steps i, and
* $\epsilon$<sub>i</sub> decays to 0 in the limit as the time step i approaches infinity

**Setting the value of $\epsilon$, in Practice**

The challenge with GLIE is that it doesn't specify how long it might take before the optimal policy is identified.

Thus, in practice, even though convergence is not guaranteed, better results can be achieved by either:  
* Using fixed $\epsilon$, or
* Letting $\epsilon$<sub>i</sub> decay to a small positive number, like 0.1

**Incremental Mean**

In this approach, the Q-table is updated after every episode (as compared to waiting till after a large number of episodes have been completed), and the updated Q-table is used to improve the policy. The new policy can then be used to generate the next episode, and so on.

The algorithm proceeds as follows:  
* Step 1: the policy $\pi$ is improved to be $\epsilon$-greedy with respect to Q, and the agent uses $\pi$ to collect an episode
* Step 2: N is updated to count the total number of first visits to each state action pair
* Step 3: The estimates in Q are updated to take into account the most recent information

The estimated action value (Q) after each episode can be calculated using the following formula:

Q $\leftarrow$ Q + $\frac{1}{N}$(G - Q)

where:  
Q = estimated action value  
N = episode number   
G = return at that episode

**Constant Alpha**

In the above equation, as N gets really large, the impact of the second term starts becoming negligible (i.e., the impact of the most recent episodes' action value returns continues to diminish). However, given the policy is getting updated after every episode, we want to give more weight to the most recent episodes' action value returns.

As a result, it is better to replace $\frac{1}{N}$ with a constant term $\alpha$.

Thus, the updated equation used to amend the values in the Q-table is as follows:

Q(S<sub>t</sub>, A<sub>t</sub>) $\leftarrow$ Q(S<sub>t</sub>, A<sub>t</sub>) + $\alpha$(G<sub>t</sub> - Q(S<sub>t</sub>, A<sub>t</sub>))

This equation can be rewritten as:

Q(S<sub>t</sub>, A<sub>t</sub>) $\leftarrow$ (1 - $\alpha$)Q(S<sub>t</sub>, A<sub>t</sub>) + $\alpha$G<sub>t</sub>

Some guiding principles when setting the value of $\alpha$:  
* Always set the value for $\alpha$ to a number greater than zero and less than (or equal to) 1
* If $\alpha$ = 0, then the action-value function estimate is never updated by the agent
* If $\alpha$ = 1, then the final value estimate for each state-action pair is always equal to the last return that was experienced by the agent (after visiting the pair)
* Smaller values for $\alpha$ encourage the agent to consider a longer history of returns when calculating the action-value function estimate. Increasing the value of $\alpha$ ensures that the agent focuses more on the most recently sampled returns.

The best value of $\alpha$ is best gauged through trial-and-error.
