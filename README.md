from queue import PriorityQueue

INITIAL_STATE = (3, 3, 1)
GOAL_STATE = (0, 0, 0)

def is_valid_state(state):
    left_missionaries, left_cannibals, boat_position = state
    right_missionaries = 3 - left_missionaries
    right_cannibals = 3 - left_cannibals
    

    if left_missionaries > 0 and left_cannibals > left_missionaries:
        return False
    if right_missionaries > 0 and right_cannibals > right_missionaries:
        return False
    
    
    if left_missionaries < 0 or left_cannibals < 0 or right_missionaries < 0 or right_cannibals < 0:
        return False
    if left_missionaries > 3 or left_cannibals > 3 or right_missionaries > 3 or right_cannibals > 3:
        return False
    
    return True

def generate_next_states(state):
    next_states = []
    left_missionaries, left_cannibals, boat_position = state
    new_boat_position = 1 - boat_position
    
    for m in range(3):
        for c in range(3):
            if 1 <= m + c <= 2:
                if boat_position == 1:  # Boat is on the left side
                    new_left_m = left_missionaries - m
                    new_left_c = left_cannibals - c
                else:  # Boat is on the right side
                    new_left_m = left_missionaries + m
                    new_left_c = left_cannibals + c
                
                new_state = (new_left_m, new_left_c, new_boat_position)
                if is_valid_state(new_state):
                    next_states.append(new_state)
    
    return next_states

def bfs():
    frontier = PriorityQueue()
    frontier.put((0, INITIAL_STATE))
    came_from = {}
    cost_so_far = {INITIAL_STATE: 0}
    
    while not frontier.empty():
        _, current_state = frontier.get()
        
        if current_state == GOAL_STATE:
            break
        
        for next_state in generate_next_states(current_state):
            new_cost = cost_so_far[current_state] + 1
            if next_state not in cost_so_far or new_cost < cost_so_far[next_state]:
                cost_so_far[next_state] = new_cost
                priority = new_cost
                frontier.put((priority, next_state))
                came_from[next_state] = current_state
    
    
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
