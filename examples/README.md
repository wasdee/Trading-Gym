# Examples

## Trading environment

The trading environment returns at each step a state vector which contains the current price
of the asset as a (bid,ask) tuple, the price at the time the latest action was performed and the current position in the market. The following parameters can be changed:
  1. episodes : number of episodes played.
  2. game_length : number of time step per episode.
  3. trading_fee : cost per action.
  4. time_fee : time penalty for not acting on the market.
  5. history_length : number of historical prices accessible to the agent.

The time series can be observed with its `render` method.

## Random Generator

Returns a list of random prices.


## DQN agent

As an example, we implement a simple value-based Q-learning algorithm that learns to trade a periodic signal. Type `python examples/dqn_agent.py` to launch it. Once the learning process is over, the strategy it learned can be observed graphically from a pop-up window that displays a live time series. Buys (Sells) are embodied by green (red) triangles. Once run, the agent should have learned to buy (sell) at local minima (maxima). Note that the parameters have been set for the agent to learn quickly and the convergence is uncertain. Feel free to run the program several times and to tweak parameters to find different trading strategies.

The attributes of the DQNAgent class are :
  1.  memory_size : number of observations kept in memory for learning
  2.  gamma : decay rate of the reward function.
  3.  epsilon_min : minimum rate of random actions to maintain exploration.
  4.  batch_size : size of the learning batch to take from memory at each learning step
  5.  action_size : number of possible actions (here 3, buy/sell/hold)
  6.  train_interval : number of observation/memory updates before entering the next learning process
  7. learning_rate: learning rate of the gradient descent.

The methods of the DQNAgent class are :

  1. `_build_brain`: builds the neural network that will be used to approximate the optimal action-value function of the agent.
  2. '_get_batches': a helper method that builds vectors of homogeneous data of size `batch_size` from the memory.
  3. 'act': returns a action chosen either randomly, or greedily with the trained action-value function. The probability of choosing a random action is proportional to `epsilon` which starts at 1 and decreases linearly every time the gradient descent is called, until reaching `epsilon_min`.
  4. `observe` which replaces old observations in the memory with the freshest one and launches the gradient descent every `train_interval` steps.

The learning process is as follows. Given an input from the environment, the agent firsts `act`s on the environment by returning an action. It then `observe`s the performance of its action by looking at the reward the environment returns and learns from this experience. This process is repeated until the agent runs out of data.