# Shortpath

## FR

L'objectif est de trouver la longueur du chemin pour aller de l'entrée du labyrinthe (en haut à gauche) à la sortie (en bas à droite) ! 
Attention car malgré l'affichage qui est trompeur le labyrinthe à un format carré
shortpath.flag4all.sh port 2000

Auteur : MAYO (ESDAcademy - ENI Rennes - RESD05)

## Solve

Using Astar implementation from https://www.geeksforgeeks.org/shortest-path-in-a-binary-maze/
And ChatGPT to generate code to parse Maze string into a Matrix (some fix require to integrate pwntools and fix the input/output)

```python
#!/usr/bin/env python
import heapq
from pwn import *

io = remote('shortpath.flag4all.sh', 2000)

def get_maze():
    maze = io.recvuntil(b'\n\rDonner la longueur du chemin pour arriver le plus rapidement:', drop=True)
    return maze.replace(b'\r', b'').decode('utf-8')

maze_str = get_maze()
print(maze_str)

# Split the maze string into lines and remove empty lines
maze_lines = [line.strip() for line in maze_str.split('\n') if line.strip()]

# Initialize the maze matrix
maze_matrix = []

# Iterate through each line of the maze string
for line in maze_lines:
    # Initialize a row for the matrix
    row = []

    # Iterate through each character in the line
    for char in line[::2]:
        if char == "[":
            row.append(0)  # 1 represents a wall
        else:
            row.append(1)  # 0 represents a path

    # Append the row to the matrix
    maze_matrix.append(row)

# Print the maze matrix
for row in maze_matrix:
    print(row)
maze = maze_matrix

# Python3 code to implement the approach
import sys

# User defined Pair class
class Pair:
        def __init__(self, x, y):
                self.first = x
                self.second = y

# Check if it is possible to go to (x, y) from the current
# position. The function returns false if the cell has
# value 0 or already visited
def isSafe(mat, visited, x, y):
        return (x >= 0 and x < len(mat) and y >= 0 and y < len(mat[0]) and mat[x][y] == 1 and (not visited[x][y]))

def findShortestPath(mat, visited, i, j, x, y, min_dist, dist):
        if (i == x and j == y):
                min_dist = min(dist, min_dist)
                return min_dist

        # set (i, j) cell as visited
        visited[i][j] = True

        # go to the bottom cell
        if (isSafe(mat, visited, i + 1, j)):
                min_dist = findShortestPath(
                        mat, visited, i + 1, j, x, y, min_dist, dist + 1)

        # go to the right cell
        if (isSafe(mat, visited, i, j + 1)):
                min_dist = findShortestPath(
                        mat, visited, i, j + 1, x, y, min_dist, dist + 1)

        # go to the top cell
        if (isSafe(mat, visited, i - 1, j)):
                min_dist = findShortestPath(
                        mat, visited, i - 1, j, x, y, min_dist, dist + 1)

        # go to the left cell
        if (isSafe(mat, visited, i, j - 1)):
                min_dist = findShortestPath(
                        mat, visited, i, j - 1, x, y, min_dist, dist + 1)

        # backtrack: remove (i, j) from the visited matrix
        visited[i][j] = False
        return min_dist

# Wrapper over findShortestPath() function
def findShortestPathLength(mat, src, dest):
        if (len(mat) == 0 or mat[src.first][src.second] == 0
                        or mat[dest.first][dest.second] == 0):
                return -1

        row = len(mat)
        col = len(mat[0])

        # construct an `M × N` matrix to keep track of visited
        # cells
        visited = []
        for i in range(row):
                visited.append([None for _ in range(col)])

        dist = sys.maxsize
        dist = findShortestPath(mat, visited, src.first,
                                                        src.second, dest.first, dest.second, dist, 0)

        if (dist != sys.maxsize):
                return dist
        return -1

src = Pair(0, 1)
dest = Pair(34, 33)
dist = findShortestPathLength(maze, src, dest)
if (dist != -1):
    print("Shortest Path is", dist)
    payload = str(dist)
    info("payload: %s", payload)
    io.sendline(payload.encode('utf-8'))
    data = io.recvall()
	print(data)
else:
        print("Shortest Path doesn't exist")
```

```
└─$ python solve_maze.py
[+] Opening connection to shortpath.flag4all.sh on port 2000: Done
[]  [][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][]
[]          []          []      []                      []          []
[][][][][]  []  [][][]  []  []  []  [][][][][]  [][][]  []  []  [][][]
[]  []      []      []  []  []  []  []  []      []      []  []      []
[]  []  [][][][][]  []  []  []  []  []  []  [][][]  [][][]  [][][]  []
[]      []          []  []  []      []  []      []      []      []  []
[]  [][][]  [][][][][]  []  [][][][][]  [][][]  [][][][][]  [][][]  []
[]  []      []      []  []  []              []              []      []
[]  [][][]  []  [][][]  []  [][][]  [][][]  [][][][][][][][][]  [][][]
[]      []      []      []      []  []                      []      []
[][][]  [][][][][]  [][][][][]  [][][]  [][][]  [][][][][][][][][]  []
[]  []      []      []                  []      []                  []
[]  [][][]  []  [][][]  [][][][][][][][][]  [][][]  [][][][][][][][][]
[]      []  []  []      []          []          []  []              []
[]  [][][]  []  []  [][][]  [][][]  [][][][][]  []  []  [][][][][]  []
[]      []  []      []      []  []          []  []          []      []
[]  []  []  [][][]  []  [][][]  [][][][][]  [][][]  [][][][][]  []  []
[]  []  []      []  []      []          []      []      []      []  []
[]  []  [][][]  [][][][][]  [][][]  [][][][][]  [][][][][]  [][][]  []
[]  []      []          []      []          []  []          []      []
[][][][][]  [][][][][]  []  []  [][][]  []  []  []  [][][][][][][]  []
[]          []          []  []      []  []  []  []              []  []
[]  [][][]  []  [][][][][]  [][][]  []  [][][]  []  [][][][][]  []  []
[]  []      []  []          []  []  []  []      []      []      []  []
[]  [][][][][]  [][][]  [][][]  []  []  []  [][][][][]  []  [][][][][]
[]          []      []          []  []          []  []  []          []
[][][][][]  [][][]  [][][][][]  []  [][][][][]  []  []  [][][][][]  []
[]      []      []          []  []      []      []      []          []
[]  []  [][][]  [][][][][]  []  [][][]  []  [][][]  [][][]  [][][]  []
[]  []                  []  []      []  []      []      []  []  []  []
[]  [][][][][][][][][]  []  [][][][][]  [][][]  [][][]  []  []  []  []
[]      []      []      []              []  []      []  []  []      []
[]  []  []  []  [][][][][][][][][][][][][]  [][][]  [][][]  []  [][][]
[]  []      []                                              []      []
[][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][][]  []

[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0]
[0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0]
[0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0]
[0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0]
[0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0]
[0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0]
[0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0]
[0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0]
[0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0]
[0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0]
[0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0]
[0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0]
[0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0]
[0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0]
[0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0]
[0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0]
[0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0]
[0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0]
[0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0]
[0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0]
[0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0]
[0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0]
[0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0]
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0]
[0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0]
[0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0]
[0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0]
Shortest Path is 158
[*] payload: 158
[+] Receiving all data: Done (53B)
[*] Closed connection to shortpath.flag4all.sh port 2000
b'\n\rGG Voici le flag: ESD{HUUUMMMMl@M@Y0avecL3P0ul3t}\r\n'
```