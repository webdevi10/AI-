# AI-
# AI Practical Programs --- Code Explanations & Viva Preparation


##Contents

1.  A\* Search --- 8-Puzzle
2.  Alpha-Beta Pruning
3.  Tower of Hanoi
4.  Sudoku Solver --- Backtracking and Constraint Propagation
5.  Breadth-First Search (BFS) --- 8-Puzzle
6.  8-Queens --- Backtracking
7.  Rule-Based Chatbot
8.  Intelligent Agent --- Number Guessing
9.  Quick comparison of similar algorithms
10. Last-minute viva revision

------------------------------------------------------------------------

# 1. A\* Search --- 8-Puzzle

**Aim:** Solve the 8-puzzle using the A\* search algorithm.

## What the problem is

The 8-puzzle has eight numbered tiles and one empty space on a 3 × 3
board. The program moves the empty space up, down, left, or right until
the board matches the goal state. In this program, `0` represents the
empty space.

The supplied initial state is:

``` text

1 2 3

0 4 6

7 5 8

```

The goal state is:

``` text

1 2 3

4 5 6

7 8 0

```

## How the code works

### `heuristic(state)`

Calculates the **Manhattan distance** of the numbered tiles from their
goal positions. It adds each tile's horizontal distance and vertical
distance. The empty tile (`0`) is ignored.

The heuristic estimates how far the board is from the goal; it does not
count the empty space as a numbered tile.

### `get\\\\\\\_neighbors(state)`

Finds the location of `0`, tests the four possible directions, rejects
moves outside the board, swaps `0` with a valid adjacent tile, and
returns the resulting states.

### `astar(initial, goal)`

A\* uses a priority queue from Python's `heapq` module. Each queue entry
stores:

-   `f`: estimated total cost
-   `g`: cost (number of moves) already taken
-   `state`: current board
-   `path`: states along the route so far

A\* uses this formula:

**`f(n) = g(n) + h(n)`**

-   `g(n)` is the cost from the initial state to the current state.
-   `h(n)` is the estimated remaining cost, calculated by the heuristic.
-   `f(n)` is the estimated total cost through the current state.

`heapq.heappush()` inserts a candidate, and `heapq.heappop()` removes
the entry with the smallest priority. The algorithm explores promising
states, checks whether the goal has been reached, and reports the
solution path and move count.

## Key terms

-   **Informed search:** Search guided by extra information, such as a
    heuristic.
-   **Heuristic:** An estimate of the remaining cost to reach the goal.
-   **Priority queue:** A structure that selects the entry with the
    lowest priority value in this implementation.
-   **Manhattan distance:** Sum of horizontal and vertical tile
    distances from their goal positions.

## Expected viva questions

**Q1. What is A\* search?**\
A. A search algorithm that uses the cost so far and a heuristic estimate
to guide the search.

**Q2. What is the A\* formula?**\
A. `f(n) = g(n) + h(n)`.

**Q3. What do `g(n)` and `h(n)` mean?**\
A. `g(n)` is the cost so far; `h(n)` estimates the cost remaining.

**Q4. Which Python module is used for the priority queue?**\
A. `heapq`.

**Q5. Which heuristic does this program use?**\
A. Manhattan distance.

**Q6. Why is `0` excluded from the heuristic?**\
A. It represents the empty space, not a numbered tile.

**Q7. Is A\* informed or uninformed search?**\
A. Informed search.

**Q8. What is one advantage of A\* over BFS?**\
A. A\* uses a heuristic to prioritize states that appear closer to the
goal, which can reduce unnecessary exploration.

**Q9. Does A\* always find the optimal path?**\
A. It does when the search implementation and heuristic satisfy the
required conditions; Manhattan distance is an admissible heuristic for
the standard 8-puzzle.

------------------------------------------------------------------------

# 2. Alpha-Beta Pruning

**Aim:** Find the optimal value in a game tree using Alpha-Beta Pruning.

## What the problem is

Alpha-Beta Pruning is an optimization of the **Minimax** game-tree
search. The MAX player tries to obtain the highest value, while the MIN
player tries to obtain the lowest value. Alpha-Beta Pruning skips
branches that cannot affect the final decision.

The supplied leaf values are:

``` python

values = \\\\\\\[3, 5, 6, 9, 1, 2, 0, -1]

```

The program uses a depth-3 tree and prints the optimal value.

## How the code works

### `alphabeta(depth, node\\\\\\\_index, max\\\\\\\_player, values, alpha, beta)`

This recursive function evaluates the game tree.

-   `depth`: current depth in the tree
-   `node\\\\\\\_index`: index of the current node
-   `max\\\\\\\_player`: indicates whether this level is a MAX level
-   `values`: values stored at the leaf nodes
-   `alpha`: best value MAX can guarantee so far
-   `beta`: best value MIN can guarantee so far

### Base condition

When `depth == 3`, the function has reached a leaf and returns
`values\\\\\\\[node\\\\\\\_index]`. This ends that recursive branch.

### MAX level

MAX initializes its best value to negative infinity, evaluates child
nodes, and keeps the largest returned value. It updates alpha using the
best value found.

### MIN level

MIN initializes its best value to positive infinity, evaluates child
nodes, and keeps the smallest returned value. It updates beta using the
best value found.

### Pruning condition

The code checks:

``` python

if beta <= alpha:

\\\&#x20;   print("Pruning at depth", depth)

\\\&#x20;   break

```

When `beta <= alpha`, the remaining children of that node cannot change
the choice already available to the relevant ancestor. The code stops
examining those children.

## Key terms

-   **Minimax:** MAX tries to maximize the outcome; MIN tries to
    minimize it.
-   **Alpha:** Best value MAX can guarantee so far.
-   **Beta:** Best value MIN can guarantee so far.
-   **Pruning:** Skipping branches that cannot affect the final result.
-   **Recursion:** A function calling itself.

## Expected viva questions

**Q1. What is Alpha-Beta Pruning?**\
A. An optimization technique for Minimax that skips unnecessary
game-tree branches.

**Q2. Where is it used?**\
A. Game-playing AI, such as search for possible moves in board games.

**Q3. What does MAX do?**\
A. Selects the maximum value.

**Q4. What does MIN do?**\
A. Selects the minimum value.

**Q5. What is alpha?**\
A. The best value MAX can guarantee so far.

**Q6. What is beta?**\
A. The best value MIN can guarantee so far.

**Q7. What is the pruning condition in this program?**\
A. `beta <= alpha`.

**Q8. Does pruning change the optimal Minimax result?**\
A. No. It reduces the number of nodes examined while preserving the
optimal result.

**Q9. Why is a base condition required?**\
A. To stop recursion at the leaf level and return a leaf value.

**Q10. Why is `math` imported?**\
A. To use positive and negative infinity when initializing best values.

------------------------------------------------------------------------

# 3. Tower of Hanoi

**Aim:** Solve the Tower of Hanoi problem using recursion.

## What the problem is

Tower of Hanoi uses three rods and a set of disks of different sizes.
The task is to move all disks from the source rod to the destination
rod, using the auxiliary rod when needed.

The standard rules are:

1.  Move only one disk at a time.
2.  Only the top disk of a rod can be moved.
3.  A larger disk must never be placed on a smaller disk.

The program asks the user for the number of disks and prints the
sequence of moves from rod `A` to rod `B`, using rod `C` as auxiliary.

## How the code works

### `Tower\\\\\\\_of\\\\\\\_hanoi(n, source, destination, auxiliary)`

The function solves the problem recursively.

### Base condition

``` python

if n == 1:

\\\&#x20;   print(f"Move disk 1 from {source} to {destination}")

\\\&#x20;   return

```

If there is only one disk, it can be moved directly from the source to
the destination. The `return` ends this function call.

### Recursive steps

For more than one disk, the function:

1.  Moves `n - 1` disks from the source to the auxiliary rod.
2.  Moves the largest disk from the source to the destination.
3.  Moves the `n - 1` disks from the auxiliary rod to the destination.

The final part reads the number of disks using `input()` and calls the
function with rods `A`, `B`, and `C`.

For `n` disks, the minimum number of moves is **`2^n - 1`**. For
example, 3 disks require 7 moves.

## Expected viva questions

**Q1. What is the aim of this program?**\
A. To solve Tower of Hanoi using recursion.

**Q2. What is recursion?**\
A. When a function calls itself to solve a smaller version of the same
problem.

**Q3. What is the base condition?**\
A. When `n == 1`, move the disk directly to the destination and return.

**Q4. Why is an auxiliary rod needed?**\
A. It temporarily holds disks while the other disks are moved.

**Q5. How many rods are used?**\
A. Three: source, destination, and auxiliary.

**Q6. What is the minimum number of moves for `n` disks?**\
A. `2^n - 1`.

**Q7. How many moves are required for 3 disks?**\
A. `2^3 - 1 = 7`.

**Q8. What is the time complexity?**\
A. `O(2^n)`, because the number of moves grows exponentially.

**Q9. What happens if the input is 1?**\
A. The base condition runs and prints one move.

**Q10. What input should be used for this code?**\
A. A positive integer. The supplied code does not validate zero or
negative input.

------------------------------------------------------------------------

# 4. Sudoku Solver --- Backtracking and Constraint Propagation

**Aim:** Solve a 9 × 9 Sudoku using backtracking and constraint
propagation.

## What the problem is

A Sudoku board is a 9 × 9 grid. The goal is to fill empty cells so that
every row, column, and 3 × 3 box contains the digits 1 through 9 without
repetition.

In the supplied program, `0` represents an empty cell.

## How the code works

### `print\\\\\\\_board(board)`

Displays the board row by row and uses separators to make the 3 × 3
boxes easier to see.

### `get\\\\\\\_candidates(board, row, col)`

Finds which digits can legally be placed in an empty cell.

It collects the values already present in the cell's row, column, and 3
× 3 box. It then subtracts those used values from the set of digits 1
through 9.

The result is the set of candidate digits. If the cell is already
filled, the function returns an empty set.

### `constraint\\\\\\\_propagation(board)`

Repeatedly scans empty cells and calculates their candidates.

-   If a cell has exactly one candidate, that value is placed in the
    cell.
-   If an empty cell has no candidates, the board has reached a
    contradiction and the function returns `False`.
-   If a full scan makes no more changes, the function returns `True` to
    allow the solver to continue.

### `solve(board)`

This function combines propagation with backtracking.

1.  Apply constraint propagation.
2.  If a contradiction is found, return `False`.
3.  Find an empty cell.
4.  If no empty cell remains, the board is solved, so return `True`.
5.  Try each candidate in a copied board.
6.  Recursively solve that copy.
7.  If a candidate leads to a solution, copy the solved rows back and
    return `True`.
8.  If no candidate works, return `False` so the previous recursive call
    can try another candidate.

This is **backtracking**: try a choice, continue solving, and undo that
choice when it leads to a contradiction.

## Key terms

-   **Candidate:** A digit that can legally be placed in an empty cell.
-   **Constraint propagation:** Using Sudoku rules to fill cells whose
    values are forced.
-   **Backtracking:** Trying a candidate and returning to an earlier
    choice when it fails.
-   **Contradiction:** An empty cell has no legal candidate.
-   **Recursive search:** Repeatedly applying the same solving process
    to a smaller unresolved problem.

## Expected viva questions

**Q1. What is the aim of this program?**\
A. To solve Sudoku using backtracking and constraint propagation.

**Q2. What does `0` represent?**\
A. An empty cell.

**Q3. What is a candidate?**\
A. A digit that can legally be placed in a particular empty cell.

**Q4. How are candidates calculated?**\
A. By removing the digits already used in the cell's row, column, and 3
× 3 box from the set 1--9.

**Q5. What is constraint propagation?**\
A. Applying constraints to determine values that are forced, such as a
cell with only one candidate.

**Q6. What is backtracking?**\
A. Trying a possible value and undoing that choice if it does not lead
to a solution.

**Q7. Why does the solver copy the board?**\
A. So that an unsuccessful candidate attempt does not corrupt the board
used by the previous recursive call.

**Q8. What happens when a cell has no candidates?**\
A. A contradiction has been found, so the current attempt fails.

**Q9. What is the base case for a solved board?**\
A. No empty cells remain, so `solve()` returns `True`.

**Q10. Why is this program more complex than Tower of Hanoi?**\
A. It combines candidate calculation, constraint propagation, board
copying, and recursive backtracking.

------------------------------------------------------------------------

# 5. Breadth-First Search (BFS) --- 8-Puzzle

**Aim:** Solve the 8-puzzle using Breadth-First Search.

## What the problem is

The initial and goal boards are the same 8-puzzle states described in
Program 1. The program searches for a sequence of valid moves that
transforms the initial board into the goal board.

## How the code works

### `from collections import deque`

Imports `deque`, which is used as a queue.

### `get\\\\\\\_neighbors(state)`

Finds the empty space (`0`) and generates every valid next board by
moving it up, down, left, or right. Moves outside the 3 × 3 board are
rejected.

### `bfs(initial)`

The main search function initializes three data structures:

-   `queue = deque(\\\\\\\[initial])`: states waiting to be explored
-   `visited = {initial}`: states already discovered
-   `parent = {initial: None}`: records the state that led to each state

The algorithm repeatedly removes the oldest state with `popleft()`.

-   If it is the goal state, the solution path is printed.
-   Otherwise, the algorithm generates its neighbours.
-   Every neighbour not already visited is marked visited, assigned a
    parent, and added to the queue.

If the queue becomes empty before the goal is found, the program reports
that no solution exists.

### `print\\\\\\\_path(parent, goal)`

Reconstructs the path by starting at the goal and following parent links
back to the initial state. It reverses the path and prints the boards in
the correct order.

The move count is `len(path) - 1` because the path includes the initial
state, which is not itself a move.

## Key terms

-   **BFS:** Breadth-First Search; explores states level by level.
-   **FIFO:** First In, First Out.
-   **Queue:** Holds states waiting to be explored.
-   **Visited set:** Prevents states from being repeatedly added to the
    queue.
-   **Parent dictionary:** Allows the path to be reconstructed.

## Expected viva questions

**Q1. What is BFS?**\
A. A search algorithm that explores states level by level.

**Q2. Which data structure does BFS use?**\
A. A queue.

**Q3. What does FIFO mean?**\
A. First In, First Out.

**Q4. Why does the code use `deque`?**\
A. It provides efficient removal from the front of the queue.

**Q5. Why is a visited set needed?**\
A. To avoid repeatedly exploring the same state.

**Q6. What does the parent dictionary store?**\
A. The predecessor or parent of each discovered state.

**Q7. Why does `print\\\\\\\_path()` reverse the path?**\
A. The path is reconstructed backward from the goal to the initial
state.

**Q8. Does BFS find a shortest solution?**\
A. Yes, when every move has equal cost.

\*\*Q9. What is the difference between BFS and A\*?\*\*\
A. BFS explores level by level without a heuristic; A\* prioritizes
states using `g(n) + h(n)`.

**Q10. Why is the move count `len(path) - 1`?**\
A. The path contains both the initial state and each state reached after
a move.

------------------------------------------------------------------------

# 6. 8-Queens --- Backtracking

**Aim:** Place eight queens on an 8 × 8 chessboard so that no two queens
attack each other.

## What the problem is

In chess, a queen can attack along its row, column, and diagonals. The
program finds a placement of eight queens in which no two queens share
one of these attack lines.

## How the code works

### `N = 8`

Sets the board size to 8 × 8.

### `print\\\\\\\_board(board)`

Prints `Q` for a square containing a queen and `.` for an empty square.

### `is\\\\\\\_safe(board, row, col)`

Checks whether placing a queen at a proposed position is safe.

The program checks: - The row to the left - The upper-left diagonal -
The lower-left diagonal

It only checks to the left because queens are placed one column at a
time, from left to right. Any queens already placed must therefore be in
earlier columns.

If any check finds a queen, the position is unsafe. Otherwise, it is
safe.

### `solve\\\\\\\_queens(board, col)`

Places one queen in each column using backtracking.

1.  If `col >= N`, all columns have been filled, so return `True`.
2.  Try each row in the current column.
3.  If the position is safe, place a queen there.
4.  Recursively try to place a queen in the next column.
5.  If the recursive call succeeds, return `True`.
6.  If it fails, remove the queen and try another row.
7.  If no row works, return `False`.

Removing the queen after an unsuccessful attempt is the backtracking
step.

## Expected viva questions

**Q1. What is the aim of the 8-Queens problem?**\
A. Place eight queens so that no two queens attack each other.

**Q2. Which technique is used?**\
A. Backtracking.

**Q3. What does `is\\\\\\\_safe()` do?**\
A. Checks whether a queen can be placed at a position without attacking
an existing queen.

**Q4. Why are only left-side directions checked?**\
A. The algorithm fills the board from left to right, so previously
placed queens are only in earlier columns.

**Q5. What is the base condition?**\
A. When `col >= N`, all eight columns have been filled successfully.

**Q6. Why is a queen removed from the board?**\
A. If that placement does not lead to a solution, it is undone so
another row can be tried.

**Q7. What does `N = 8` mean?**\
A. The board has eight rows and eight columns, and eight queens must be
placed.

**Q8. What is backtracking?**\
A. Making a choice, exploring it, and undoing it when it does not lead
to a solution.

**Q9. Why does the program place one queen per column?**\
A. This design guarantees that no two queens share a column.

**Q10. What is the difference between a safe position and a solution?**\
A. A safe position does not conflict with queens already placed; a
solution successfully places all eight queens.

------------------------------------------------------------------------

# 7. Rule-Based Chatbot

**Aim:** Implement a simple chatbot that responds to user messages using
predefined rules.

## What the problem is

The chatbot matches user messages to patterns and chooses a
corresponding response. It does not train a machine-learning model or
understand language in the same way a large language model does.

## How the code works

### Imports

``` python

import re

import random

```

-   `re` supports regular-expression pattern matching.
-   `random` lets the chatbot choose randomly among possible responses.

### Rules

The rules pair message patterns with lists of responses. The supplied
rules cover greetings, names, help, weather, well-being, goodbye
messages, and a fallback response.

In a regular expression, `|` means OR. For example, `hi|hello|hey`
represents alternatives.

### `respond(user\\\\\\\_input)`

This function processes a message:

1.  Preserves the original input where needed.
2.  Converts the input to lowercase for case-insensitive matching.
3.  Checks the input against each predefined pattern using
    `re.fullmatch()`.
4.  If a rule matches, selects one of its responses using
    `random.choice()`.
5.  Uses captured text for certain personalized replies.
6.  Returns a fallback response if no pattern matches.

**Important detail:** `re.fullmatch()` requires the pattern to match the
entire input. A pattern like `hi|hello|hey` matches exactly `hi`,
`hello`, or `hey`, but not `hello there` unless the pattern is written
to allow the extra words.

### `chat()`

Runs the conversation loop. It accepts a message, calls `respond()`,
displays the reply, and exits when a goodbye/exit condition is detected.

### Main guard

``` python

if \\\\\\\_\\\\\\\_name\\\\\\\_\\\\\\\_ == "\\\\\\\_\\\\\\\_main\\\\\\\_\\\\\\\_":

\\\&#x20;   chat()

```

Starts the conversation when the file is executed directly.

## Expected viva questions

**Q1. What is a rule-based chatbot?**\
A. A chatbot that matches user input against predefined rules and
returns predefined responses.

**Q2. Which libraries are used?**\
A. `re` and `random`.

**Q3. What is a regular expression?**\
A. A pattern used to match or search text.

**Q4. What does `re.fullmatch()` do?**\
A. Checks whether the pattern matches the entire input string.

**Q5. What does `random.choice()` do?**\
A. Selects one item randomly from a sequence.

**Q6. What is a fallback response?**\
A. The default response used when none of the patterns match.

**Q7. Does this program use machine learning?**\
A. No. It uses predefined rules and patterns.

**Q8. What is one limitation of the chatbot?**\
A. It can respond poorly to messages that are not covered by its rules.

**Q9. What is the purpose of `chat()`?**\
A. To manage the conversation loop and handle exit conditions.

**Q10. What does `if \\\\\\\_\\\\\\\_name\\\\\\\_\\\\\\\_ == "\\\\\\\_\\\\\\\_main\\\\\\\_\\\\\\\_":` do?**\
A. Runs the chatbot when the file is executed directly.

------------------------------------------------------------------------

# 8. Intelligent Agent --- Number Guessing

**Aim:** Implement an intelligent agent that guesses a secret number by
narrowing the search range.

## What the problem is

The program chooses a secret integer from 1 to 100. The agent repeatedly
guesses a number and narrows its range based on whether the guess is too
low or too high. It uses the midpoint of the remaining range, similar to
**binary search**.

## How the code works

### `import random`

Used to generate the secret number randomly.

### `NumberGuessingAgent` class

The class represents the guessing agent.

#### `\\\\\\\_\\\\\\\_init\\\\\\\_\\\\\\\_(self, low=1, high=100)`

Initializes the current search range. By default, the agent searches
from 1 to 100.

#### `guess(self)`

Returns the midpoint of the current range:

``` python

(low + high) // 2

```

The `//` operator performs integer division.

#### `update(self, feedback)`

Adjusts the search range according to the feedback:

-   If the guess is too high, reduce the upper bound.
-   If the guess is too low, increase the lower bound.

This removes numbers that can no longer be the secret.

#### `play(self, secret\\\\\\\_number)`

Repeats the process:

1.  Make a guess.
2.  Increase the attempt count.
3.  Check the guess against the secret number.
4.  Return the number of attempts if correct.
5.  Otherwise, update the range and try again.

If the range becomes invalid without finding the number, the method
reports failure.

### Main guard

When the file is executed directly, it generates a secret number with
`random.randint(1, 100)`, creates the agent, and runs the game.

## Key terms

-   **Agent:** A program that observes information and takes actions
    toward a goal.
-   **Binary search:** Repeatedly halves a sorted search range.
-   **Feedback:** Information that tells the agent how to adjust its
    next guess.
-   **Search range:** The lowest and highest values that remain
    possible.

## Expected viva questions

**Q1. What is the aim of this program?**\
A. To create an agent that guesses a secret number by narrowing the
possible range.

**Q2. Which library is used?**\
A. `random`.

**Q3. What is an intelligent agent in this example?**\
A. The `NumberGuessingAgent` object, which makes guesses and updates its
search range using feedback.

**Q4. Which strategy does the agent use?**\
A. A midpoint strategy similar to binary search.

**Q5. What does `\\\\\\\_\\\\\\\_init\\\\\\\_\\\\\\\_()` do?**\
A. Initializes the agent's search range.

**Q6. What does `//` mean?**\
A. Integer division.

**Q7. What happens when a guess is too high?**\
A. The upper limit of the search range is reduced.

**Q8. What happens when a guess is too low?**\
A. The lower limit of the search range is increased.

**Q9. Why does the program count attempts?**\
A. To record how many guesses were made before finding the secret
number.

**Q10. What is the benefit of guessing the midpoint?**\
A. With correct feedback and range updates, it removes a large part of
the remaining possibilities after each guess.

------------------------------------------------------------------------

# 9. Quick comparison of similar algorithms

## BFS vs A\*

  -----------------------------------------------------------------------
  BFS                                 A\*
  ----------------------------------- -----------------------------------
  Uses a queue.                       Uses a priority queue.

  Explores level by level.            Prioritizes states by
                                      `g(n) + h(n)`.

  Does not need a heuristic.          Uses a heuristic.

  Finds a shortest path when all      Can find an optimal path when the
  moves have equal cost.              heuristic and implementation meet
                                      the required conditions.
  -----------------------------------------------------------------------

## Backtracking examples

  -----------------------------------------------------------------------
  Program                 What is tried?          When does it backtrack?
  ----------------------- ----------------------- -----------------------
  8-Queens                A row for the next      When a placement cannot
                          queen                   lead to a complete
                                                  solution

  Sudoku                  A candidate digit for   When the choice leads
                          an empty cell           to a contradiction

  Tower of Hanoi          Smaller subproblems in  It uses recursion, but
                          a recursive sequence    does not perform
                                                  trial-and-error
                                                  backtracking
  -----------------------------------------------------------------------

## Search vs pruning

  -----------------------------------------------------------------------
  Concept                             Meaning
  ----------------------------------- -----------------------------------
  A\*                                 Searches using path cost plus a
                                      heuristic

  BFS                                 Searches states level by level

  Alpha-Beta Pruning                  Cuts off branches in a Minimax game
                                      tree

  Constraint propagation              Applies rules to force values
                                      before or during search
  -----------------------------------------------------------------------

------------------------------------------------------------------------

#ALGORITHM
PART A

1.  A* ALGORITHM - 8 PUZZLE

2.  Start.

3.  Set the initial state and goal state of the puzzle.

4.  Calculate the heuristic value using Manhattan distance.

5.  Insert the initial state into the priority queue.

6.  Remove the state with the lowest cost from the queue.

7.  If the state is the goal state, display the solution path and total
    moves.

8.  Find the empty space (0) and generate possible moves.

9.  Calculate the new cost for each neighbouring state.

10. Add the neighbouring states to the priority queue.

11. Repeat until the goal is found or the queue becomes empty.

12. If the queue is empty, display no solution.

13. Stop.

14. ALPHA-BETA PRUNING

15. Start.

16. Set the leaf node values.

17. Set alpha to negative infinity and beta to positive infinity.

18. Check whether the current node is a leaf node.

19. If it is a leaf node, return its value.

20. If it is a MAX node, select the maximum value and update alpha.

21. If it is a MIN node, select the minimum value and update beta.

22. If beta is less than or equal to alpha, prune the remaining
    branches.

23. Repeat until the best value is found at the root node.

24. Display the optimal value.

25. Stop.

26. TOWER OF HANOI

27. Start.

28. Accept the number of disks n.

29. Define the Tower of Hanoi function.

30. If n == 1, move the disk directly from source to destination.

31. Move n-1 disks from source to auxiliary.

32. Move the largest disk from source to destination.

33. Move n-1 disks from auxiliary to destination.

34. Repeat until all disks reach the destination.

35. Stop.

36. SUDOKU SOLVER

37. Start.

38. Initialize the Sudoku board.

39. Find the possible numbers for each empty cell by checking its row,
    column and 3x3 box.

40. If a cell has only one possible number, fill it.

41. Repeat constraint propagation while cells are being filled.

42. If an empty cell has no possible number, return failure.

43. Select an empty cell and try one of its possible numbers.

44. Apply backtracking if the selected number does not lead to a
    solution.

45. Repeat until the Sudoku is solved.

46. Display the solved Sudoku board.

47. Stop.

PART B

5.  BREADTH-FIRST SEARCH (BFS) - 8 PUZZLE

6.  Start.

7.  Set the initial state and goal state of the puzzle.

8.  Insert the initial state into the queue.

9.  Mark the initial state as visited.

10. Remove the first state from the queue.

11. If the state is the goal state, display the solution path.

12. Find the empty space (0) and generate its possible moves.

13. Add unvisited neighbouring states to the queue and mark them as
    visited.

14. Repeat until the goal is found or the queue becomes empty.

15. If the queue is empty, display no solution.

16. Stop.

17. 8-QUEENS USING BACKTRACKING

18. Start.

19. Create an empty 8x8 chessboard.

20. Define the function to check whether a queen’s position is safe.

21. Place a queen in the current column if the position is safe.

22. Move to the next column and repeat the process.

23. If no safe position is available, remove the previously placed
    queen.

24. Try another position using backtracking.

25. Repeat until all 8 queens are placed safely.

26. Display the chessboard.

27. Stop.

28. RULE-BASED CHATBOT

29. Start.

30. Import the required modules.

31. Define predefined patterns and responses.

32. Accept a message from the user.

33. Convert the message to lowercase.

34. Compare the message with the predefined patterns.

35. If a pattern matches, select and display a suitable response.

36. If no pattern matches, display the default response.

37. If the user enters bye, goodbye, exit or quit, end the conversation.

38. Stop.

39. INTELLIGENT AGENT - NUMBER GUESSING

40. Start.

41. Generate a random secret number between 1 and 100.

42. Create the number guessing agent with lower and upper limits.

43. Calculate the guess using the middle value of the range.

44. Compare the guess with the secret number.

45. If the guess is correct, display the number of attempts.

46. If the guess is lower than the secret number, increase the lower
    limit.

47. If the guess is higher than the secret number, decrease the upper
    limit.

48. Repeat until the agent guesses the correct number.

49. Stop.
