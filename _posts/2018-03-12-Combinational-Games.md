---
layout:     post
title:      "Minimax, Nim Game, Grundy Numbers  (XOR)"
date:       2018-03-10 12:00:00
author:     "Xin"
tags:
    - Algorithms
---



# Minimax, Nim Game, Grundy Numbers  (XOR)

## Example
LC 464. Can I Win
In the "100 game," two players take turns adding, to a running total, any integer from 1..10. The player who first causes the running total to reach or exceed 100 wins.
What if we change the game so that players cannot re-use integers?

LC 486. Predict the Winner
Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.
Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.

## General Idea
1. Assume current function returns if current player could win or not **MiniMax**
2. Recursion-  if any(not self.nextPlayer(potential movements)):  return True/current step    else:    return False
3. Keep visited state for efficiency

## Special Case




### LC 292. Nim Game
You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

> The first one who got the number that is multiple of 4 (i.e. n % 4 == 0) will lost, otherwise he/she will win.

### Nim Game2
What if there are multiple heaps?
* Given heaps of size n1, n2, ..., n<sub>m</sub>
* The first player wins iff n1 ^ n2 ^ ... ^ nm != 0

### Playing multiple games at once
Suppose that multiple games are played at the same time. At
each turn, the player chooses a game and make a move. You
lose if there is no possible move. We want to determine the
winner.
* For each game, we compute its Grundy number
* The first player wins iff XOR of all the Grundy numbers is nonzero
### How to calculate grundy number?
* Let S be a state, and T1, T2, ... Ts be states can be reached from S using a single move
* The Grundy number g(S) is **the smallest nonnegative integer that doesn't appear in {g(T1), g(T2), ..., g(Tm)}**
    * Grundy number of a **losing state** is 0
    
### Example one-pile one-step nim game
g(0) = 0, because lose <br>
g(1) = 1, because 0 is the only reachable state <br>
similarly, g(2) = 0, g(3) = 1, ... <br>
### Example one-pile two-step nim game
g(0) = 0, because lose<br>
g(1) = 1, next can only be 0 <br>
g(2) = 2, next can be 0 or 1 <br>
g(3) = 0, next can be 1 or 2 <br>

**Hints**:
* If the state space is small, use memorization
* If not, print out result of the games for small test data and look for a pattern
* If multiple games are played at once, use Grundy numbers
  

### LC 294. Flip Game II
You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Initial Time Complexity:  T(N) = (N-2) * T(N-2) = (N-2) * (N-4) * T(N-4) ... = (N-2) * (N-4) * (N-6) * ... ~ O(N!!)

[Can we do better?](https://leetcode.com/problems/flip-game-ii/discuss/73954/Theory-matters-from-Backtracking(128ms)-to-DP-(0ms)).  Following part from the article.

>Now we understand the the flip game is an impartial game under normal play. Luckily, this type of game has been extensively studied. Note that our following discussion only applies to normal impartial games.


>**Concept 3 (Sprague-Grundy Function):** Suppose x represents a particular
arrangement of board, and x_0, x_1, x_2, … ,x_k represent the board
after a valid move, then we define the Sprague-Grundy function as:

> g(x) = FirstMissingNumber(g(x_0), g(x_1), g(x_2), ... , g(x_k)). 
where FirstMissingNumber(y) stands for the smallest positive number
that is not in set y. For instance, if g(x_0) = 0, g(x_1) = 0, g(x_k) =
2, then g(x) = FMV({0, 0, 2}) = 1.

>**Theorem 1:** If g(x) != 0, then 1P must have a guaranteed winning move
from board state x. Otherwise, no matter how 1P moves, 2P must then
have a winning move.

>So our task now is to calculate g(board). But how to do that? Let’s first of all find a way to numerically describe the board. Since we can only flip ++ to --, then apparently, we only need to write down the lengths of consecutive ++'s of length >= 2 to define a board. For instance ++--+--++++-+ will be represented as (2, 4)

>(2, 4) has two separate ‘+’ subsequences. Any operation made on one subsequence does not interfere with the state of the other. Therefore, we say (2, 4) consists of two subgames: (2) and (4).

>Okay now we are only one more theorem away from the solution. This is the last theorem. I promise:

>Theorem 2 (Sprague-Grundy Theorem): The S-G function of game x = (s1,
s2, …, sk) equals the XOR of all its subgames s1, s2, …, sk. e.g.
g((s1, s2, s3)) = g(s1) XOR g(s2) XOR g(s3).

>With the S-G theorem, we can now compute any arbitrary g(x). If x contains only one number N (there is only one ‘+’ subsequence), then

>g(x) = FMV(g(0, N-2), g(1, N-3), g(2, N-4), ... , g(N/2-1, N-N/2-2));
>     = FMV(g(0)^g(N-2), g(1)^g(N-3), g(2)^g(N-4)), ... g(N/2-1, N-N/2-2));

>Now we have the whole algorithm:

>Convert the board to numerical representation: x = (s1, s2, ..., sk)
Calculate g(0) to g(max(si)) using DP.
if g(s1)^g(s2)^...^g(sk) != 0 return true, otherwise return false.


```python
import re
def canWin(s):
    g, G = [0], 0
    print(re.split('-+', s))
    print(s.split('-'))
    for p in map(len, re.split('-+', s)):
        while len(g) <= p:
            g += min(set(range(p)) - {a^b for a, b in zip(g, g[-2::-1])}),
            print ('g',  g)
        G ^= g[p]
    return G > 0

print("Note the first index is 0......")
print("For multiple heap Nim games, its the same because each heap would have the same properties !!!!!!")
s = '++--+--++++-+'
print(canWin(s))
s = '++'
print(canWin(s))

```

    Note the first index is 0......
    For multiple heap Nim games, its the same because each heap would have the same properties !!!!!!
    ['++', '+', '++++', '+']
    ['++', '', '+', '', '++++', '+']
    g [0, 0]
    g [0, 0, 1]
    g [0, 0, 1, 1]
    g [0, 0, 1, 1, 2]
    True
    ['++']
    ['++']
    g [0, 0]
    g [0, 0, 1]
    True

## Alpha-beta pruning
Alpha–beta pruning is a search algorithm that seeks to decrease the number of nodes that are evaluated by the minimax algorithm in its search tree. It is an adversarial search algorithm used commonly for machine playing of two-player games (Tic-tac-toe, Chess, Go, etc.). <br>

alpha: best already explored option along path to the root for maximizer <br>
beta: ................................................................for minimizer
