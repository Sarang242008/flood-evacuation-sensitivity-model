# 🌊 4-Variable Flood Evacuation Route Recommender & Sensitivity Analysis

An advanced multivariable mathematical model built in **MATLAB** to optimize disaster evacuation planning by evaluating population congestion, distance delays, water depth impedance, and road capacities using partial derivatives.

---

## 📌 Project Overview
During emergency flood scenarios, selecting the optimal evacuation route requires analyzing multiple competing factors simultaneously. Traditional tools rely on static routing, but real-world bottlenecks depend dynamically on interacting physical conditions.

This project models total evacuation time ($T$) as a rigorous 4-variable multivariable function:
$$T = f(X, Y, Z, W)$$

Using MATLAB, the program takes live emergency inputs for population and water depth, evaluates multiple routes, computes the partial derivatives for all four variables to measure system sensitivity, and automatically recommends the safest path.

---

## 🧮 The 4 Independent Variables

1. **$X$ = Population:** Total number of evacuating citizens entering the zone.
2. **$Y$ = Route Distance:** Length of the evacuation path in kilometers.
3. **$Z$ = Water Depth Factor:** Flood severity multiplier (e.g., $1.0$ = Dry, $2.0$ = Deep/Impeded).
4. **$W$ = Route Capacity:** Baseline vehicle capacity limit of the infrastructure.

---

## 📐 Mathematical Formulation & Partial Derivatives

* **Total Evacuation Time Function:**
  $$T(X, Y, Z, W) = \left(\frac{Y}{\text{Velocity}}\right) \times Z \times \left(1 + \frac{X}{W}\right)$$

* **Sensitivity Analysis (First-Order Partial Derivatives):**
  To isolate and measure how each individual factor influences evacuation time while holding the others constant, the script computes:
  * **Population Sensitivity ($\frac{\partial T}{\partial X}$):** Measures crowd choke rates and traffic density scaling.
  * **Distance Sensitivity ($\frac{\partial T}{\partial Y}$):** Measures time delays per kilometer of route length.
  * **Water Depth Sensitivity ($\frac{\partial T}{\partial Z}$):** Measures how severely rising floodwaters slow down transit.
  * **Capacity Sensitivity ($\frac{\partial T}{\partial W}$):** Measures how infrastructure upgrades or limitations impact overall flow.

---

## 🛠️ MATLAB Implementation (`flood_rescue.m`)

The script loads infrastructure parameters, prompts for live emergency inputs, computes matrix-based time and sensitivity calculations, and outputs a ranked recommendation table.

```matlab
% Core 4-variable calculation snippet from flood_rescue.m
baseTerm = (data.Distance_Y ./ data.AverageVelocity) .* waterDepth_Z;
data.EstimatedTime = baseTerm .* (1 + (userPop_X ./ data.Capacity_W));

% Partial Derivatives
data.dT_dX = baseTerm ./ data.Capacity_W;
data.dT_dY = waterDepth_Z .* (1 + (userPop_X ./ data.Capacity_W)) ./ data.AverageVelocity;
data.dT_dZ = (data.Distance_Y ./ data.AverageVelocity) .* (1 + (userPop_X ./ data.Capacity_W));
data.dT_dW = - (baseTerm .* userPop_X) ./ (data.Capacity_W .^ 2);
