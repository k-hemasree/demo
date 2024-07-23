[next_state] = current_state
    
    
    current_state = GOAL_STATE
    path = [current_state]
    while current_state != INITIAL_STATE:
        current_state = came_from[current_state]
        path.append(current_state)
    
    path.reverse()
    return path

def print_path(path):
    for i, state in enumerate(path):
        left_missionaries, left_cannibals, boat_position = state
        right_missionaries = 3 - left_missionaries
        right_cannibals = 3 - left_cannibals
        print(f"Step {i}: ({left_missionaries}M, {left_cannibals}C, {'left' if boat_position == 1 else 'right'}) -> "
              f"({right_missionaries}M, {right_cannibals}C, {'right' if boat_position == 0 else 'left'})")

path = bfs()
print("Solution path:")
print_path(path)
