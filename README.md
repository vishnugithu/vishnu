import numpy as np
import matplotlib.pyplot as plt

def is_valid(x, y, grid):
    """
    Check if position (x,y) is valid on the grid
    """
    return x>=0 and x<grid.shape[0] and y>=0 and y<grid.shape[1]

def get_adjacent_cells(x, y, grid, adjacency):
    """
    Get adjacent cells to (x,y) based on adjacency type
    """
    if adjacency == 8:
        dx = [-1, -1, 0, 1, 1, 1, 0, -1]
        dy = [0, 1, 1, 1, 0, -1, -1, -1]
    elif adjacency == 4:
        dx = [-1, 0, 1, 0]
        dy = [0, 1, 0, -1]
    else:
        raise ValueError("Invalid adjacency type")
    
    adjacent_cells = []
    for i in range(len(dx)):
        if is_valid(x+dx[i], y+dy[i], grid):
            adjacent_cells.append((x+dx[i], y+dy[i]))
            
    return adjacent_cells

def bfs(start, end, grid, drone_id, adjacency):
    """
    Breadth First Search algorithm to find shortest path from start to end position
    """
    queue = [(start, [start])]
    visited = set([start])
    while queue:
        (x,y), path = queue.pop(0)
        if (x,y) == end:
            # Found path to end position
            for i in range(len(path)):
                grid[path[i][0], path[i][1]] = drone_id
            return path
        adjacent_cells = get_adjacent_cells(x, y, grid, adjacency)
        for (x2,y2) in adjacent_cells:
            if (x2,y2) not in visited:
                queue.append(((x2,y2), path+[(x2,y2)]))
                visited.add((x2,y2))
    return None

def check_collision(path, grid):
    """
    Check for collisions with other drones
    """
    for i in range(len(path)):
        if grid[path[i][0], path[i][1]] != 0:
            return True
    return False

def solve(drones, adjacency=8):
    """
    Solve the problem for given list of drones and adjacency type
    """
