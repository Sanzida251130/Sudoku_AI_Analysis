# Solving Sudoku with A*, Hill Climbing, and Backtracking

**Course:** Artificial Intelligence | **Dataset:** [Kaggle Sudoku (9 million puzzles)](https://www.kaggle.com/datasets/rohanrao/sudoku?select=sudoku.csv)

An experimental comparison of classical AI search and constraint satisfaction algorithms applied to a sampled benchmark of 9×9 Sudoku puzzles. Every algorithm is evaluated on the exact same puzzles using **success rate**, **execution time**, and **search effort (nodes/states explored or evaluated)** across **Easy**, **Medium**, and **Hard** difficulty tiers.

---

## Algorithms Compared

| Algorithm | Paradigm / Family | Core Strategy |
|---|---|---|
| **Backtracking (Plain)** | Constraint Satisfaction (CSP) | Depth-first assignment filling the first empty cell; undoes assignments on constraint violation. |
| **Backtracking + MRV** | Constraint Satisfaction (CSP) | Same depth-first backtracking, but selects the empty cell with the **Minimum Remaining Values** (fewest legal candidates) to detect dead ends earlier. |
| **A\* Search** | Informed Search | Expands partial board states ordered by $f(n) = g(n) + h(n)$, where $g(n)$ is filled cells and $h(n)$ is the sum of candidate counts across empty cells (pruning states with dead-end cells). |
| **Hill Climbing** | Local Search | Randomly completes missing digits in each 3×3 box, then greedily performs intra-box swaps of non-given cells to minimize row/column duplicate cost, with random restarts and step limits. |

---

## Dataset & Experimental Setup

- **Source:** `sudoku.csv` from Kaggle (~9 million puzzle/solution pairs).
- **Sampling & Difficulty Split:**
  - Streams a random **0.3%** sample (`KEEP = 0.003`, `random.Random(42)`) across the dataset, then draws **60 puzzles** (`N_PUZZLES = 60`, `random_state=42`).
  - Blank cells (`0` or `.` normalized to `0`) serve as the difficulty proxy, split into three equal groups of 20 puzzles each:
    - **Easy:** 26–38 blanks (mean: 32.65)
    - **Medium:** 38–43 blanks (mean: 40.70)
    - **Hard:** 43–53 blanks (mean: 46.90)
- **Validity Check:** A run is counted as **solved** only if the algorithm reports success, all given clues are preserved, and every row, column, and 3×3 box contains digits `1–9`.

---

## Results Summary (60 Puzzles)

| Algorithm | Success Rate (%) | Mean Time (s) | Median Time (s) | Max Time (s) | Mean Nodes |
|---|---:|---:|---:|---:|---:|
| **Backtracking** | 100.0% | 0.0042 | 0.0006 | 0.0624 | 722.67 |
| **Backtracking + MRV** | 100.0% | 0.0023 | 0.0017 | 0.0237 | 50.92 |
| **A\*** | 100.0% | 0.0029 | 0.0017 | 0.0620 | 63.00 |
| **Hill Climbing** | 25.0% | 0.0259 | 0.0291 | 0.0550 | 3,481.57 |

### Overall Success Rate (Text View)

- **Backtracking:**       `██████████` 100.0%
- **Backtracking + MRV:** `██████████` 100.0%
- **A\*:**                `██████████` 100.0%
- **Hill Climbing:**      `███░░░░░░░` 25.0%

### Breakdown by Difficulty Tier (Easy / Medium / Hard)

> *Note:* Exact per-tier metrics (`Easy`, `Medium`, and `Hard`—20 puzzles each) are generated when running the notebook cells. Fill in the values from your notebook output table below:

| Difficulty Tier (Blanks) | Algorithm | Success Rate (%) | Mean Time (s) | Mean Nodes |
|---|---|---|---|---:|
| **Easy** (26–38 blanks) | Backtracking | — | — | — |
| | Backtracking + MRV | — | — | — |
| | A\* | — | — | — |
| | Hill Climbing | — | — | — |
| **Medium** (38–43 blanks) | Backtracking | — | — | — |
| | Backtracking + MRV | — | — | — |
| | A\* | — | — | — |
| | Hill Climbing | — | — | — |
| **Hard** (43–53 blanks) | Backtracking | — | — | — |
| | Backtracking + MRV | — | — | — |
| | A\* | — | — | — |
| | Hill Climbing | — | — | — |

### Key Takeaways
- **100% Reliability with Systematic Search & CSP:** Plain Backtracking, Backtracking + MRV, and A\* all solved every puzzle in the sample within milliseconds.
- **Much Lower Search Effort with MRV and A\*:** Adding the **MRV** heuristic reduced average explored assignments from **722.67** to **50.92** (~14× fewer nodes) and cut mean runtime nearly in half. **A\*** also explored very few states (**63.00** mean nodes) with competitive speed.
- **Local Optima Limit Hill Climbing:** Greedy box-swap Hill Climbing (10 restarts, 300 steps per restart) achieved a **25.0%** success rate while evaluating far more candidate swaps (**3,481.57** mean nodes), illustrating how local search can get trapped in non-zero conflict local minima on structured constraint problems like Sudoku.

---

## Getting Started

1. **Clone the repository** and open the Jupyter/Colab notebook.
2. **Download the dataset:** Obtain `sudoku.csv` from [Kaggle Sudoku](https://www.kaggle.com/datasets/rohanrao/sudoku?select=sudoku.csv) and place it in the **same directory** as the notebook.
3. **Install dependencies:**
   ```bash
   pip install numpy pandas matplotlib
