# AI Projects

Portfolio of four AI/ML algorithms, each implemented from scratch and running live in the browser — no backend, no ML/algorithm libraries.

**Live site:** [https://kmb-ai-projects.netlify.app/](https://kmb-ai-projects.netlify.app/)

I originally wrote these as standalone Python scripts (for Columbia CS coursework). I later rewrote every project from scratch in JavaScript so I could give them a frontend and ship them as an interactive website instead of CLI scripts.

## Stack

Static site, no build step, no package manager, no framework.

- Each page (`2048.html`, `npuzzle.html`, `sudoku.html`, `transformer.html`) is a single self-contained HTML file with the algorithm implemented in an inline `<script>`.
- `style.css` is shared across all pages.
- `index.html` is the landing page linking out to each demo.

Run locally with any static file server, e.g.:

```
python3 -m http.server
```

## Projects

### 2048 — Expectiminimax Agent (`2048.html`)

Adversarial search agent that plays 2048 against the game's random tile spawner.

- **Expectiminimax** search — chance nodes for the random tile placement (90% chance of `2`, 10% chance of `4`), max nodes for the agent's move choice.
- **Iterative deepening** — searches progressively deeper until a time budget is exhausted, so the agent always returns a move even under time pressure.
- **Alpha-beta pruning** on the max/min branches to cut the search space.
- 6-component board evaluation heuristic: empty cell count, corner placement of the max tile, monotonicity, merge potential, max tile value, and smoothness.
- Two search modes toggle in the UI: probabilistic expectimax (true chance nodes, configurable 2/4 weights) vs. adversarial minimax (worst-case opponent).

### N-Puzzle Solver (`npuzzle.html`)

Solves the sliding-tile N-puzzle (8-puzzle / 15-puzzle) with a choice of six search strategies, visualized in real time:

- **A\*** and **Greedy Best-First** (Manhattan distance heuristic)
- **BFS**, **DFS**, **IDS** (iterative deepening search), **UCS**
- Solvability check before search (parity of inversions) so unsolvable boards fail fast instead of exhausting the search space.

### Sudoku — CSP Solver (`sudoku.html`)

Sudoku as a constraint satisfaction problem:

- **Backtracking search** over cell assignments.
- **MRV (minimum remaining values) heuristic** for variable ordering — picks the most-constrained empty cell first.
- **Forward checking** to prune domains of neighboring cells after each assignment, detecting dead ends early.
- Step-by-step solver trace with backtrack count, playable live against a bundled puzzle set.

### Transformer (`transformer.html`)

Encoder-decoder Transformer trained live in-browser on a toy sequence-copy task, with backpropagation implemented by hand (no autograd).

- Multi-head self-attention and cross-attention, positional encoding, encoder/decoder blocks.
- Manual forward and backward passes, Adam optimizer, cross-entropy loss.
- Toggleable Pre-LN vs. Post-LN residual/normalization variant and a dropout control, so architectural choices can be compared live during training.

## Repo layout

```
index.html        landing page + nav
style.css          shared styles (theme vars, fonts: Orbitron / Rajdhani / JetBrains Mono)
2048.html          2048 expectiminimax agent
npuzzle.html        N-puzzle solver (A*, BFS, DFS, IDS, UCS, Greedy)
sudoku.html          Sudoku CSP solver
transformer.html    in-browser Transformer training
```

Each page is independent — there's no shared JS module system, so any logic common to multiple pages is duplicated per file rather than imported.
