# Pathfinding Visualizer

A desktop app that visualizes and compares three classic grid pathfinding algorithms — **A\***, **Greedy Best-First Search**, and **Uniform Cost Search (UCS)** — side by side on the same maze. Draw walls, drop a start and end node, hit **Run All**, and watch each algorithm explore the grid in real time, then read off a performance table comparing nodes expanded, path length, and runtime.

Built with Python's standard library only (`tkinter`), so there's nothing to install beyond Python itself.

## Demo

The grid renders on the left and a live performance comparison table builds on the right as each algorithm finishes.

| Algorithm | What it optimizes | Heuristic |
|-----------|-------------------|-----------|
| **A\*** | Shortest path, guided by a heuristic | Manhattan distance, with a cross-product tie-break to keep the search aimed at the goal |
| **Greedy Best-First** | Speed — heads straight for the goal | Manhattan distance only (ignores cost so far) |
| **UCS** | Shortest path, exhaustive | None (expands by accumulated cost, equivalent to Dijkstra on a uniform grid) |

Running all three on the same maze makes the trade-offs visible: Greedy is usually fastest but can return a longer path, while A\* and UCS both return shortest paths with A\* typically expanding far fewer nodes.

## Features

- **Interactive 25×25 grid** — click to place the start and end nodes, click and drag to draw or erase walls.
- **Three algorithms run in sequence** on the same layout for a fair comparison.
- **Animated exploration** — each algorithm's explored cells and final path animate in their own color so you can tell them apart.
- **Adjustable animation speed** via a slider in the toolbar.
- **Performance table** reporting nodes expanded, path length, and time in milliseconds for each algorithm.
- **Reset Grid** to clear everything, or **Clear Paths** to keep your maze and rerun.

## Requirements

- Python 3.x
- Tkinter (bundled with most Python installations)

On most systems Tkinter is already included. If it's missing on Linux, install it with:

```bash
sudo apt-get install python3-tk
```

## Installation & Running

```bash
git clone https://github.com/NehalRauf/Pathfinding-Visualizer.git
cd Pathfinding-Visualizer
python3 "Pathfinding visualizer/main.py"
```

No external packages or virtual environment are required.

## How to Use

1. **Set Start** — click the button, then click any cell to place the green start node.
2. **Set End** — click the button, then click any cell to place the red end node.
3. **Draw Walls** — the default mode; click and drag across the grid to add walls, drag over existing walls to erase them.
4. **Run All** — runs A\*, then Greedy, then UCS, animating each one's search and overlaying the results.
5. Use the **speed slider** to slow down or speed up the animation.
6. **Clear Paths** keeps your maze but wipes the colored search results; **Reset Grid** clears the entire board.

## Color Legend

| Color | Meaning |
|-------|---------|
| Green | Start node |
| Red | End node |
| Dark slate | Wall |
| Light blue / blue | A\* explored / A\* path |
| Pale yellow / orange | Greedy explored / Greedy path |
| Light purple / purple | UCS explored / UCS path |

## How It Works

The code is organized around a few clear pieces:

- **`Node`** — a single grid cell holding its position, `g`/`h`/`f` costs, parent pointer, and wall/start/end flags.
- **`Grid`** — manages the 2D array of nodes, start/end assignment, wall toggling, and neighbor lookup (4-directional movement).
- **`PathfindingAlgorithm`** — an abstract base class providing the shared Manhattan-distance heuristic and path-reconstruction helper. `AStarAlgorithm`, `GreedyAlgorithm`, and `UCSAlgorithm` each implement `solve()`.
- **`VisualizerApp`** — the Tkinter UI: toolbar, canvas grid, legend, animation loop, and results table.

Each `solve()` returns the order cells were explored, the final path, and a stats dictionary, which the app uses to drive the animation and populate the comparison table. All three algorithms use a binary heap (`heapq`) for their frontier and a closed set to avoid re-expanding cells.

## Project Structure

```
Pathfinding visualizer/
└── main.py        # Entire application: algorithms + Tkinter UI
```

## License

Ontario Tech University
