# Wildfire Smoke PM2.5 Modeling with Convex Optimization and Low-Cost Sensors

This repository contains the code and resources for my **CSE 203B WI25 (Convex Optimization) final project**, which focuses on **modeling PM\textsubscript{2.5} data from wildfire smoke events** using **convex optimization** techniques on a graph. By representing sensor measurements as a graph signal (with nodes as sensor locations and edges reflecting spatial adjacency), I apply various convex regularizers (e.g., Laplacian smoothing, total variation) and compare them to classical interpolation baselines such as **Kriging**, **IDW**, and **Nearest Neighbor**. I also implement and evaluate iterative solvers like **ADMM** and **gradient descent** to solve the resulting optimization problems.

----

## Project Overview

- **Objective**:  
  Improve the reconstruction and denoising of wildfire PM\textsubscript{2.5} sensor data using:
  1. A convex optimization framework leveraging the graph Laplacian or TV-based regularization.
  2. Comparisons against classical geostatistical methods (Kriging, IDW) and naive baselines (Nearest Sensor, Simple Average).
  3. Real-world dataset from [Barkjohn *et al.* (2022)](https://www.mdpi.com/1424-8220/22/24/9669) on PurpleAir low-cost sensors, plus T640 references, processed for cross-validation experiments and parameter tuning.

- **Key Contributions**:
  1. A formal **primal-dual** problem statement with **KKT conditions** for the sensor-based interpolation task.
  2. A robust implementation of ADMM-based solvers and gradient-based algorithms for both **Laplacian** and **Total Variation** formulations.
  3. Detailed **cross-validation** results comparing convex methods to Kriging/IDW under varied hyperparameters ($\lambda$, distance thresholds, $k$-NN) and evaluating performance in extreme smoke scenarios.

- **Main Insights**:  
  1. **Kriging** often performs best in moderate PM\textsubscript{2.5} regimes, but the **ADMM** approach remains competitive, especially with parameter tuning and potential extensions (e.g., total variation).  
  2. Performance under **extreme smoke** (PM\textsubscript{2.5} $\gg 300$) reveals limitations due to sensor saturation and coverage gaps.  
  3. Graph-based methods are robust to moderate sensor removal, showing only small degradations even when 50% of sensors are withheld.

----

## Repository Structure

Below is a brief overview of the main files and directories:

```
.
├─ WildfireSmoke_PM25_ConvexOptimization_GraphInterpolation.ipynb
│       # Main Jupyter notebook with code for data loading, graph building,
│       # solver implementations (ADMM, gradient-based), baseline comparisons,
│       # cross-validation loops, parameter sweeps, and result visualizations.
│
├─ requirements.txt
│       # Only the libraries actually used in the notebook
│
├─ figures/
│   ├─ admm_confusion.png
│   ├─ admm_lam_sweep.png
│   ├─ admm_scatter_t640.png
│   ├─ admm_spatial_heatmap.png
│   ├─ AQS3yearT640-histogram.png
│   ├─ boxplot_errors.png
│   ├─ kriging_heatmap.png
│   ├─ sensor_map_usa.png
│   └─ solver_time_series*.png
│       # Various output figures of scatter plots, heatmaps, parameter sweeps, etc.
│
├─ latex-report/
│   ├─ main.tex
│   ├─ yourbib.bib
│   ├─ UCSD_CSE_203B_Final_Project.pdf
│       # Contains the LaTeX source and final PDF of the project report.
│
├─ README.md
```

----

## Installation & Environment

1. **Clone the repo**:

   ```bash
   git clone https://github.com/ChestnutKurisu/CSE203B_Final_Project_WI25.git
   cd CSE203B_Final_Project_WI25
   ```

2. **Optional**: Create and activate a Python virtual environment (recommended):
   ```bash
   python3 -m venv env
   source env/bin/activate
   ```

3. **Install packages**:  
   The project primarily uses **pandas**, **numpy**, **scipy**, **networkx**, **pykrige**, **matplotlib**, **seaborn**, **sklearn**, **folium**, **geopy**, **branca**, **plotly**, and **selenium** (for automated map screenshots).  

   Please install them with:
   ```bash
   pip install -r requirements.txt
   ```

4. **Jupyter**:  
   Ensure you have **jupyter** or **jupyterlab** installed to open the main notebook.

----

## Usage

1. **Open the main notebook**:

   ```bash
   jupyter notebook WildfireSmoke_PM25_ConvexOptimization_GraphInterpolation.ipynb
   ```

2. **Run Cells**:
   - The notebook will guide you through:
     1. **Data loading** of the CSVs containing PurpleAir and T640 references (paths in code may need adjusting).
     2. **Graph construction** (e.g., distance threshold or k-NN).
     3. **Solver** approaches: ADMM, gradient descent, Laplacian vs. TV.
     4. **Baseline** comparisons: IDW, Kriging, Nearest Sensor, Simple Average.
     5. **Evaluation**: LOOCV, parameter sweeps, and extreme smoke filtering.

3. **Results**:
   - The notebook automatically generates **figures** (saved in `figures/` by default) for:
     - Scatter plots of predicted vs. true PM\textsubscript{2.5}.
     - Spatial heatmaps comparing different interpolation methods.
     - Parameter-sweep tables for $\lambda$ or distance threshold variations.
     - Error distribution boxplots, confusion matrices for AQI categories, etc.

----

## Methodology in Brief

- **Convex Formulation**:  
  The method solves the minimization problem:
  $\min_{x\in \mathbb{R}^N} \frac{1}{2}\sum_{i\in \Omega}(x_i - y_i)^2 +\frac{\lambda}{2}x^T L x$
  under a graph Laplacian regularization (or a total variation penalty $\sum_{(i,j)\in E} |x_i-x_j|$ for piecewise-smooth fields).

- **Solvers**:
  1. **Gradient Descent** or **Proximal Updates** for the Laplacian-based smooth problem.
  2. **ADMM** for decoupling fidelity and regularization, especially with non-smooth TV.
  3. **Baseline**: **Kriging**, **IDW**, **Nearest**, **Simple Avg** for comparison.

- **Dataset**:  
  - Real-world CSVs from **Barkjohn et al.** with corrected PurpleAir sensor data, T640 monitors, and reference $\text{PM}_{2.5}$ measurements covering multiple US locations under wildfire smoke conditions.
  - Preprocessing merges lat/lon, timestamps, and filters out invalid or extreme saturations.

- **Performance Metrics**:  
  $\text{MAE}$, $\text{RMSE}$, correlation ($r$), and confusion matrices for AQI categories.

----

## Latex Report

For more in-depth explanation, derivations, references, and extended result discussion, check the **LaTeX report** in:
```
latex-report/
  ├─ main.tex
  ├─ yourbib.bib
  └─ UCSD_CSE_203B_Final_Project.pdf
```
Compile with `pdflatex main.tex` (or view the provided `UCSD_CSE_203B_Final_Project.pdf` directly).

----

## Contact

If you have any questions, feedback, or need assistance running the code, **please reach out** to me at:

**Email**: [psomane@ucsd.edu](mailto:psomane@ucsd.edu)

or open an issue on this repository.  
Thank you for your interest in this project!
