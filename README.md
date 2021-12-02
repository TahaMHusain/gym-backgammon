
<h1 align="center">Backgammon OpenAI Gym</h1> <br>
<p align="center">
   <img alt="Backgammon" title="Backgammon" src="./logo.png" width="350">
</p>

# Table of Contents
- [gym-backgammon](#gym_backgammon)
- [Installation](#installation)
- [Environment](#env)
    - [Observation](#observation)
    - [Actions](#actions)
    - [Reward](#reward)
    - [Starting State](#starting_state)
    - [Episode Termination](#episode_termination)
    - [Reset](#reset)
    - [Rendering](#rendering)
    - [Example](#example)
        - [Play random Agents](#play)
        - [Valid actions](#valid_actions)
- [Backgammon Versions](#versions)
- [Useful links and related works](#useful_links)
- [License](#license)

---
## <a name="gym_backgammon"></a>gym-backgammon
Nannon is a 2-player game that involves both the movement of the checkers and also the roll of the dice. The goal of each player is to move all of his checkers off the board. Nannon is a smaller version of the original game of backgammon.

This repository contains a Nannon game implementation in OpenAI Gym.   
Given the current state of the board, a roll of the dice, and the current player, it computes all the legal actions/moves (iteratively) that the current player can execute.
---
## <a name="installation"></a>Installation
```
git clone https://github.com/dellalibera/gym-backgammon.git
cd gym-backgammon/
pip3 install -e .
```

---
## <a name="env"></a>Environment
The encoding used to represent the state is inspired by the one used by Gerald Tesauro[1].
### <a name="observation"></a>Observation
Type: Box(14)

| Num   | Observation                	 | Min | Max |
| ----- | ----------------------------   | --- | --- |
| 0     | WHITE - HOME			 |  0  |  3  |
| 1     | WHITE - 1st point              |  0  |  1  |
| 2     | WHITE - 2nd point              |  0  |  1  |
| 3     | WHITE - 3rd point              |  0  |  1  |
| 4     | WHITE - 4th point              |  0  |  1  |
| 5     | WHITE - 5th point              |  0  |  1  |
| 6     | WHITE - 6th point              |  0  |  1  |
| 7	| BLACK - HOME			 |  0  |  3  |
| 8	| BLACK - 1st point              |  0  |  1  |
| 9	| BLACK - 2nd point              |  0  |  1  |
| 10	| BLACK - 3rd point              |  0  |  1  |
| 11	| BLACK - 4th point              |  0  |  1  |
| 12	| BLACK - 5th point              |  0  |  1  |
| 13	| BLACK - 6th point              |  0  |  1  |
| 14	| Current player                 |  0  |  1  |


Encoding of the current player:

| Player  | Encoding   |
| ------- | ---------- |
| WHITE   |    [0]     |
| BLACK   |    [1]     |

### <a name="actions"></a>Actions
The valid actions that an agent can execute depend on the current state and the roll of the dice. So, there is no fixed shape for the action space.  

### <a name="reward"></a>Reward
+1 if player WHITE wins, and 0 if player BLACK wins

### <a name="starting_state"></a>Starting State
All the episodes/games start in the same [starting position](#starting_position): 
```

|      |-----------------------|      |
|      |   |   |   |   |   |   |      |
|   O  | O | O |   |   | X | X |  X   |
|      |-------- Board --------|      |
| HOME | 1 | 2 | 3 | 4 | 5 | 6 | HOME | 
```

### <a name="episode_termination"></a>Episode Termination
1. One of the 2 players win the game


### <a name="reset"></a>Reset
The method `reset()` returns:
- the player that will move first (`0` for the `WHITE` player, `1` for the `BLACK` player) 
- the first roll of the dice, a tuple with the dice rolled, i.e `(1,3)` for the `BLACK` player or `(-1, -3)` for the `WHITE` player
- observation features from the starting position


### <a name="rendering"></a>Rendering
If `render(mode = 'rgb_array')` or `render(mode = 'state_pixels')` are selected, this is the output obtained (on multiple steps):
<p align="center">
   <img alt="Backgammon" title="Backgammon" src="./board.gif" width="450">
</p>

---
### <a name="example"></a>Example
#### <a name="play"></a>Play Random Agents
To run a simple example (both agents - `WHITE` and `BLACK` select an action randomly):
```
cd examples/
python3 play_random_agent.py
```
#### <a name="valid_actions"></a>Valid actions
An internal variable, `current player` is used to keep track of the player in turn (it represents the color of the player).   
To get all the valid actions:
```python
actions = env.get_valid_actions(roll)
```

The legal actions are represented as a set of tuples.    
Each action is a tuple of tuples, in the form `((source, target), (source, target))`     
Each tuple represents a move in the form `(source, target)` 

#### NOTE:
The actions of asking a double and accept/reject a double are not available.  

Given the following configuration ([starting position](#starting_position), `WHITE` player in turn, `roll = 2`):
```
|      |-----------------------|      |
|      |   |   |   |   |   |   |      |
|   O  | O | O |   |   | X | X |  X   |
|      |-------- Board --------|      |
| HOME | 1 | 2 | 3 | 4 | 5 | 6 | HOME | 

Current player=1 (O - WHITE) | Roll=(1, 3)
```
The legal actions are:
```
Legal Actions:
(2, 4)
(1, 3)
```
---

## <a name="versions"></a>Backgammon Versions 
### `backgammon-v0`
The above description refers to `backgammon-v0`.

### `backgammon-pixel-v0`
The state is represented by `(96, 96, 3)` feature vector.    
It is the only difference w.r.t `backgammon-v0`.

An example of the board representation:
<p align="center">
   <img alt="raw_pixel" title="raw_pixel" src="./raw_pixel.png" width="150">
</p>


---
## <a name="useful_links"></a>Useful links and related works
- [1][Implementation Details TD-Gammon](http://www.scholarpedia.org/article/User:Gerald_Tesauro/Proposed/Td-gammon)
- [2][Practical Issues in Temporal Difference Learning](https://papers.nips.cc/paper/465-practical-issues-in-temporal-difference-learning.pdf)
<br><br>   
- Rules of Backgammon:
    - www.bkgm.com/rules.html
    - https://en.wikipedia.org/wiki/Backgammon
    - <a name="starting_position"></a>Starting Position: http://www.bkgm.com/gloss/lookup.cgi?starting+position
    - https://bkgm.com/faq/
<br><br>   
- Other Implementation of TD-Gammon and the game of Backgammon:
    - https://github.com/TobiasVogt/TD-Gammon
    - https://github.com/millerm/TD-Gammon
    - https://github.com/fomorians/td-gammon
<br><br>
- Other Implementation of the Backgammon OpenAI Gym Environment: 
    - https://github.com/edusta/gym-backgammon

---
## <a name="license"></a>License
[MIT](https://github.com/dellalibera/gym-backgammon/blob/master/LICENSE) 
