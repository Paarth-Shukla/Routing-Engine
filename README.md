# Intelligent Traffic Routing Engine using Graph Clustering

## Overview
Urban traffic congestion is a critical challenge in smart cities. Traditional algorithms like **Dijkstra** and **A\*** struggle with **large-scale road networks** and **real-time traffic fluctuations**.  

This project introduces a **Gravity-Based Clustering Traffic Routing Engine**, leveraging:
- **6G ultra-low latency** for real-time updates
- **Graph algorithms** for scalable pathfinding
- **Transit Node Enhanced Routing (TNECR)** and **Optimized Multi-Level Dijkstra (OMLD)** for efficiency

Our approach achieves a **97.4% reduction in computation time** with only a **5.48% increase in route length**, making it suitable for **city-scale deployments**.

---

## Project Architecture
```
OpenStreetMap Data → Weighted Graph (traffic + gravity)  
                   ↓
        Gravity-Based Clustering  
                   ↓
   ┌───────────────────────────┐
   │ Routing Algorithms        │
   │  • Transit Node Enhanced  │
   │  • Multi-Level Dijkstra   │
   └───────────────────────────┘
                   ↓
   Real-Time Route Suggestions
```

---

## Key Features
- **Gravity-Based Clustering** → partitions road networks into adaptive communities (15–100 nodes).
- **Transit Nodes** → central + border nodes for efficient inter-cluster routing.
- **Hybrid Pathfinding** → intra-cluster + inter-cluster routing for scalability.
- **Dynamic Traffic Simulation** → models congestion with sinusoidal + random variations.
- **6G Real-Time Integration** → supports ultra-low latency updates.

---

## Results
| Method     | Avg Query Time | Speedup     | Distance Loss |
|------------|---------------|-------------|---------------|
| Dijkstra   | 0.0544 s       | Baseline    | 0%            |
| TNECR      | 0.0014 s       | **97.4%** faster | 5.48%       |
| OMLD       | ~3× faster     | Major gain  | ~5%           |

**Scalable to 3,000+ intersections & 10,000+ roads**  
**Handles real-time traffic updates with minimal overhead**  

---

## Implementation Details

### **1. Graph Preparation**
- Extract road network via **OpenStreetMap (OSM)** using `osmnx`.
- Nodes = intersections, Edges = roads.
- Edge weight = distance × (1 + traffic_density/100).
- Gravity formula:
  ```python
  gravity(i, j) = influence(i) * influence(j) / distance(i, j)
  ```

### **2. Community Detection**
- Adaptive BFS-based clustering.
- Smaller clusters in dense areas for finer resolution.

### **3. Transit Node Enhanced Routing (TNECR)**
- Selects **central & border nodes** as transit points.
- Precomputes shortest paths between transit nodes.
- Efficient routing via transit-based hops.

### **4. Optimized Multi-Level Dijkstra (OMLD)**
- Builds a **supergraph of communities**.
- Uses **dynamic border node selection** for minimal crossing cost.
- Further reduces query time with little path distortion.

---

## How to Run

### **1. Clone the Repository**
```bash
git clone https://github.com/Paarth-Shukla/Routing-Engine.git
cd Routing-Engine
```

### **2. Install Dependencies**
```bash
pip install -r requirements.txt
```
**Key libraries:**  
- `osmnx` (road network extraction)  
- `networkx` (graph algorithms)  
- `numpy`, `scipy` (computations)  
- `matplotlib` (visualizations)  

### **3. Run the Engine**
#### Gravity-Based Clustering + Transit Node Routing
```bash
python traffic_best.py
```

#### Multi-Level Dijkstra Optimized Routing
```bash
python traffic_improved.py
```

---

## Benchmarking
Example benchmarking (50 trials):
```
Standard Dijkstra Avg Time: 0.0544s
TNECR Algorithm Avg Time: 0.0014s
Time Saved: 97.4%
Distance Loss: 5.48%
```

---

## Future Work
- Integrating **real-world traffic APIs** (Google Maps, MapMyIndia).
- Adding **Reinforcement Learning** for dynamic route optimization.
- Supporting **multi-modal transport** (cars, buses, bicycles).
- Deploying on **6G edge nodes** for smart city pilots.
