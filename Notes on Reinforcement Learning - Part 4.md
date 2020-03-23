#####<div align="center">Introduction to Reinforcement Learning - Part 4</div>

<br />

###### Temporal-Difference Methods ######
<br/>

**Overview of TD Control**

Monte Carlo (MC) control methods update the Q-table after completing an entire episode of interaction. In contrast, the Temporal Difference (TD) method update the Q-table after every time step.

**Sarsa**

This is one method for TD control.

In this algorithm, the agent:  
* Takes the action A<sub>t</sub> (from the current state S<sub>t</sub>) that is $\epsilon$-greedy with respect to the Q-table,
* Receives the reward R<sub>t+1</sub> and next state S<sub>t+1</sub>
Chooses the next action A<sub>t+1</sub> (from the next state S<sub>t+1</sub>) that is $\epsilon$-greedy with respect to the Q-table,
* Uses the information in the tuple (S<sub>t</sub>, A<sub>t</sub>, R<sub>t+1</sub>, S<sub>t+1</sub>, A<sub>t+1</sub>) to update the entry Q(S<sub>t</sub>, A<sub>t</sub>) in the Q-table corresponding to the current state S<sub>t</sub> and the action A<sub>t</sub>

Equation:

Q(S<sub>t</sub>, A<sub>t</sub>) $\leftarrow$ Q(S<sub>t</sub>, A<sub>t</sub>) + $\alpha$(R<sub>t+1</sub> + $\gamma$Q(S<sub>t+1</sub>, A<sub>t+1</sub>) - Q(S<sub>t</sub>, A<sub>t</sub>))

**Q-Learning (or Sarsamax)**

This is a second method for TD control.

In this algorithm, the agent selects the greedy action (as opposed to $\epsilon$-greedy action) for the next time step.

Equation:

Q(S<sub>t</sub>, A<sub>t</sub>) $\leftarrow$ Q(S<sub>t</sub>, A<sub>t</sub>) + $\alpha$(R<sub>t+1</sub> + $\gamma$max<sub>a $\in$ A </sub> Q(S<sub>t+1</sub>, a) - Q(S<sub>t</sub>, A<sub>t</sub>))

**Expected Sarsa**

This is a third method for TD control.

It uses the expected value of the next state-action pair, where the agent takes into account the probability of selecting each action step in the next state.

Equation:

Q(S<sub>t</sub>, A<sub>t</sub>) $\leftarrow$ Q(S<sub>t</sub>, A<sub>t</sub>) + $\alpha$(R<sub>t+1</sub> + $\gamma$$\sum$<sub>a $\in$ A</sub> $\pi$(a|S<sub>t+1</sub>)Q(S<sub>t+1</sub>, a) - Q(S<sub>t</sub>, A<sub>t</sub>))

**Optimism**

It has been shown that initializing the estimates of the values in the Q-table to large values can improve performance.

For instance, if all of the possible rewards that can be received by the agent are negative, then initializing every estimate in the Q-table to zeros is a good technique. In this case, we refer to the initialized Q-table as optimistic, since the action-value estimates are guaranteed to be larger than the true action values.

**Difference across the three TD algorithms**

The differences are summarized below:  
* Sarsa and Expected Sarsa are both *on-policy* TD control algorithms. In this case, the same ($\epsilon$-greedy) policy that is evaluated and improved is also used to select actions.
* Sarsamax is an *off-policy* method, where the (greedy) policy that is evaluated and improved is different from the ($\epsilon$-greedy) policy that is used to select actions.
* On-policy TD control methods have better online performance that off-policy TD control methods.
* Expected Sarsa generally achieves better performance that Sarsa.
