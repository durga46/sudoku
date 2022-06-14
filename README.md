# Sudoku Solver
## AIM:
To develop a code to solve a given sudoku puzzle.

## THEORY:
We create a python program to solve a given sudoku puzzle by using Elimination and Only Choice strategies.
Firstly, we form the puzzle itself, either by getting data in String form or by indexes and values, from the user.
Then we fill the empty boxes with all the possible values, 1-9. We iteratively apply Elimination and Only Choice Strategies to finally end up with a result.

## DESIGN STEPS:
STEP 1:
We define the puzzle. We initialize those boxes with only one value, with the data provided by the user, either in String format or in the Index & value format.

STEP 2:
We identify the Row units, Column units, and the square units. And then we form a unit list using the prior data.

STEP 3:
Two Dictionaries with units and peers of each boxes is defined.

STEP 4:
We reduce the puzzle using the two strategies.

STEP 5:
Search function is defined to find the final solution to a given sudoku puzzle.


## PROGRAM:
```python
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
import time

rows = 'ABCDEFGHI'
cols = '123456789'

def cross(a,b):
    return [i+j for i in a for j in b]
boxes=cross(rows,cols)
ru=[cross(r,cols) for r in rows]
cu=[cross(rows,c) for c in cols]
su=[cross(rs,cs) for rs in ('ABC','DEF','GHI') for cs in('123','456','789')]
ul=ru+cu+su
units = dict((s, [u for u in ul if s in u])  for s in boxes)
peers = dict((s, set(sum(units[s],[]))-set([s]))for s in boxes)
def display(values):
    width = 1+max(len(values[s]) for s in boxes)
    line = '+'.join(['-'*(width*3)]*3)
    for r in rows:
        print(''.join(values[r+c].center (width)+('|' if c in '36' else '') for c in cols))
        if r in 'CF': print(line)
    return
def grid_values_improved(grid):
    values = []
    all_digits = '123456789'
    for c in grid:
        if c == '.':
            values.append(all_digits)
        elif c in all_digits:
                values.append(c)
    assert len(values) == 81
    boxes = cross(rows,cols)
    return dict(zip(boxes,values))    
def elimination(values):
    solved_values = [box for box in values.keys() if len(values[box])==1]
    for box in solved_values:
        digit = values[box]
        for peer in peers[box]:
            values[peer] = values[peer].replace(digit,'')
    return values
def only_choice(values):
    for unit in ul:
        for digit in '123456789':
            dplaces = [box for box in unit if digit in values[box]]
            if len(dplaces) == 1:
                values[dplaces[0]] = digit
    return values    
def reduce_puzzle(values):
    stalled =False
    while not stalled:
        solved_values_before = len([box for box in values.keys() if len(values[box])==1])
        elimination(values)
        only_choice(values)
        solved_values_after = len([box for box in values.keys() if len(values[box])==1])
        stalled = solved_values_after == solved_values_before
        if len([box for box in values.keys() if len(values[box])==1])==0:
            return False
    return values    
def search(values):
    values_reduced = reduce_puzzle(values)
    if not values_reduced:
        return False
    else:
        values=values_reduced
    if len([box for box in values.keys() if len(values[box])==1])==81:
        return values   
    possibility_count_list = [(len(values[box]),box) for box in values.keys() if len(values[box])>1]    
    possibility_count_list.sort()
    for(_,t_box_min) in possibility_count_list:
        for i_digit in values[t_box_min]:
            new_values = values.copy()
            new_values[t_box_min]=i_digit
            new_values = search(new_values)
            if new_values:
                return new_values           
    return values

            
    return values
    p='3..8.1..22.1.3.6.4...2.4...8.9...1.6.6.....5.7.2...4.9...5.9...9.4.8.7.56..1.7..3'


start_time = time.time()
p1=grid_values_improved(p)
print("Before solving Sudoku Puzzle")
print("\n")
display(p)
result = search(p1)
print("\n\n")
print("After solving Sudoku Puzzle")
display(result)
time_taken=time.time() - start_time
print("\n\n\nThe time required to solve the sudoku puzzle : {0} seconds".format(time_taken))
``` 
## OUTPUT:
![k1](https://user-images.githubusercontent.com/75235704/173546697-0ea8b4ce-2d2c-4068-8db6-3a7b796dfde9.png)

![k2](https://user-images.githubusercontent.com/75235704/173546710-9e52c39c-eca5-40d5-888c-e64c00dfd7d9.png)



## RESULT:
Hence, a python program has been developed to solve a given sudoku puzzle.
