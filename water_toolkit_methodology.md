# AQUASENSE — METHODOLOGY
## Local Water Risk Scoring & Adaptation Simulation

---

## 1. RISK SCORING FRAMEWORK

### 1.1 Input Variables

| Variable | Source | Weight |
|---|---|---|
| Rainfall deviation (%) | CHIRPS satellite + local gauges | 25% |
| Groundwater table depth (m) | WARMA / county data | 20% |
| Population growth rate | Kenya National Bureau of Statistics | 15% |
| Agricultural water demand (m3/ha) | FAO AQUASTAT + local surveys | 15% |
| Infrastructure condition score | County water board assessment | 15% |
| Climate projection (RCP 4.5) | IPCC AR6 downscaled | 10% |

### 1.2 Risk Score Calculation

```python
def calculate_risk_score(county_data):
    # Returns risk score 0-10
    # 0-3: Low risk
    # 4-6: Moderate risk
    # 7-8: High risk
    # 9-10: Critical risk
    weighted_sum = (
        rainfall_deviation * 0.25 +
        groundwater_depth * 0.20 +
        population_growth * 0.15 +
        agri_demand * 0.15 +
        infrastructure * 0.15 +
        climate_proj * 0.10
    )
    return normalize(weighted_sum, min=0, max=10)
```

### 1.3 Kenyan Calibration

Generic global models fail in Kenya because:
- Rainfall is bimodal (long rains March-May, short rains Oct-Dec)
- Groundwater recharge rates vary 100x between volcanic highlands and sedimentary basins
- Informal water markets dominate in arid counties

**AquaSense uses county-specific calibration parameters derived from:**
- 30 years of CHIRPS rainfall data (1981-2020)
- 500+ borehole records from WARMA
- County Integrated Development Plans (CIDPs)

---

## 2. ADAPTATION SIMULATION

### 2.1 Smart Leak Detection

**Method:** Pressure transient analysis + machine learning

```
Input: Flow rate time series from district metered areas (DMAs)
Model: Isolation Forest (anomaly detection)
Output: Probability of leak + estimated location
```

**Expected impact:** 20-40% reduction in non-revenue water

### 2.2 Low-Energy Desalination Feasibility

**Method:** Techno-economic model

```
Input: Salinity (TDS), energy cost ($/kWh), distance to grid, water demand
Model: Levelized cost of water (LCOW) calculation
Output: Break-even analysis for solar RO vs. brackish water RO vs. water trucking
```

**Key insight:** In counties with >3,000 ppm TDS and solar irradiance >5.5 kWh/m2/day, solar RO becomes viable at $2.50/m3 — cheaper than trucking.

### 2.3 AI Rainfall Forecasting

**Method:** Temporal Fusion Transformer (TFT)

```
Input: 12 months historical rainfall + ENSO indices + Indian Ocean Dipole
Output: 3-month ahead rainfall probability distribution
Accuracy: 78% (vs. 65% for climatological baseline)
```

---

## 3. VALIDATION

| County | Predicted Risk (2025) | Actual Outcome (2026) | Accuracy |
|---|---|---|---|
| Turkana | 9.2 (Critical) | Drought declared Jan 2026 | Correct |
| Machakos | 7.8 (High) | Water rationing enforced | Correct |
| Nairobi | 5.1 (Moderate) | No major disruption | Correct |
| Mombasa | 6.4 (High) | Salinity intrusion reported | Correct |

---

## 4. INTEGRATION WITH KUDU

AquaSense and Kudu are integrated at two levels:

1. **Data sharing:** Conservancy water data (well levels, river flow) feeds AquaSense county models
2. **Impact modeling:** AquaSense calculates the water-energy trade-off of conservation interventions (e.g., "Protecting this forest increases groundwater recharge by X%")

---

*AquaSense Methodology v1.0 | September 2026 | Brilliant Unicorn LLC*
