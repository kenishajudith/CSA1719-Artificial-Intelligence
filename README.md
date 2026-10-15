1) Write the python program to solve 8-Puzzle problem
import heapq

# Goal state
goal = (1, 2, 3,
        4, 5, 6,
        7, 8, 0)

# Calculate Manhattan Distance
def heuristic(state):
    distance = 0

    for i in range(9):
        if state[i] != 0:
            goal_position = goal.index(state[i])

            current_row = i // 3
            current_col = i % 3

            goal_row = goal_position // 3
            goal_col = goal_position % 3

            distance += abs(current_row - goal_row)
            distance += abs(current_col - goal_col)

    return distance


# Generate possible moves
def get_neighbors(state):
    neighbors = []

    zero_position = state.index(0)

    row = zero_position // 3
    col = zero_position % 3

    # Possible movements: Up, Down, Left, Right
    moves = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    for move_row, move_col in moves:
        new_row = row + move_row
        new_col = col + move_col

        if 0 <= new_row < 3 and 0 <= new_col < 3:

            new_position = new_row * 3 + new_col

            new_state = list(state)

            # Swap blank space with tile
            new_state[zero_position], new_state[new_position] = \
                new_state[new_position], new_state[zero_position]

            neighbors.append(tuple(new_state))

    return neighbors


# A* Algorithm
def solve(start):
    priority_queue = []

    # (f, g, state, path)
    heapq.heappush(priority_queue,
                   (heuristic(start), 0, start, [start]))

    visited = set()

    while priority_queue:

        f, g, current, path = heapq.heappop(priority_queue)

        if current in visited:
            continue

        visited.add(current)

        # Goal reached
        if current == goal:
            return path

        for neighbor in get_neighbors(current):

            if neighbor not in visited:

                new_g = g + 1
                new_f = new_g + heuristic(neighbor)

                heapq.heappush(
                    priority_queue,
                    (new_f, new_g, neighbor, path + [neighbor])
                )

    return None


# Print puzzle
def print_puzzle(state):
    for i in range(0, 9, 3):
        print(state[i], state[i + 1], state[i + 2])
    print()


# Starting state
start = (1, 2, 3,
         4, 0, 6,
         7, 5, 8)

# Solve puzzle
solution = solve(start)

# Display solution
if solution:
    print("Solution found!")
    print("Number of moves:", len(solution) - 1)
    print()

    for step in solution:
        print_puzzle(step)
else:
    print("No solution found.")

2) Write the python program to solve 8-Queen problem
# Program to solve 8-Queens Problem using Backtracking

N = 8

# Print the chess board
def print_board(board):
    for row in board:
        print(row)
    print()


# Check whether a queen can be placed
def is_safe(board, row, col):

    # Check column
    for i in range(row):
        if board[i][col] == 1:
            return False

    # Check upper-left diagonal
    i = row - 1
    j = col - 1

    while i >= 0 and j >= 0:
        if board[i][j] == 1:
            return False
        i -= 1
        j -= 1

    # Check upper-right diagonal
    i = row - 1
    j = col + 1

    while i >= 0 and j < N:
        if board[i][j] == 1:
            return False
        i -= 1
        j += 1

    return True


# Solve the problem
def solve(board, row):

    # All queens are placed
    if row == N:
        return True

    # Try placing queen in each column
    for col in range(N):

        if is_safe(board, row, col):

            # Place queen
            board[row][col] = 1

            # Recursively place remaining queens
            if solve(board, row + 1):
                return True

            # Backtrack
            board[row][col] = 0

    return False


# Create an empty 8 x 8 board
board = [[0 for _ in range(N)] for _ in range(N)]

# Solve
if solve(board, 0):
    print("Solution:")
    print_board(board)
else:
    print("No solution exists.")

3)Write the python program for Water Jug Problem

from collections import deque

# Capacity of the two jugs
jug1 = 4
jug2 = 3

# Target amount
target = 2

# BFS function
def water_jug():
    queue = deque()
    visited = set()

    # Start with both jugs empty
    queue.append((0, 0, []))
    visited.add((0, 0))

    while queue:

        a, b, path = queue.popleft()

        # Add current state to path
        path = path + [(a, b)]

        # Check whether target is reached
        if a == target or b == target:
            return path

        # All possible operations
        states = [
            (jug1, b),                  # Fill Jug 1
            (a, jug2),                  # Fill Jug 2
            (0, b),                     # Empty Jug 1
            (a, 0),                     # Empty Jug 2
            (a - min(a, jug2 - b),      # Pour Jug 1 -> Jug 2
             b + min(a, jug2 - b)),
            (a + min(b, jug1 - a),      # Pour Jug 2 -> Jug 1
             b - min(b, jug1 - a))
        ]

        # Add new states
        for state in states:

            if state not in visited:
                visited.add(state)
                queue.append((state[0], state[1], path))

    return None


# Find solution
solution = water_jug()

# Display solution
if solution:
    print("Steps to solve the Water Jug Problem:")

    for i, state in enumerate(solution):
        print("Step", i, ":", state)

else:
    print("No solution exists.")

4)Write the python program for Cript-Arithmetic problem

from itertools import permutations

words = ["SEND", "MORE"]
result = "MONEY"

letters = set("SENDMORY")
letters = list(letters)

for p in permutations(range(10), len(letters)):
    d = dict(zip(letters, p))

    if d["S"] == 0 or d["M"] == 0:
        continue

    send = 1000*d["S"] + 100*d["E"] + 10*d["N"] + d["D"]
    more = 1000*d["M"] + 100*d["O"] + 10*d["R"] + d["E"]
    money = 10000*d["M"] + 1000*d["O"] + 100*d["N"] + 10*d["E"] + d["Y"]

    if send + more == money:
        print("Solution:")
        print("SEND =", send)
        print("MORE =", more)
        print("MONEY =", money)
        break
5)  Write the python program for Missionaries Cannibal problem

from collections import deque

def is_valid(m_left, c_left, m_right, c_right):
    if m_left < 0 or c_left < 0 or m_right < 0 or c_right < 0:
        return False

    if m_left > 0 and m_left < c_left:
        return False

    if m_right > 0 and m_right < c_right:
        return False

    return True

def solve():
    start = (3, 3, 0, 0, 0)
    goal = (0, 0, 3, 3, 1)

    queue = deque([(start, [start])])
    visited = {start}

    while queue:
        state, path = queue.popleft()
        ml, cl, mr, cr, boat = state

        if state == goal:
            return path

        moves = [(1,0), (2,0), (0,1), (0,2), (1,1)]

        for m, c in moves:
            if boat == 0:
                new_state = (ml-m, cl-c, mr+m, cr+c, 1)
            else:
                new_state = (ml+m, cl+c, mr-m, cr-c, 0)

            if new_state not in visited and is_valid(
                new_state[0], new_state[1],
                new_state[2], new_state[3]
            ):
                visited.add(new_state)
                queue.append((new_state, path + [new_state]))

    return None

solution = solve()

if solution:
    for i, state in enumerate(solution):
        print("Step", i, ":", state)
else:
    print("No solution")

6)Write the python program for Vacuum Cleaner problem

def vacuum_cleaner():
    rooms = {
        "A": "Dirty",
        "B": "Dirty"
    }

    location = "A"

    while "Dirty" in rooms.values():
        print("Vacuum is in room", location)

        if rooms[location] == "Dirty":
            print("Cleaning room", location)
            rooms[location] = "Clean"

        if location == "A":
            location = "B"
        else:
            location = "A"

    print("All rooms are clean.")

vacuum_cleaner()

7)Write the python program to implement BFS.

from collections import deque

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

def bfs(start):
    visited = set()
    queue = deque([start])

    while queue:
        node = queue.popleft()

        if node not in visited:
            print(node, end=" ")
            visited.add(node)

            for neighbour in graph[node]:
                if neighbour not in visited:
                    queue.append(neighbour)

bfs('A')

8)Write the python program to implement DFS.

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

def dfs(node, visited):
    if node not in visited:
        print(node, end=" ")
        visited.add(node)

        for neighbour in graph[node]:
            dfs(neighbour, visited)

visited = set()
dfs('A', visited)

8)Write the python to implement Travelling Salesman Problem

from itertools import permutations

def tsp(graph, start):
    cities = list(graph.keys())
    cities.remove(start)

    min_cost = float('inf')
    best_path = []

    for path in permutations(cities):
        route = [start] + list(path) + [start]
        cost = 0

        for i in range(len(route) - 1):
            cost += graph[route[i]][route[i + 1]]

        if cost < min_cost:
            min_cost = cost
            best_path = route

    print("Shortest Path:", " -> ".join(best_path))
    print("Minimum Cost:", min_cost)

graph = {
    'A': {'A': 0, 'B': 10, 'C': 15, 'D': 20},
    'B': {'A': 10, 'B': 0, 'C': 35, 'D': 25},
    'C': {'A': 15, 'B': 35, 'C': 0, 'D': 30},
    'D': {'A': 20, 'B': 25, 'C': 30, 'D': 0}
}

tsp(graph, 'A')

9)Write the python program to implement A* algorithm

import heapq

graph = {
    'A': [('B', 1), ('C', 3)],
    'B': [('D', 1), ('E', 5)],
    'C': [('F', 2)],
    'D': [],
    'E': [('G', 2)],
    'F': [('G', 1)],
    'G': []
}

heuristic = {
    'A': 5,
    'B': 4,
    'C': 3,
    'D': 3,
    'E': 1,
    'F': 1,
    'G': 0
}

def a_star(start, goal):
    queue = [(heuristic[start], 0, start, [start])]
    visited = set()

    while queue:
        f, g, node, path = heapq.heappop(queue)

        if node == goal:
            print("Path:", " -> ".join(path))
            print("Cost:", g)
            return

        if node in visited:
            continue

        visited.add(node)

        for neighbor, cost in graph[node]:
            if neighbor not in visited:
                new_g = g + cost
                new_f = new_g + heuristic[neighbor]
                heapq.heappush(
                    queue, (new_f, new_g, neighbor, path + [neighbor])
                )

    print("No path found")

a_star('A', 'G')

10)Write the python to implement Travelling Salesman Problem

from itertools import permutations

def tsp(graph, start):
    cities = list(graph.keys())
    cities.remove(start)

    min_cost = float('inf')
    best_path = []

    for path in permutations(cities):
        route = [start] + list(path) + [start]
        cost = 0

        for i in range(len(route) - 1):
            cost += graph[route[i]][route[i + 1]]

        if cost < min_cost:
            min_cost = cost
            best_path = route

    print("Shortest Path:", " -> ".join(best_path))
    print("Minimum Cost:", min_cost)

graph = {
    'A': {'A': 0, 'B': 10, 'C': 15, 'D': 20},
    'B': {'A': 10, 'B': 0, 'C': 35, 'D': 25},
    'C': {'A': 15, 'B': 35, 'C': 0, 'D': 30},
    'D': {'A': 20, 'B': 25, 'C': 30, 'D': 0}
}

tsp(graph, 'A')
