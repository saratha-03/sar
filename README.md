import random
import heapq

# Intersection directions
directions = ['North', 'South', 'East', 'West']

# Car queues for each direction
traffic_queues = {d: [] for d in directions}
wait_times = {d: [] for d in directions}

# Parameters
simulation_time = 100  # seconds
arrival_rate = 0.3     # probability of car arrival per second per direction
green_light_duration = 10

# Generate traffic
def generate_traffic():
    for t in range(simulation_time):
        for d in directions:
            if random.random() < arrival_rate:
                traffic_queues[d].append(t)

# Simulate traffic light cycle
def simulate():
    cycle = ['North', 'South', 'East', 'West']
    current_time = 0
    while current_time < simulation_time:
        for d in cycle:
            green_end = min(current_time + green_light_duration, simulation_time)
            while traffic_queues[d] and traffic_queues[d][0] < green_end:
                arrival_time = heapq.heappop(traffic_queues[d])
                wait = current_time - arrival_time
                wait_times[d].append(max(wait, 0))
            current_time = green_end# Calculate metrics
def calculate_stats():
    total_wait = 0
    total_cars = 0
    for d in directions:
        total_wait += sum(wait_times[d])
        total_cars += len(wait_times[d])
    avg_wait = total_wait / total_cars if total_cars else 0
    print(f"Total Cars: {total_cars}")
    print(f"Average Wait Time: {avg_wait:.2f} seconds")

# Run simulation
def run_simulation():
    print("Starting traffic simulation...")
    generate_traffic()
    for d in directions:
        heapq.heapify(traffic_queues[d])
    simulate()
    calculate_stats()

run_simulation()
