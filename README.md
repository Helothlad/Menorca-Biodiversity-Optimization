# Optimization Project I: Menorca Wildlife Conservation 🦔🌲

**Project Year:** 2025  
**Course:** Optimization I  
**Location:** Menorca, Spain (UNESCO Biosphere Reserve)

## 📖 Project Overview

This project focuses on the mathematical optimization of conservation strategies for four key mammal species in Menorca. Using **Integer Linear Programming (ILP)** and geospatial data analysis, the project aims to allocate a limited budget to expand habitats and establish wildlife corridors, ensuring genetic connectivity and population stability.

The solution is implemented in Python using **Google OR-Tools**, **GeoPandas**, and **Folium** for interactive visualization.

---

## 🐾 Focal Species

The project targets four species with distinct ecological requirements and interactions:

1.  **North African Hedgehog** (*Atelerix algirus*) - *Erizo moruno*
    * *Habitat:* Scrubland, agricultural mosaics.
2.  **European Pine Marten** (*Martes martes*) - *Marta*
    * *Habitat:* Forests, rocky terrain.
    * *Constraint:* Predator (cannot co-exist in the same cell with prey species in the model).
3.  **Garden Dormouse** (*Eliomys quercinus*) - *Lirón careto*
    * *Habitat:* Woodlands, orchards.
    * *Constraint:* Prey for the Pine Marten.
4.  **European Rabbit** (*Oryctolagus cuniculus*) - *Conejo europeo*
    * *Habitat:* Open pastures, scrubland.
    * *Constraint:* Prey for the Pine Marten.

---

## ⚙️ Mathematical Methodology

The optimization problem is tackled in two distinct phases to manage computational complexity:

### Phase 1: Habitat Expansion (The "Onion" Model)
**Objective:** Expand existing colonies into optimal adjacent cells to meet population targets while minimizing cost.
* **Algorithm:** Layered BFS ("Onion Layers") to limit the search space to $N$ steps from existing populations.
* **Solver:** SCIP (via OR-Tools).
* **Constraints:**
    * **Budget:** Total expansion cost $\le$ Global Budget.
    * **Connectivity:** New cells must be connected to an existing population source (Parent-Child logic).
    * **Predator-Prey:** *Martes martes* cannot be placed in the same cell as *Eliomys* or *Oryctolagus*.
    * **Suitability:** Species cannot expand into unsuitable terrain (e.g., Airports, Industrial zones).

### Phase 2: Wildlife Corridors (Network Design)
**Objective:** Connect isolated population clusters ("Islands") to ensure genetic diversity.
* **Algorithm:** Virtual Source/Sink Flow Model (Approximation of the Steiner Tree problem).
* **Logic:**
    * Identifies isolated clusters of populations.
    * Connects the largest cluster (Root) to the next largest clusters (Sinks).
    * Minimizes the cost of land acquisition for corridors.
* **Output:** Continuous paths of green "Corridor" cells connecting distinct habitats.

---

## 📊 Experiments & Benchmarking

The project includes a rigorous experimental phase to evaluate solver performance:

1.  **Statistical Significance:** Repeated runs (N=20) comparing **CBC** vs. **SCIP** solvers to account for variance in execution time.
2.  **Scalability Analysis:** Testing the model against dataset subsets (20%, 40%, 60%, 80%, 100%) to measure time complexity growth.
3.  **Hypothesis Testing:** T-Tests are applied to determine if one solver is statistically superior for this specific topology.

---

## 🛠️ Technology Stack

* **Python 3.12+**
* **Optimization:** `ortools` (Google Operations Research Tools)
* **Geospatial:** `geopandas`, `shapely`
* **Visualization:** `folium`, `matplotlib`, `seaborn`
* **Data Manipulation:** `pandas`, `numpy`
