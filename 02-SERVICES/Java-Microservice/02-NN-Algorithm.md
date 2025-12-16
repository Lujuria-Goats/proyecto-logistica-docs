---
title: NN Algorithm
description: Explanation of the algorithm and formulas.
---

# Nearest Neighbor (NN) Algorithm

> **Technical Note:** In this project, "NN" refers to the **Nearest Neighbor** combinatorial optimization algorithm, used as a heuristic for the Traveling Salesperson Problem (TSP).

## Description
The implemented algorithm is a greedy heuristic that builds the route step-by-step. In each iteration, it selects the nearest unvisited destination from the current location.

## Formulas
To calculate "proximity", we use the **Haversine Formula**, which determines the great-circle distance (shortest distance on a sphere) between two geographical points.

Given two coordinates:
*   Point 1: $(\varphi_1, \lambda_1)$
*   Point 2: $(\varphi_2, \lambda_2)$

Where $\varphi$ is latitude and $\lambda$ is longitude (in radians).

**1. Difference Calculation:**
$$ \Delta\varphi = \varphi_2 - \varphi_1 $$
$$ \Delta\lambda = \lambda_2 - \lambda_1 $$

**2. Haversine Formula:**
$$ a = \sin^2\left(\frac{\Delta\varphi}{2}\right) + \cos(\varphi_1) \cdot \cos(\varphi_2) \cdot \sin^2\left(\frac{\Delta\lambda}{2}\right) $$

**3. Angular Distance ($c$):**
$$ c = 2 \cdot \text{atan2}\left(\sqrt{a}, \sqrt{1-a}\right) $$

**4. Final Distance ($d$):**
$$ d = R \cdot c $$

*Where $R$ is the Earth's radius ($R \approx 6371.0 \text{ km}$).*

---

## Implementation

The logic is encapsulated within the following class structure:

### 1. `NearestNeighborStrategyImpl.java`
This is the main class that implements the `OptimizationStrategy` interface.
*   **Time Complexity:** $O(N^2)$, where $N$ is the number of locations.
*   **Workflow:**
    1.  Takes the first element as the starting node.
    2.  Linearly searches the "pending" list for the node with the shortest distance `d` from the current node.
    3.  Adds that node to the optimal route and marks it as visited.
    4.  Repeats until all nodes have been visited.

### 2. `GeoUtils.java`
A static utility class (following the pattern) or component responsible for pure mathematics. It contains the `calculateDistanceKm` method which translates the above mathematical formulas into Java code using `Math.sin`, `Math.cos`, `Math.sqrt`, and `Math.atan2`.
