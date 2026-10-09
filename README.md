# Cryptarithmetic Puzzle Solver (SEND + MORE = MONEY)

Three state-space search approaches (a priority-queue search labelled A*, depth-first search with backtracking, and breadth-first search) applied to the cryptarithmetic puzzle SEND + MORE = MONEY.

**Interactive demo:** [Run it in your browser](https://laiba-khawar-portfolio.vercel.app/work/cryptarithmetic#demo)

## Where the code is

The repository contains a single archive, `i211697_Laibakhawar.zip`, which holds:

- `i211697_Laibakhawar_N.ipynb`: the solver notebook (4 code cells).
- `ASSIGNMENT_1_REPORT (1).pdf`: the written report.

## How it works

The puzzle is written as the string `"SEND + MORE == MONEY"`. A state is a partial dictionary mapping letters to digit characters, with each digit used at most once.

**Cell 1: "A*" search**

- Successors assign a digit to the next unassigned letter only (one letter per level).
- `heuristic = number of letters - number of assigned letters`.
- A successor's priority is `len(assignment) + heuristic`.
- A state is accepted when `eval()` of the translated equation returns `True`. Python rejects integer literals with a leading zero, so assignments that start a word with 0 fail to evaluate.
- An explored set prevents re-expanding identical assignments.

**Cell 2: depth-first search**

Recursive backtracking over the letters, trying digits 0 to 9 for each. The equation is only checked once all 8 letters are assigned (no partial-sum pruning). The check converts each side with `int()`, which accepts leading zeros.

**Cell 3: breadth-first search**

A queue of `(remaining_letters, assignment)` pairs. At each level it branches on every remaining letter and every unused digit, not just on the next letter, so the same assignment is reached through many orderings.

## Results

Recorded outputs in `i211697_Laibakhawar_N.ipynb`:

| Method | Recorded result | Valid? |
| --- | --- | --- |
| "A*" (cell 1) | S=9, E=5, N=6, D=7, M=1, O=0, R=8, Y=2, i.e. 9567 + 1085 = 10652 | Yes |
| DFS (cell 2) | S=2, E=8, N=1, D=7, M=0, O=3, R=6, Y=5, i.e. 2817 + 0368 = 03185 | No: M=0 gives MORE and MONEY a leading zero |
| BFS (cell 3) | No recorded output | Not known |

The interactive demo adds a column-wise constraint search (assigning letters column by column with carries). On SEND + MORE = MONEY it expands 1,773 nodes, compared with 2,606,501 for the plain DFS.

## Running it

The notebook was last run with Python 3.10 and uses only the standard library (`queue`, `collections`).

```bash
git clone https://github.com/LaibaKhawar/Cryptarithmetic-puzzle-solver.git
cd Cryptarithmetic-puzzle-solver
mkdir notebook && unzip i211697_Laibakhawar.zip -d notebook
pip install jupyter
jupyter notebook notebook/i211697_Laibakhawar_N.ipynb
```

Cell 2 reuses the `equation` variable from cell 1, so run the cells in order.

## Known limitations

- **The "A*" search is breadth-first in practice.** Its priority is `len(assignment) + (letters - len(assignment))`, which equals the number of letters (8) for every state. All states tie, the insertion counter breaks the ties, and the queue behaves first-in first-out. The heuristic gives no guidance.
- **DFS ignores the no-leading-zero rule.** Its check uses `int()`, which accepts `0368`, so the recorded solution sets M=0 and is not a valid answer to the puzzle.
- **BFS has no recorded result.** Branching on every remaining letter at every level makes the frontier grow very quickly, and the cell in the notebook has no output.
- None of the methods prune partial assignments using column sums and carries, which is what makes the demo's constraint search far cheaper.
- The puzzle string is hardcoded; there is no input interface.

## Author

[Laiba Khawar](https://github.com/LaibaKhawar) · [LinkedIn](https://www.linkedin.com/in/laiba-k-00b2b1249/)
