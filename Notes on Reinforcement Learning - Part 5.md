#####<div align="center">Introduction to Reinforcement Learning - Part 5</div>

<br />

###### Reinforcement Learning in Continuous Spaces ######
<br/>

**Strategies to deal with continuous spaces**

1. Discretization
2. Function Approximation

**Discretization**

Discretization is basically converting a continuous space into a discrete space using grids. Grid sizes do not necessarily need to be uniform. Non-uniform discretization (i.e., with varying grid sizes) or very small grid sizes can be used to optimize the state space based on the specific context.

**Tile Coding**

Tile coding is a technique wherein we implement multiple tiles on top of the 2D continuous state space, with each tile slightly offset from the other. By applying this technique, we can then keep track of the tiles that get activated via a Bit value function.

In general, tile coding is a better way of discretizing a continuous space compared to a single grid-based approach. The fundamental idea is to create several overlapping grids or tilings; then for any given sample value, we need only check which tiles it lies in. Thereafter, we can then encode the original continuous value by a vector of integer indices or bits that identifies each activated tile.

Adaptive tile coding takes this concept further by dynamically splitting specific tiles into sub-tiles to further improve the value function. The advantage of adaptive tile coding is that the specific number and size of tiles does not need to be fully determined ahead of time.

**Coarse Coding**

Similar to tile coding, coarse coding uses a similar principle but instead of using tiles, it uses alternative shapes (e.g., circles in 2D) to create layers on the continuous state space.

Coarse coding allows us to choose the extent of generalization we want to introduce (i.e., narrow vs. broad) based on the size of the chosen shape.

In addition, coarse coding can allow us to represent the state using radial basis functions (wherein the distance of the input from the center of the circles is calculated).

**Function Approximation**

When the continuous space is extremely large and complex, the number of discretized grids / tiles can become very large and thus, become not helpful.

As a result, an alternative approach is used called Function Approximation.

In Function Approximation, we look to find a parameterized function that approximates the true value functions: state value function (for prediction) and q-value (for control).

A common intermediate step is to compute a feature vector that is representative of the state x(s)
