# Wood-Pellet-Pile-Temperature-Simulator
A numerical simulation model for predicting the self-heating and spontaneous combustion risk of wood pellet piles caused by moisture adsorption and environmental dynamics.

# Wood Pellet Self-Heating Simulation Model

## Overview
This repository contains a Python-based numerical simulation tool designed to analyze the **self-heating phenomenon in wood pellet piles**. The model predicts the temperature evolution of the pile core by calculating the unsteady-state energy balance derived from moisture adsorption kinetics and thermodynamic principles.

This tool is particularly useful for assessing the risk of **spontaneous combustion** in biomass storage facilities under varying ambient temperature and humidity conditions.

## Key Features
* **Physics-Based Modeling:** Incorporates the **latent heat of vaporization** and the **differential heat of wetting** (based on Back, 1981) to calculate total heat generation.
* **Dynamic Moisture Equilibrium:** Utilizes **Simpson’s (1973) EMC model** with vapor pressure continuity to simulate real-time moisture exchange between the pile and the environment.
* **Environmental Sensitivity:** Accounts for ambient temperature ($T_{env}$) and relative humidity ($RH_{env}$) fluctuations.
* **Numerical Stability:** Implements an **Explicit Euler Method with temporal sub-stepping** (1-minute intervals) to ensure stability against rapid exothermic reactions.
* **Heat Loss Calculation:** Models heat dissipation using an effective overall heat transfer coefficient ($U_{eff}$) dependent on the pile radius.

## Mathematical Model
The simulation solves the following governing equation for the conservation of energy:

$$\rho C_p \frac{dT}{dt} = \dot{q}_{gen} - \dot{q}_{loss}$$

Where:
* **$\dot{q}_{gen}$ (Heat Generation):** Driven by moisture adsorption rate ($dM/dt$) and sorption energy ($L_v + H_w$).
* **$\dot{q}_{loss}$ (Heat Loss):** Governed by Newton's law of cooling considering the pile's surface-to-volume ratio ($A/V$).
* **Feedback Loop:** As pile temperature rises, the internal relative humidity ($RH_{pore}$) decreases, naturally limiting the equilibrium moisture content ($M_{eq}$) and preventing infinite temperature run-away.

## Input Data Format
The script requires a CSV file containing hourly weather data.
* **Format:** `.csv`
* **Columns:** `Time`, `Ambient_RH` (%), `Ambient_Temp` (°C)
* **Note:** The script automatically handles column renaming and missing value interpolation.

## Physical Parameters
The following constants are used in the simulation (customizable in the script):
* **Bulk Density ($\rho$):** $650 \, kg/m^3$ (ISO 17225-2 standard)
* **Specific Heat ($C_p$):** $1500 \, J/kg \cdot K$ (USDA Wood Handbook)
* **Initial Moisture Content:** Fixed at 6% (0.06) to simulate winter storage conditions.

## Output
The simulation generates a CSV file containing time-series data for:
1.  **Pile_Temp:** Core temperature of the pellet pile (°C).
2.  **Pile_Moisture:** Moisture content of the pellets (decimal, dry basis).
3.  **Net_Heat_Flux:** Net energy accumulation rate ($W/m^3$).
4.  **Ambient Conditions:** Reference environmental data.

## References
* **Back, E. L. (1981).** The bonding mechanism in hardboard manufacture review. *Holzforschung*.
* **Simpson, W. T. (1973).** Predicting equilibrium moisture content of wood. *Wood Science*.
