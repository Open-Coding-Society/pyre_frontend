---
layout: post
search_exclude: true
show_reading_time: false
permalink: /UndH
title: Undecideable Problems, Graphs + Heuristics
categories: [TeamTeachTrio1]
---
# Undecidable Problems, Graphs + Heuristics

## Overview

This lesson explores three key concepts in computer science:
1. **Decidable vs. Undecidable Problems** - Understanding what problems computers can and cannot solve
2. **Graph Theory** - A powerful way to represent relationships between objects
3. **Heuristics** - Practical approaches for solving complex problems


---
### Collegeboard Definitions (Big Ideas)

| **Code**   | **Concept**          | **Definition**                                                                 |
|------------|----------------------|-------------------------------------------------------------------------------|
| AAP-4.B.1  | Decidable Problem    | A decision problem for which an algorithm exists that produces a correct yes-or-no answer for all possible inputs. |
| AAP-4.B.2  | Undecidable Problem  | A decision problem for which no algorithm can be constructed that always provides a correct yes-or-no answer for all inputs. |
| AAP-4.B.3  | Partial Solutions    | For some undecidable problems, certain instances can be solved algorithmically, but no single algorithm solves all instances correctly. |

### Decidable Problems
These are problems where an algorithm can definitively provide a correct answer for all possible inputs in finite time.

**Examples:**
- Is a number even?
- Is a string a palindrome?
- Does a list contain a specific value?

```python
# Decidable problem: Is a number even?
def is_even(num):
    return num % 2 == 0

# Decidable problem: Is a string a palindrome?
def is_palindrome(s):
    return s == s[::-1]
```

### Undecidable Problems
These are problems for which no algorithm can be constructed that always gives a correct answer for all possible inputs.

**The most famous example: The Halting Problem**

[Halting Problem Simulator]({{ site.baseurl }}/HSim)

Explore this interactive simulator to better understand the Halting Problem and why it is undecidable.

"Given a computer program and an input, will the program eventually halt (terminate) or will it run forever?"



**Why is it undecidable?** We can prove by contradiction that no algorithm can solve this for all possible programs and inputs.

#### The Paradoxical Proof

Imagine we have a magical function `will_halt(program, input)` that can predict if any program will halt on any input:

```python
def will_halt(program, input):
    # Magically determines if program halts on input
    # Returns True if it halts
    # Returns False if it runs forever
    ...

# Now, let's create a paradoxical program:
def paradox():
    if will_halt(paradox, None):
        while True:  # Run forever
            pass
    else:
        return  # Halt immediately
```

**The contradiction:**
- If `will_halt(paradox, None)` returns `True`, then `paradox()` will run forever (contradiction!)
- If `will_halt(paradox, None)` returns `False`, then `paradox()` will halt immediately (contradiction!)

Therefore, the function `will_halt` cannot exist for all programs.

### Partial Solutions (AAP-4.B.3)

Even though some problems are undecidable in general, we can often:
- Solve them for specific types of inputs
- Create approximate solutions
- Set practical constraints (like time limits)

---

## Graph Theory

Graphs are mathematical structures used to model relationships between objects.

### Basic Components

- **Vertices (nodes)**: Objects in the graph
- **Edges**: Connections between objects

<svg viewBox="0 0 600 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Background -->
  <rect x="0" y="0" width="600" height="400" fill="#f9f9f9" rx="10" ry="10" />
  
  <!-- Title -->
  <text x="300" y="50" font-family="Arial, sans-serif" font-size="24" text-anchor="middle" font-weight="bold">Basic Graph Components</text>
  
  <!-- Vertices/Nodes -->
  <g>
    <!-- Node A -->
    <circle cx="150" cy="150" r="30" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="150" y="155" font-family="Arial, sans-serif" font-size="18" text-anchor="middle" fill="white" font-weight="bold">A</text>
    
    <!-- Node B -->
    <circle cx="300" cy="100" r="30" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="300" y="105" font-family="Arial, sans-serif" font-size="18" text-anchor="middle" fill="white" font-weight="bold">B</text>
    
    <!-- Node C -->
    <circle cx="450" cy="150" r="30" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="450" y="155" font-family="Arial, sans-serif" font-size="18" text-anchor="middle" fill="white" font-weight="bold">C</text>
    
    <!-- Node D -->
    <circle cx="300" cy="250" r="30" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="300" y="255" font-family="Arial, sans-serif" font-size="18" text-anchor="middle" fill="white" font-weight="bold">D</text>
  </g>
  
  <!-- Edges -->
  <g>
    <!-- Edge A-B -->
    <line x1="173" y1="135" x2="277" y2="115" stroke="#ff6b6b" stroke-width="4" />
    
    <!-- Edge B-C -->
    <line x1="327" y1="109" x2="423" y2="141" stroke="#ff6b6b" stroke-width="4" />
    
    <!-- Edge C-D -->
    <line x1="427" y1="170" x2="323" y2="230" stroke="#ff6b6b" stroke-width="4" />
    
    <!-- Edge D-A -->
    <line x1="277" y1="230" x2="173" y2="170" stroke="#ff6b6b" stroke-width="4" />
    
    <!-- Edge A-C (diagonal across) -->
    <line x1="179" y1="150" x2="421" y2="150" stroke="#ff6b6b" stroke-width="4" stroke-dasharray="8" />
  </g>
  
  <!-- Labels -->
  <g>
    <!-- Vertex Label -->
    <text x="150" y="320" font-family="Arial, sans-serif" font-size="20" text-anchor="middle" font-weight="bold">Vertices (Nodes)</text>
    <path d="M150,200 L150,240 L190,270" fill="none" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)" />
    
    <!-- Edge Label -->
    <text x="450" y="320" font-family="Arial, sans-serif" font-size="20" text-anchor="middle" font-weight="bold">Edges</text>
    <path d="M450,200 L450,240 L410,270" fill="none" stroke="#333" stroke-width="2" marker-end="url(#arrowhead)" />
  </g>
  
  <!-- Arrow marker definition -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#333" />
    </marker>
  </defs>
  
  <!-- Legend -->
  <rect x="140" y="350" width="320" height="30" rx="5" ry="5" fill="#eee" stroke="#ccc" />
  <text x="300" y="370" font-family="Arial, sans-serif" font-size="14" text-anchor="middle">A graph consists of vertices connected by edges</text>
</svg>

### Types of Graphs

1. **Directed vs. Undirected**
   - In directed graphs, edges have a direction
   - In undirected graphs, edges have no direction

   ![Directed vs Undirected](https://media.geeksforgeeks.org/wp-content/uploads/20200630111809/graph18.jpg)

2. **Weighted vs. Unweighted**
   - In weighted graphs, edges have values (weights)
   - In unweighted graphs, all edges are equal

<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Background -->
  <rect x="0" y="0" width="800" height="400" fill="#f9f9f9" rx="10" ry="10" />
  
  <!-- Title -->
  <text x="400" y="40" font-family="Arial, sans-serif" font-size="24" text-anchor="middle" font-weight="bold">Weighted vs. Unweighted Graphs</text>
  
  <!-- Unweighted Graph Section -->
  <g transform="translate(0, 0)">
    <!-- Section Title -->
    <text x="200" y="80" font-family="Arial, sans-serif" font-size="20" text-anchor="middle" font-weight="bold">Unweighted Graph</text>
    
    <!-- Nodes -->
    <circle cx="100" cy="150" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="100" y="155" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">A</text>
    
    <circle cx="200" cy="120" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="200" y="125" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">B</text>
    
    <circle cx="300" cy="150" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="300" y="155" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">C</text>
    
    <circle cx="200" cy="230" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="200" y="235" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">D</text>
    
    <!-- Edges -->
    <line x1="120" y1="137" x2="180" y2="126" stroke="#ff6b6b" stroke-width="3" />
    <line x1="223" y1="130" x2="277" y2="140" stroke="#ff6b6b" stroke-width="3" />
    <line x1="280" y1="170" x2="223" y2="217" stroke="#ff6b6b" stroke-width="3" />
    <line x1="180" y1="217" x2="120" y2="170" stroke="#ff6b6b" stroke-width="3" />
    
    <!-- Description -->
    <text x="200" y="300" font-family="Arial, sans-serif" font-size="14" text-anchor="middle" fill="#333">All edges have equal importance</text>
    <text x="200" y="320" font-family="Arial, sans-serif" font-size="14" text-anchor="middle" fill="#333">(no numerical values)</text>
  </g>
  
  <!-- Weighted Graph Section -->
  <g transform="translate(400, 0)">
    <!-- Section Title -->
    <text x="200" y="80" font-family="Arial, sans-serif" font-size="20" text-anchor="middle" font-weight="bold">Weighted Graph</text>
    
    <!-- Nodes -->
    <circle cx="100" cy="150" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="100" y="155" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">A</text>
    
    <circle cx="200" cy="120" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="200" y="125" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">B</text>
    
    <circle cx="300" cy="150" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="300" y="155" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">C</text>
    
    <circle cx="200" cy="230" r="25" fill="#4f86f7" stroke="#2a4b8d" stroke-width="3" />
    <text x="200" y="235" font-family="Arial, sans-serif" font-size="16" text-anchor="middle" fill="white" font-weight="bold">D</text>
    
    <!-- Edges with weights -->
    <line x1="120" y1="137" x2="180" y2="126" stroke="#ff6b6b" stroke-width="3" />
    <rect x="138" y="122" width="24" height="16" fill="white" rx="4" ry="4" stroke="#333" stroke-width="1" />
    <text x="150" y="134" font-family="Arial, sans-serif" font-size="12" text-anchor="middle" fill="#333" font-weight="bold">5</text>
    
    <line x1="223" y1="130" x2="277" y2="140" stroke="#ff6b6b" stroke-width="3" />
    <rect x="238" y="123" width="24" height="16" fill="white" rx="4" ry="4" stroke="#333" stroke-width="1" />
    <text x="250" y="135" font-family="Arial, sans-serif" font-size="12" text-anchor="middle" fill="#333" font-weight="bold">2</text>
    
    <line x1="280" y1="170" x2="223" y2="217" stroke="#ff6b6b" stroke-width="3" />
    <rect x="238" y="185" width="24" height="16" fill="white" rx="4" ry="4" stroke="#333" stroke-width="1" />
    <text x="250" y="197" font-family="Arial, sans-serif" font-size="12" text-anchor="middle" fill="#333" font-weight="bold">8</text>
    
    <line x1="180" y1="217" x2="120" y2="170" stroke="#ff6b6b" stroke-width="3" />
    <rect x="138" y="185" width="24" height="16" fill="white" rx="4" ry="4" stroke="#333" stroke-width="1" />
    <text x="150" y="197" font-family="Arial, sans-serif" font-size="12" text-anchor="middle" fill="#333" font-weight="bold">3</text>
    
    <!-- Description -->
    <text x="200" y="300" font-family="Arial, sans-serif" font-size="14" text-anchor="middle" fill="#333">Edges have numerical values (weights)</text>
    <text x="200" y="320" font-family="Arial, sans-serif" font-size="14" text-anchor="middle" fill="#333">representing distance, cost, time, etc.</text>
  </g>
  
  <!-- Dividing Line -->
  <line x1="400" y1="70" x2="400" y2="340" stroke="#ccc" stroke-width="2" stroke-dasharray="5,5" />
  
  <!-- Footer -->
  <rect x="150" y="350" width="500" height="30" rx="5" ry="5" fill="#eee" stroke="#ccc" />
  <text x="400" y="370" font-family="Arial, sans-serif" font-size="14" text-anchor="middle">
    Weights can represent distances, costs, time, capacity, or other measures
  </text>
</svg>

3. **Connected vs. Disconnected**
   - In connected graphs, there is a path between every pair of vertices
   - In disconnected graphs, some vertices cannot be reached from others

### Graph Representations

#### 1. Adjacency Matrix
A 2D array where element [i][j] indicates if there is an edge from vertex i to vertex j.

```
For this graph:
    A --- B
   /|     |
  / |     |
 C  |     D
  \ |    /
   \|   /
    E---

Adjacency Matrix:
  | A | B | C | D | E |
--+---+---+---+---+---+
A | 0 | 1 | 1 | 0 | 1 |
--+---+---+---+---+---+
B | 1 | 0 | 0 | 1 | 0 |
--+---+---+---+---+---+
C | 1 | 0 | 0 | 0 | 1 |
--+---+---+---+---+---+
D | 0 | 1 | 0 | 0 | 1 |
--+---+---+---+---+---+
E | 1 | 0 | 1 | 1 | 0 |
```

#### 2. Adjacency List
A collection of lists, where each list contains the neighbors of a vertex.

```python
# For the same graph:
graph = {
    'A': ['B', 'C', 'E'],
    'B': ['A', 'D'],
    'C': ['A', 'E'],
    'D': ['B', 'E'],
    'E': ['A', 'C', 'D']
}
```

### Graph Traversal Algorithms

#### Breadth-First Search (BFS)
- Explores all neighbors at the current depth before moving to nodes at the next depth
- Uses a queue data structure
- Good for finding shortest paths in unweighted graphs

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=" ")
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# Using our example graph
bfs(graph, 'A')  # Output: A B C E D
```

#### Depth-First Search (DFS)
- Explores as far as possible along each branch before backtracking
- Uses a stack data structure (or recursion)
- Good for maze solving, topological sorting, etc.

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=" ")
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# Using our example graph
dfs(graph, 'A')  # Output: A B D E C
```

---

## Heuristics

A heuristic is a problem-solving approach that employs a practical method that may not be perfect but is sufficient for reaching an immediate goal.

### What are Heuristics?

- "Rules of thumb" or educated guesses
- Practical approaches when finding an optimal solution is impractical
- Often used for:
  - Undecidable problems
  - NP-hard problems
  - Time-constrained scenarios

### Real-World Example: The Traveling Salesperson Problem (TSP)

**Problem:** Find the shortest possible route that visits each city exactly once and returns to the origin city.

**Challenge:** For n cities, there are (n-1)!/2 possible routes. With just 20 cities, that's over 60 quadrillion routes!

**Solution:** Use heuristics like:

#### 1. Nearest Neighbor Algorithm
- Start at a random city
- Visit the nearest unvisited city
- Repeat until all cities are visited
- Return to the starting city

[Interactive TSP Visualization](https://tspvis.com/)

```python
def nearest_neighbor_tsp(distances, start=0):
    n = len(distances)
    unvisited = set(range(n))
    unvisited.remove(start)
    tour = [start]
    current = start
    total_distance = 0
    
    while unvisited:
        nearest = min(unvisited, key=lambda city: distances[current][city])
        total_distance += distances[current][nearest]
        current = nearest
        tour.append(current)
        unvisited.remove(current)
    
    # Return to start
    total_distance += distances[current][start]
    tour.append(start)
    
    return tour, total_distance
```

#### Example:

For a 4-city problem with the following distances:

| City | A | B | C | D |
|------|---|---|---|---|
| A    | 0 | 10| 15| 20|
| B    | 10| 0 | 35| 25|
| C    | 15| 35| 0 | 30|
| D    | 20| 25| 30| 0 |

Starting from city A:
1. Nearest to A is B (distance 10)
2. Nearest to B is A (already visited), then D (distance 25)
3. Nearest to D is A (already visited), then B (already visited), then C (distance 30)
4. Return to A (distance 15)

Total route: A → B → D → C → A
Total distance: 10 + 25 + 30 + 15 = 80

**Note:** This may not be optimal! The optimal route could be A → C → D → B → A with distance 15 + 30 + 25 + 10 = 80 (in this case they happen to be the same, but often they're not).

### Other Heuristic Approaches

1. **Greedy Algorithms**
   - Make locally optimal choices at each step
   - May not lead to global optimum
   - Example: Nearest neighbor for TSP

2. **Local Search**
   - Start with a solution and iteratively improve it
   - Example: 2-opt for TSP (swap two edges if it improves the solution)

3. **Meta-heuristics**
   - Higher-level strategies that guide other heuristics
   - Examples:
     - Genetic algorithms (inspired by natural selection)
     - Simulated annealing (inspired by metallurgy)
     - Ant colony optimization (inspired by ant behavior)

---

## Case Study: Package Delivery Route Optimization

A delivery company needs to determine routes for delivering packages to 100+ locations daily.

**Challenges:**
- Finding the optimal route is an NP-hard problem
- Computing all possible routes (100+ factorial) would take longer than the universe has existed
- Routes need to be computed quickly (in minutes, not years)

**Solution: Heuristic Approaches**
1. **Clustering** - Group nearby locations
2. **Nearest Neighbor** - Create initial routes
3. **Local Search** - Refine routes by swapping deliveries between routes

**Result:**
- Not mathematically optimal, but practically efficient
- Routes computed in minutes instead of centuries
- Significant fuel and time savings

![Route Optimization]({{ site.baseurl }}/images/cool.png)
---

## Connection Between These Concepts

1. **Undecidable Problems and Heuristics**
   - When problems can't be solved algorithmically for all cases, heuristics offer practical solutions
   - Example: While the halting problem is undecidable, we can use heuristics like timeout mechanisms

2. **Graphs and Heuristics**
   - Many graph problems (like TSP) are NP-hard
   - Heuristics make these problems tractable
   - Example: Using nearest neighbor for TSP instead of checking all permutations

3. **Real-World Applications**
   - Package delivery (UPS, FedEx)
   - Network routing (Internet packets)
   - Resource allocation (CPU scheduling)
   - GPS navigation systems

---

## Practice Problems

### Problem 1: Classify as Decidable or Undecidable

For each problem, determine if it's decidable or undecidable:

1. "Is the sum of two integers greater than 100?"
2. "Will this Python program ever print 'Hello World'?"
3. "Do two regular expressions generate the same language?"
4. "Is this number prime?"

<details>
<summary><strong>Answers</strong></summary>
1. Decidable - Simple arithmetic comparison<br>
2. Undecidable - Related to the halting problem<br>
3. Undecidable - Equivalence of regular expressions is undecidable<br>
4. Decidable - Primality testing has known algorithms
</details>

### Problem 2: Graph Traversal

For the following graph:
```
    A --- B
   /|     |
  / |     |
 C  |     D
  \ |    /
   \|   /
    E---
```

1. Write the adjacency list representation
2. Show the traversal order using BFS starting from node A
3. Show the traversal order using DFS starting from node A

<details>
<summary><strong>Answers</strong></summary>

1. Adjacency List:
```python
graph = {
    'A': ['B', 'C', 'E'],
    'B': ['A', 'D'],
    'C': ['A', 'E'],
    'D': ['B', 'E'],
    'E': ['A', 'C', 'D']
}
```

2. BFS from A: A → B → C → E → D
3. DFS from A: A → B → D → E → C (may vary based on implementation)
</details>

### Problem 3: TSP with Nearest Neighbor

Given these distances between cities:

| City | A | B | C | D | E |
|------|---|---|---|---|---|
| A    | 0 | 12| 19| 8 | 15|
| B    | 12| 0 | 10| 18| 5 |
| C    | 19| 10| 0 | 6 | 14|
| D    | 8 | 18| 6 | 0 | 9 |
| E    | 15| 5 | 14| 9 | 0 |

1. Find a route using the Nearest Neighbor heuristic starting from city A
2. Calculate the total distance

<details>
<summary><strong>Answer</strong></summary>

Starting from A:
1. Nearest to A is D (distance 8)
2. Nearest to D is C (distance 6)
3. Nearest to C is B (distance 10)
4. Nearest to B is E (distance 5)
5. Return to A (distance 15)

Route: A → D → C → B → E → A
Total distance: 8 + 6 + 10 + 5 + 15 = 44
</details>

---

## MCQ Review Question

**Question:**
A company delivers packages by truck and would like to minimize the length of the route that each driver must travel to reach n delivery locations. The company is considering two different algorithms for determining delivery routes.

*Algorithm I*
Generate all possible routes, compute their lengths, and then select the shortest possible route. This algorithm does not run in reasonable time.

*Algorithm II*
Starting from an arbitrary delivery location, find the nearest unvisited delivery location. Continue creating the route by selecting the nearest unvisited location until all locations have been visited. This algorithm runs in time proportional to n².

Which of the following best describes Algorithm II?

**Options:**
- A: Algorithm II attempts to use an algorithmic approach to solve an otherwise undecidable problem.
- B: Algorithm II uses a heuristic approa ch to provide an approximate solution in reasonable time.
- C: Algorithm II provides no improvement over algorithm I because neither algorithm runs in reasonable time.
- D: Algorithm II requires a much faster computer in order to provide any improvement over algorithm I.

<details>
<summary><strong>Answer</strong></summary>
B: Algorithm II uses a heuristic approach to provide an approximate solution in reasonable time.

Explanation: Algorithm II is implementing the Nearest Neighbor heuristic. While it may not find the optimal solution, it runs in polynomial time (O(n²)), which is considered reasonable, unlike Algorithm I which would take factorial time (O(n!)).
</details>

Which of the following best explains why it is not possible to use computers to solve every problem?

Responses  
A  Current computer processing capabilities cannot improve significantly.

B  Large-scale problems require a crowdsourcing model, which is limited by the number of people available to work on the problem.

C  The ability of a computer to solve a problem is limited by the bandwidth of the computer’s Internet connection.

D  There exist some problems that cannot be solved using any algorithm.

<details>
<summary><strong>Answer</strong></summary>
D: There exist some problems that cannot be solved using any algorithm.

Explanation: These are known as undecidable problems in computer science. No matter how powerful a computer is, or how advanced algorithms become, there are some problems (like the Halting Problem) that no algorithm can ever solve for all possible inputs.
</details>


---

## Homework Assignment

1. Research and write a brief explanation of another undecidable problem not discussed in class
2. Implement the Nearest Neighbor algorithm for the TSP in a programming language of your choice
3. Find a real-world example where heuristics are used and explain why an exact solution isn't practical
4. Complete the Kahoot quiz on these topics: [Link to quiz]

---

## Additional Resources

- [Computerphile: The Halting Problem](https://www.youtube.com/watch?v=macM_MtS_w4)
- [Visualizing Graph Algorithms](https://visualgo.net/en/graphds)
- [Interactive TSP Visualization](https://tspvis.com/)
- [Introduction to Graph Theory](https://math.mit.edu/~rpeng/18.330/graph/index.html)
- [Heuristic Algorithms in Computer Science](https://towardsdatascience.com/heuristic-algorithms-in-machine-learning-59b2b8f8e3be)
- [11 Animated Algorithms for the Traveling Salesman Problem](https://stemlounge.com/animated-algorithms-for-the-traveling-salesman-problem/) 