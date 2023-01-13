
# Basic Environment

in this section you will understand of how to create a basic environemnt from scratch using openai gym library

## Creating a custom env

This is the basic environment that was created and in order to create a basic environment and as well
as to maintain it easily the open AI gym library was used to create a custom environment.

Environment contains 5 main parts 

- initialization
- step
- reset
- render
- close


Define the environment's observation and action spaces. The observation space defines the format of the observations that the agent will receive, and the action space defines the format of the actions that the agent can take.

Implement the step() method, which takes an action as an input and returns the next observation, the reward, a Boolean indicating whether the episode has ended, and any additional information.

Optionally, implement the reset() method, which returns the initial observation when the environment is reset.

Optionally, implement the render() method, which displays the current state of the environment.

Create an instance of the Environment class and register it with Gym by calling gym.envs.register()


```python
import gym
import numpy as np
from gym import spaces


class CustomEnv(gym.Env):
    """Custom Environment that follows gym interface."""

    metadata = {"render.modes": ["human"]}

    def __init__(self, arg1, arg2, ...):
        super().__init__()
        # Define action and observation space
        # They must be gym.spaces objects
        # Example when using discrete actions:
        self.action_space = spaces.Discrete(N_DISCRETE_ACTIONS)
        # Example for using image as input (channel-first; channel-last also works):
        self.observation_space = spaces.Box(low=0, high=255,
                                            shape=(N_CHANNELS, HEIGHT, WIDTH), dtype=np.uint8)

    def step(self, action):
        ...
        return observation, reward, done, info

    def reset(self):
        ...
        return observation  # reward, done, info can't be included

    def render(self, mode="human"):
        ...

    def close(self):
        ...
}
```

this was used as the basic structure to create the Nao trainning environment.
