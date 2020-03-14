#####<div align="center">Introduction to Reinforcement Learning - Part 2</div>

<br />

###### The RL Framework: The Problem ######

In order to rigorously define a reinforcement learning task, a Markov Decision Process (MDP) is used to model the environment. The MDP specifies the rules that the environment uses to respond to the agent's actions, including how much reward to give to the agent in response to its behavior. The agent's goal is to learn how to play by the rules of the environment, in order to maximize reward.

**Markov Decision Process (MDP)**

In general, the state space *S* is the set of all nonterminal states.

In continuing tasks, this is equivalent to the set of all states.

In episodic tasks, S+ is used to refer to the set of all states, including terminal states.

The action state *A* is the set of possible actions available to the agent.

*A(s)* can be used to refer to the set of actions available in state s in *S*

**Definition:**
<br />

A (finite) Markov Decision Process (MDP) is defined by: <br />
1. a (finite) set of states *S*
2. a (finite) set of actions *A*
3. a (finite) set of rewards *R*
4. the one-step dynamics of the environment
5. a discount rate $\gamma$ $\in$ [0, 1]

###### The RL Framework: The Solution ######
<br/>

The optimal policy $\pi$<sub>\*</sub> specifies - for each environment state - how the agent should select an action towards its goal of maximizing reward. The agent can structure its search for an optimal policy by first estimating the optimal action-value function q<sub>\*</sub>; then once q<sub>\*</sub> is known, $\pi$<sub>\*</sub> can be obtained via argmax.


**Policies**


A deterministic policy is a mapping $\pi$: *S* $\to$ *A*

*A deterministic policy is a mapping that accepts environment state s and returns the action a*

A stochastic policy is a mapping $\pi$: *S* x *A* $\to$ [0,1]

*A stochastic policy is a mapping that accepts environment state s and action a and returns the probability that the agent takes action a while in state s.*

**State-Value Functions**

For each state, the state-value function yields the expected return, if the agent started in that state, and then followed the policy for all time steps.

v<sub>$\pi$</sub> = $\mathbf{E}$<sub>$\pi$</sub>[G<sub>t</sub> | S<sub>t</sub> = s]

where:

v<sub>$\pi$</sub> = value of state s under policy $\pi$  
$\mathbf{E}$<sub>$\pi$</sub> = expected  
G<sub>t</sub> = return  
S<sub>t</sub> = s = if the agent starts in state s  

**Bellman Expectation Equation**

Principle: the value of any state can be calculated as the sum of the immediate reward and the (discounted) value of the next state.

The Bellman Expectation Equation (for v<sub>$\pi$</sub>) expresses the value of any state *s* in terms of the expected reward and the expected value of the next state:

v<sub>$\pi$</sub>(*s*) = $\mathbf{E}$<sub>$\pi$</sub>[R<sub>t+1</sub> + $\gamma$v<sub>$\pi$</sub>(S<sub>t+1</sub>)|S<sub>t</sub> = s]

where:

R<sub>t+1</sub> = the immediate reward  
$\gamma$v<sub>$\pi$</sub>(S<sub>t+1</sub>)|S<sub>t</sub> = s = the discounted value of the state that follows, assuming the agent starts in state s and then uses the policy to choose its actions for all time steps

Note:  
* There are a total of 4 Bellman equations
* All of the Bellman equations attest to the fact that value functions satisfy recursive relationships

**Optimality**

A policy is considered to be better than another policy if and only if the state-value function associated with that policy is equal to or better than the state-value function of the alternative policy for all *s* $\in$ *S*

Note:
* It is often possible to find two policies that cannot be compared

An optimal policy $\pi$<sub>\*</sub> satisfies:  
$\pi$<sub>\*</sub> $\geq$ $\pi$ for all $\pi$

Note:  
* An optimal policy is guaranteed to exist, but may not be unique

The optimal state-value function is denoted v<sub>\*</sub>

**Action-Value Functions**

We call q<sub>$\pi$</sub> the action-value function for policy $\pi$. The value of taking action *a* in state *s* under a policy $\pi$ is:

q<sub>$\pi$</sub>(s, a) = $\mathbf{E}$<sub>$\pi$</sub>[G<sub>t</sub> = s, A<sub>t</sub> = a]

where:

$\mathbf{E}$<sub>$\pi$</sub>[G<sub>t</sub> = s, A<sub>t</sub> = a] = the expected return if the agent starts in state s and then chooses action a

For each state and action, the action-value function yields the expected return, if the agent starts in that state, takes the action, and then follows the policy for all future time steps.

The optimal action-value function is denoted q<sub>\*</sub>

Note:  
* For a deterministic policy $\pi$,

v<sub>$\pi$</sub>(s) = q<sub>$\pi$</sub>(s, $\pi$(s))

holds for all s $\in$ *S*

**Optimal Policies**

Once the agent has determined the optimal action-value function q<sub>\*</sub>, it can obtain an optimal policy $\pi$<sub>\*</sub> by setting:

$\pi$<sub>\*</sub>(s) = argmax<sub>a $\in$ *A*(s)</sub>q<sub>\*</sub>(s,a) for all s $\in$ *S*

In the event that there is some state s $\in$ *S* for which multiple actions a $\in$ *A*(s) maximize the optimal action-value function, an optimal policy can be constructed by placing any amount of probability on any of the (maximizing) actions.
