---
title: "Surabaya Sidoarjo Rail Bridge Inspection Route Optimization"
description: "Route optimization using metaheuristic algorithms (GA, ACO, PSO, SA) to minimize fuel costs and priority penalties for railway bridge inspections."
publishDate: "28 July 2026"
tags: ["Academic Projects", "Soft Computing", "Optimization", "Python"]
---

> *A comparative study of Genetic Algorithm, Ant Colony Optimization, Particle Swarm Optimization, and Simulated Annealing for priority-based TSP in railway bridge inspection.*

---

## 📚 Project Links

| Resource | Link |
|----------|------|
| **Demo Web** | [https://sc-bridge-inspection-dashboard.netlify.app](https://sc-bridge-inspection-dashboard.netlify.app) |
| **GitHub Repository** | [github.com/MCXGenesis/Soft-Computing-Bridge-Railway-Route-Optimization](https://github.com/MCXGenesis/Soft-Computing-Bridge-Railway-Route-Optimization/) |
| **Canva Presentation** | [Canva Link](https://107softcomputingfp.my.canva.site) |
<!-- | **Full Report** | [PDF Report](https://github.com/isan40-IS/bridge-railway/raw/main/docs/report.pdf) | -->

---

## 📖 Overview

Infrastructure bridges are critical pillars of national transportation networks. Routine inspections are mandatory to ensure public safety, but managing inspection logistics—especially fuel costs and the urgency of high-priority bridges—is a significant challenge.

This project formulates the problem as a **Priority-based TSP**, where the objective is to **minimize the total cost** consisting of:
- **Travel Cost**: Fuel consumption based on real-world road distances and traffic conditions.
- **Priority Penalty**: A penalty for visiting high-usage bridges (Main lines) later in the route.

The goal is to find a cost-efficient route that respects the urgency of critical bridges while minimizing operational costs.

### 📊 Dataset

The dataset consists of **38 railway bridges** in the Surabaya area, collected using **Overpass Turbo** (OpenStreetMap data) and exported as GeoJSON. Key attributes include:
- **Way ID**: Unique identifier for each bridge.
- **Coordinates**: Latitude and longitude.
- **Usage**: Priority level (`main` = 3, `branch` = 2, `industrial` = 1).

<!-- ![Bridge Map](docs/map.png) -->

## 🛠️ Methodology

### 1. Data Preprocessing

- **Depot Selection**: Bridge index 8 (way_id: `121501796`) is chosen as the depot due to its central geographic position.
- **Distance & Duration Matrix**: Built using the **OSRM Table API** (Open Source Routing Machine) to reflect real road networks. A Haversine fallback is available if OSRM is unreachable.
- **Fuel Cost Matrix**: Calculated using the **Stop-and-Go Factor (SAG)** to model fuel inefficiency caused by traffic conditions.  
  Formula:  
  `Fuel Cost = distance × (price per liter / km per liter) × SAG`  
  (with price = Rp 10,000/liter and efficiency = 10 km/liter).
- **Priority Penalty**: Defined as `λ × Σ(usage × position)`, where `λ = 100` (agreed upon across all algorithms).

### 2. Algorithms Implemented

| Algorithm | Description |
|-----------|-------------|
| **Genetic Algorithm (GA)** | A memetic GA with priority-seeded initialization, tournament selection, order crossover (OX), swap mutation, and 2-opt local search on the best individual each generation. |
| **Ant Colony Optimization (ACO)** | Simulates ant behavior using pheromone trails and heuristic information to construct routes, with pheromone evaporation to avoid premature convergence. |
| **Particle Swarm Optimization (PSO)** | Uses swap operators to move particles toward personal and global best solutions. |
| **Simulated Annealing (SA)** | A probabilistic local search that accepts worse solutions with a decreasing probability over time, controlled by a cooling schedule. |

### 3. Results

#### 🏆 Best Performing Algorithm: Genetic Algorithm (GA)

| Metric | GA Best | NN Baseline | Improvement |
|--------|---------|-------------|-------------|
| Total Cost | **Rp 274,269** | Rp 308,390 | **-11.06%** |
| Travel Cost | Rp 120,069 | Rp 134,490 | -10.7% |
| Priority Penalty | 1,542 | 1,739 | -11.3% |
| Main in Top 10 | 9/10 | 7/10 | +2 bridges |
| Main in Top 20 | 18/20 | 14/20 | +4 bridges |

#### 📊 Algorithm Comparison

| Algorithm | Total Cost | Travel Cost | Priority Penalty | Improvement vs NN |
|-----------|------------|-------------|------------------|-------------------|
| **Genetic Algorithm** | Rp 274,269 | Rp 120,069 | Rp 154,200 | **-11.06%** |
| **Simulated Annealing** | Rp 279,561 | Rp 124,861 | Rp 154,700 | -9.35% |
| **Ant Colony Optimization** | Rp 294,798 | Rp 123,998 | Rp 170,800 | -4.40% |
| **Particle Swarm Optimization** | Rp 302,344 | Rp 286,244 | Rp 16,100 | -1.96% |

> **Note**: GA outperformed all other algorithms due to its **priority-seeded initialization** and **memetic 2-opt local search**, which effectively balances exploration and exploitation.

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Required libraries: `numpy`, `pandas`, `matplotlib`, `json`, `csv`

### Installation

```bash
git clone https://github.com/isan40-IS/bridge-railway.git
cd bridge-railway
pip install -r requirements.txt
```

### Running the Code

#### 1. Prepare the data:

- Run the preprocessing script to generate the distance, duration, fuel cost, and SAG matrices.

- The preprocessed data is saved as tsp_data_100.pkl.

#### 2. Run an algorithm:

``` python
# Example: Run Genetic Algorithm
from ga import run_ga
best_route, best_cost = run_ga(tsp_data, lambda_priority=100)
```

#### 3. Visualize results:

- Generate route maps, convergence plots, and cost decomposition charts.

## 📁 Repository Structure
```text
bridge-railway/
├── data/
│   ├── bridge-railway-sby-ext.json   # Raw GeoJSON from Overpass Turbo
│   └── tsp_data_100.pkl              # Preprocessed data (distance, fuel, SAG matrices)
├── notebooks/
│   ├── 1_preprocessing.ipynb         # Data cleaning and matrix construction
│   ├── 2_ga.ipynb                    # Genetic Algorithm implementation
│   ├── 3_aco.ipynb                   # Ant Colony Optimization
│   ├── 4_pso.ipynb                   # Particle Swarm Optimization
│   └── 5_sa.ipynb                    # Simulated Annealing
├── src/
│   ├── ga.py
│   ├── aco.py
│   ├── pso.py
│   └── sa.py
├── docs/
│   ├── map.png                       # Bridge location map
│   └── convergence_plots/            # Algorithm convergence graphs
├── requirements.txt
└── README.md
```

## 📝 Key Findings
- GA's strength: Combining evolutionary search with local refinement (2-opt) allows it to find superior routes.

- SA's robustness: Although slower, SA reliably avoids local optima.

- ACO's stagnation: Pheromone evaporation parameters need tuning to prevent premature convergence.

- PSO's limitation: The simple swap operator lacks sufficient exploration after early convergence.

## 🤝 Contributors
- Fernandio Farrel Putra Susilo – 5026221102

- Hizkia Crisantino – 5026231040

- Naufal Maula Nabil – 5026231107

- Gabriel Hadi Melvanto Sihaloho – 5026231189

**Course**: Komputasi Lunak (Soft Computing)

**Department**: Sistem Informasi, Institut Teknologi Sepuluh Nopember (ITS)

**Academic Year**: 2026/2027

## 📚 References
- Goldberg, D. E. (1989). Genetic Algorithms in Search, Optimization, and Machine Learning.

- Kirkpatrick, S., Gelatt, C. D., & Vecchi, M. P. (1983). Optimization by Simulated Annealing. Science, 220(4598), 671-680.

- Maniezzo, V., Gambardella, L. M., & de Luigi, F. (2004). Ant Colony Optimization. In New Optimization Techniques in Engineering.

- Kennedy, J., & Eberhart, R. (1995). Particle Swarm Optimization. Proceedings of ICNN'95.

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
