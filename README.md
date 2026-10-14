# flood-evacuation-sensitivity-model
A multivariable MATLAB model evaluating flood evacuation routes using partial derivatives and population sensitivity analysis.
---
## 📌 Project Overview
During emergency flood scenarios, selecting the optimal evacuation route is critical. Traditional routing tools rely on static distances, but real-world congestion depends dynamically on population surges. 

This project models total evacuation time ($T$) as a multivariable function:
$$T = f(\text{Population}, \text{Distance}, \text{Velocity}, \text{Capacity})$$

Using MATLAB, the program takes user-defined population inputs ($X$) and evaluates multiple evacuation routes ($Y$) to recommend the safest path while computing the **partial derivative sensitivity ($\frac{\partial T}{\partial X}$)** for each route.

---

## 🧮 Mathematical Formulation

* **Total Evacuation Time Function:**
  $$T = \left(\frac{\text{Distance}}{\text{Average Velocity}}\right) \times \text{Bottleneck Factor} \times \left(1 + \frac{\text{Population}}{\text{Capacity}}\right)$$

* **Sensitivity Analysis (Partial Derivative):**
  To measure how aggressively a route congests when population ($X$) increases, we compute the partial derivative with respect to $X$:
  $$\frac{\partial T}{\partial X} = \frac{\text{Distance} \times \text{Bottleneck Factor}}{\text{Average Velocity} \times \text{Capacity}}$$
  * *Low $\frac{\partial T}{\partial X}$:* Route handles population scaling smoothly.
  * *High $\frac{\partial T}{\partial X}$:* Route is prone to severe bottlenecks under high density.

---

## 🛠️ MATLAB Implementation (`flood_rescue.m`)

The script loads infrastructure data, prompts for real-time population input, computes matrix operations, and outputs a ranked recommendation table.

```matlab
% Core snippet from flood_rescue.m
baseTerm = (data.Distance ./ data.AverageVelocity) .* data.BottleneckFactor;
data.EstimatedTime = baseTerm .* (1 + (userPop ./ data.Capacity));
data.Sensitivity_dT_dX = baseTerm ./ data.Capacity;
