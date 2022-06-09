# Sudoku

## Aim:
To develop a code to solve a sudoku puzzle using contraint propagation

## Theory:
Sudoku consists of a 9x9 grid, and the objective is to fill the grid with digits in such a way that each row, each column, and each of the 9 principal 3x3 subsquares contains all of the digits from 1 to 9.

## Design Steps:
### Step 1:
Take an unsolved sudoku puzzle and make it has a single string format.
### Step 2:
Convert given string format into dictionary format.
### Step 3:
Eliminate possible values for a box by looking at its peers.
### Step 4:
Check whether any box which allows only a certain digit in the unit after elimination.
### Step 5:
Repeat 3 and 4 until we get the solved puzzle.
### Step 6:
Calculate the time taken to solve the sudoku.

# <br><br><br>Program:
## Developed By : S.Sumyuktha Rani
## Register Number : 212220230050

```
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
def grid_values(grid):
    assert len(grid) == 81, "Input grid must be a string length of 81 (9x9)"
    boxes = cross(rows,cols)
    return dict(zip(boxes,grid))
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
    
    p='...4.723.2.....5..83.5.....4.....3.2....2....1.8.....9.....9.21..5.....4.927.1...'
start_time = time.time()
display(grid_values(p))
p1=grid_values_improved(p)
print("\n\n")
display(p1)
result = search(p1)
print("\n\n")
display(result)
time_taken=time.time() - start_time
print("\n\n{0} seconds".format(time_taken))

```





## Output:

![WhatsApp Image 2022-06-08 at 9 07 31 PM](https://user-images.githubusercontent.com/75235818/172763005-39434a27-bb94-4d5c-bb13-5a2145068676.jpeg)

## Result:
Thus, a program to solve sudoku puzzle using constraint propagation is implemented successfully.
    
