# player construction

The aim of the player class is to construct players with different ways of interacting with the environment and generate observations with controlled randomness. This is done using giving probabilities to a player. 

## basic player
A basic player has these references 

- id
- observation 
- random generator 
- type of player
- reward array
- punishment array

other than these parameters a seed was added where it will always a certain way of randomness every time calls the class in order to keep the robustness.

### observation spectrum
this was constructed into 3 main parts
- gaze
- distance
- task completion 

#### gaze 
this represent do the player maintain the gaze with the robot.

| value | observation |
| --------- | -------------------------------------------------- |
| 0   | not maintaining the observation correctly or out of focus  |
| 1   | maintaining the observation with the robot |

#### distance 

as save as the above in the gaze 

| value | observation |
| --------- | -------------------------------------------------- |
| 0   | not maintaining the observation correctly or out of focus  |
| 1   | maintaining the observation with the robot |


#### task
task was divided into 4 main parts 

| value | observation |
| --------- | ------- |
| 0   | best performance  |
| 1   | average performance |
| 2   | week performance |
| 3   | no performance (not responding) |

# creating a player

in order to create a player player class should be called.

```python

    Player(self,player_id:int=0, player_type =0 , prob = [[0.1, 0.9],
                                                                [0.1, 0.9],
                                                                [0.8, 0.1, 0.1, 0.0]])
```


```python

from players.player3 import Player as Player

player1 = Player()
ret = player1.generate(prob = [[0.1, 0.9],
                        [0.1, 0.9],
                        [0.8, 0.1, 0.1, 0.0]] )

print(ret)

```