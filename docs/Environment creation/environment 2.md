# Nao Basic Environment

The concept is to identify the best possible action by looking at the test subject.
The goal is to keep the engagement with with test subject all the time and ask the best suitable question from the test subject in order to maximize the attention.

there are 5 sets of state sets and in a state set there are 4 states. each state set represent a different question and based on the observation the action must be choose from set current state set or from the next state set. 

as for the demonstration purpose we have created these steps as in a way of representing,

- find child
- name calling 
- with whom did you come
- asking for the favorite color
- picture showing
- final state

and as for the observations we have stated with 3 main parameters 
- gaze
- distance
- task completion 

and the task completion was divided into 4 pars as well
- well finished with the 100% expectance 
- gave an answer with a yes or no by verbally or competed the task with near accuracy of 75%
- answer was given through head nodding or poorly conducted bellow 50%
- task was never completed at all or never tried to complete it

"""
[image should be added here representing the state diagram] 
"""

# state machine

in order to track the progress of the robot a state machine was created. 

## state structure 

a state class was created in order to maintain a proper structure and it included 
> - id number
> - name 
> - what should be the output from the state

this was created to have have with these funtions on it

- observation 
- work that should set when the state was achieved 
- set the child states
- and some other basic functions to get details and set details 

```python
class State:
    def __init__(self, id:int=0, name:str="default", state_out:str="out_default") -> None:
        self.id:int= id
        self.name:str = name
        self.state_out:str = state_out
        self.children: List[State] = []

    def observe(self, gaze:int, distance:int, task:int) -> tuple:
        self.gaze = gaze
        self.distance = distance
        self.task = task
        return self.gaze, self.distance, self.task

    def work(self, fuctions:List[callable]) -> None:
        ret = []
        for fuction in fuctions:
            _ret = fuction()
            ret.append(_ret)
        return ret

    def set_child(self, child):
        if child not in self.children:
            self.children.append(child)
            return True
        else:
            self.children = set(self.children)
            return False    

    def get_child(self):
        return self.children

```



## state set strucure 
this is more similar to state class but this holds the states. 


## 1st environment

By changing how the observation get rewarded will allow us to differentiate the outcome of the action to be selected by the model.

as for the reward structure a reward array was created according to some pre-made moves. 
for an example 

> asd
> asd