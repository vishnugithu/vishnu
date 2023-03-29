from collections import deque

def bfs(start, end, grid, size):
    visited = set()
    queue = deque([(start, [])])
    while queue:
        curr_pos, path = queue.popleft()
        if curr_pos == end:
            return path
        if curr_pos in visited:
            continue
        visited.add(curr_pos)
        x, y = curr_pos
        for dx in [-1, 0, 1]:
            for dy in [-1, 0, 1]:
                new_x, new_y = x + dx, y + dy
                if (new_x, new_y) not in visited and 0 <= new_x < size and 0 <= new_y < size and grid[new_x][new_y] == 0:
                    queue.append(((new_x, new_y), path + [(new_x, new_y)]))
    return []

def get_paths(drones):
    grid = [[0 for _ in range(20)] for _ in range(20)]
    size = 20
    paths = []
    for drone in drones:
        start_x, start_y, end_x, end_y, time = drone
        start = (start_x, start_y)
        end = (end_x, end_y)
        path = bfs(start, end, grid, size)
        for x, y in path:
            grid[x][y] = time
        paths.append(path)
    return paths

drones = [
    [1, 1, 10, 10, 1],
    [5, 5, 15, 15, 2],
    [2, 2, 18, 18, 3]
]

paths = get_paths(drones)
print(paths)
