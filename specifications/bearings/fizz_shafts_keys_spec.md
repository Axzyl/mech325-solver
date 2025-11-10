# CALCULATOR SPECIFICATION: Shafts and Keys Design

**Calculator Name**: fizz_shafts_keys
**Source Document**: `documents/bearings/fizz_shafts_keys.pdf`
**Pages**: 17 (pages 106-122 in original document)
**Date Created**: 2025-11-09
**Specification Version**: 1.0

---

## 1. CALCULATOR METADATA

### 1.1 Purpose
This calculator provides a comprehensive 31-step procedure for shaft design and key selection. It calculates minimum shaft diameters at critical locations based on forces from various power transmission components (gears, belts, chains), applies stress concentration factors, selects appropriate bearings, and sizes keys for torque transmission.

### 1.2 Scope
- **Inputs**: Shaft geometry, component locations, power, speed, gear types, material properties, reliability requirements
- **Outputs**: Minimum shaft diameters at each location, bearing selections, key dimensions, keyseat/keyway specifications
- **Target Users**: Mechanical engineers designing shafts for power transmission systems

### 1.3 Limitations
- Assumes standard bearing types (ball bearings)
- Limited to static or steady-state loading conditions
- Requires manual interpretation of shear/moment diagrams
- Graph lookups for endurance limit (sn) and size factor (Cs) require interpolation

### 1.4 Assumptions
- Default belt tension ratio k=5 for V-belts, k=3 for flat belts
- Default safety factor N=3 if not specified
- Default reliability 99% (CR=0.81)
- Default manufacturing: cold drawn if not specified
- Default material factor Cm=1.0 (wrought steel)
- Default stress type: bending (Cst=1.0)
- Default initial size factor Cs=0.8 (~2" diameter guess)
- Profile keyseats assumed unless specified

---

## 2. VARIABLES

### 2.1 Variable Definitions Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Input Variables - System** |
| P | Power | Power transmitted at each component | hp | User Input | Problem statement | Yes | Multiple values for multiple components |
| n | Angular speed | Rotational speed | rpm | User Input | Problem statement | Yes | Multiple values for gears/sheaves |
| N_design | Design safety factor | Safety factor for shaft design | - | User Input | Problem statement | Yes | Default = 3 if not given |
| **Calculated - Torque** |
| T | Torque | Torque at each component location | lbf·in | Calculated | Step 1: T = 63000P/n | Yes | Must verify ΣT = 0 |
| **Input Variables - Gear/Component Geometry** |
| D_pitch | Pitch diameter | Pitch diameter of gear/sheave | in | User Input | Problem statement | Yes | For force calculations |
| φ | Pressure angle | Pressure angle (spur/bevel gears) | deg | User Input | Problem statement | Yes | Typically 20° |
| φ_n | Normal pressure angle | Normal pressure angle (helical) | deg | User Input | Problem statement | Yes | For helical gears only |
| ψ | Helix angle | Helix angle (helical gears) | deg | User Input | Problem statement | Yes | For helical gears only |
| γ | Cone angle pinion | Cone angle of pinion (bevel) | deg | User Input | Problem statement | Yes | For bevel gears only |
| Γ | Cone angle gear | Cone angle of gear (bevel) | deg | User Input | Problem statement | Yes | For bevel gears only |
| k | Belt tension ratio | Ratio F1/F2 for belts | - | User Input | Problem statement | Yes | Default: 5 for V-belt, 3 for flat |
| **Calculated - Forces** |
| W_t | Tangential force | Tangential force from component | lbf | Calculated | Step 2: W_t = 2T/D | Yes | Multiple instances |
| W_r | Radial force | Radial force from component | lbf | Calculated | Step 2: varies by gear type | Yes | Multiple instances |
| W_x | Axial force | Axial force from component | lbf | Calculated | Step 2: varies by gear type | Yes | Helical/bevel only |
| F_shaft | Shaft force | Force on shaft from chain/belt | lbf | Calculated | Step 2: varies by type | Yes | Chain/belt only |
| **Calculated - Reaction Forces** |
| R_x | Reaction force X | Bearing reaction force, X-direction | lbf | Calculated | Step 4: ΣFx = 0 | Yes | At each bearing |
| R_y | Reaction force Y | Bearing reaction force, Y-direction | lbf | Calculated | Step 4: ΣFy = 0 | Yes | At each bearing |
| R_z | Reaction force Z | Bearing reaction force, Z-direction | lbf | Calculated | Step 4: ΣFz = 0 | Yes | If axial loads present |
| **Calculated - Internal Forces** |
| V_x | Shear force X | Shear force in X-direction | lbf | Calculated | Step 5: Shear diagram | Yes | At critical points |
| V_y | Shear force Y | Shear force in Y-direction | lbf | Calculated | Step 5: Shear diagram | Yes | At critical points |
| V | Resultant shear | Resultant shear force | lbf | Calculated | Step 8: V = √(Vx² + Vy²) | Yes | At critical points |
| M_x | Bending moment X | Bending moment about X-axis | lbf·in | Calculated | Step 6: M diagram | Yes | At critical points |
| M_y | Bending moment Y | Bending moment about Y-axis | lbf·in | Calculated | Step 6: M diagram | Yes | At critical points |
| M | Resultant moment | Resultant bending moment | lbf·in | Calculated | Step 8: M = √(Mx² + My²) | Yes | At critical points |
| T_point | Torque at point | Torque at specific shaft location | lbf·in | Calculated | Step 7: Torque diagram | Yes | At critical points |
| **Input Variables - Stress Concentration** |
| K_t | Stress concentration | Stress concentration factor | - | Table Lookup | Step 9: Based on feature type | Yes | Multiple instances |
| **Input Variables - Material Properties** |
| material_shaft | Shaft material | Material designation for shaft | - | User Input | Step 10 | Yes | e.g., "AISI 1045" |
| s_u | Ultimate strength | Tensile strength of shaft material | ksi | Table Lookup | Step 10: Material tables | Yes | - |
| s_y | Yield strength shaft | Yield strength of shaft material | ksi | Table Lookup | Step 10: Material tables | Yes | - |
| C_m | Material factor | Material correction factor | - | Table Lookup | Step 11: Table based on type | Yes | Default = 1.0 (wrought steel) |
| C_st | Stress type factor | Type of stress correction | - | User Input | Step 12: Bending or axial | Yes | Default = 1.0 (bending) |
| manuf_type | Manufacturing type | Manufacturing process | - | User Input | Step 13 | Yes | Cold drawn, machined, etc. |
| s_n | Endurance limit base | Base endurance limit from graph | ksi | Graph Lookup | Step 13: vs s_u and manuf | Yes | Yellow input |
| C_R | Reliability factor | Reliability correction factor | - | Table Lookup | Step 14: Table 5-3 | Yes | Default = 0.81 (99%) |
| C_s | Size factor | Size correction factor | - | Graph Lookup | Step 15, 20: Iterative | Yes | Initial guess 0.8 |
| s_n_prime | Actual endurance limit | Estimated actual endurance limit | ksi | Calculated | Step 16: sn·Cm·Cst·CR·Cs | Yes | - |
| **Calculated - Shaft Diameters** |
| D_min | Minimum diameter | Minimum shaft diameter | in | Calculated | Step 17: Two formulas | Yes | Multiple instances |
| D_retaining | Retaining ring diameter | Adjusted diameter for retaining ring | in | Calculated | Step 18: 1.06 × D_min | Yes | If retaining ring present |
| D_final | Final diameter | Most restrictive diameter | in | Calculated | Step 19: Maximum of overlapping | Yes | At each location |
| D_standard | Standard diameter | Rounded to standard size | in | Table Lookup | Step 25: Table A2-1 | Yes | Except bearing locations |
| **Bearing Selection Variables** |
| usage_rpm | Usage speed | Operating speed for life calc | rpm | User Input | Step 21a | Yes | - |
| usage_hrs_day | Usage hours/day | Hours per day of operation | hr/day | User Input | Step 21a | Yes | - |
| usage_days_month | Usage days/month | Days per month of operation | day/month | User Input | Step 21a | Yes | - |
| usage_years | Usage years | Expected lifetime in years | years | User Input | Step 21a | Yes | - |
| L_2 | Load life | Total revolutions over lifetime | rev | Calculated | Step 21a: Formula | Yes | - |
| bearing_reliability | Bearing reliability | Desired bearing reliability | % | User Input | Step 21b | Yes | Default 90% |
| C_R_bearing | Reliability constant | Bearing reliability adjustment | - | Table Lookup | Step 21b: Table 14-6 | Yes | Default 1.0 for 90% |
| L_1 | Catalog life | Adjusted catalog life | rev | Calculated | Step 21b: CR · 10⁶ | Yes | - |
| k_bearing | Load-life exponent | Bearing type exponent | - | User Input | Step 21c | Yes | 3.00 for ball bearings |
| P_2 | Actual radial force | Radial force at bearing | lbf | From Analysis | Step 21c: Reaction force | Yes | - |
| C_bearing | Basic load rating | Required bearing capacity | lbf | Calculated | Step 21c: Formula | Yes | For bearing selection |
| bearing_number | Bearing designation | Selected bearing model number | - | Table Lookup | Step 22: Table 14-3 | Yes | Yellow input |
| D_bore | Bearing bore diameter | Bore diameter of selected bearing | in | Table Lookup | Step 23: From bearing table | Yes | Becomes minimum shaft dia |
| **Key Selection Variables** |
| material_key | Key material | Material designation for key | - | User Input | Step 26 | Yes | Recommend SAE 1018 |
| s_y_key | Yield strength key | Yield strength of key material | ksi | Table Lookup | Step 26: Table 11-4 | Yes | - |
| W_key | Key width | Width of key | in | Table Lookup | Step 27: Table 11-1 vs D | Yes | Yellow input |
| H_key | Key height | Height of key | in | Table Lookup | Step 27: Table 11-1 vs D | Yes | Yellow input |
| L_min_key | Minimum key length | Minimum required key length | in | Calculated | Step 28: Formula(s) | Yes | Depends on key type |
| L_key | Standard key length | Rounded standard key length | in | Table Lookup | Step 30: Table A2-1 | Yes | Round up from L_min |
| **Keyseat/Keyway Variables** |
| Y | Chordal height | Chordal height of keyseat | in | Calculated | Step 31: Formula | Yes | - |
| S | Keyseat depth shaft | Depth of keyseat in shaft | in | Calculated | Step 31: Formula | Yes | - |
| T | Keyseat depth hub | Depth of keyseat in hub | in | Calculated | Step 31: Formula | Yes | - |
| C_allowance | Clearance allowance | Clearance for parallel/taper keys | in | User Input | Step 31 | Yes | 0.005 parallel, -0.020 taper |

### 2.2 Variable Categories by Source Type

**User Inputs (White)**: P, n, N_design, D_pitch, φ, φ_n, ψ, γ, Γ, k, material_shaft, C_st, manuf_type, usage_rpm, usage_hrs_day, usage_days_month, usage_years, bearing_reliability, k_bearing, material_key, C_allowance

**Table/Graph Lookups (Yellow)**: K_t, s_u, s_y, C_m, s_n, C_R, C_s, bearing_number, D_bore, s_y_key, W_key, H_key, L_key, D_standard

**Calculated (Green)**: T, W_t, W_r, W_x, F_shaft, R_x, R_y, R_z, V_x, V_y, V, M_x, M_y, M, T_point, s_n_prime, D_min, D_retaining, D_final, L_2, L_1, C_bearing, L_min_key, Y, S, T

---

## 3. EQUATIONS

### 3.1 Core Equations

**Step 1: Torque Calculation**
```latex
T = \frac{63000P}{n}
```
Where: P in hp, n in rpm, T in lbf·in
Constraint: ΣT = 0 (sum of all torques must balance)

**Step 2a: Spur Gear Forces**
```latex
W_t = \frac{2T}{D}
```
```latex
W_r = W_t \tan \phi
```

**Step 2b: Helical Gear Forces**
```latex
W_t = \frac{2T}{D}
```
```latex
W_r = W_t \frac{\tan \phi_n}{\cos \psi}
```
```latex
W_x = W_t \tan \psi
```

**Step 2c: Bevel Gear Forces**
```latex
W_t = \frac{2T}{D}
```
```latex
W_r = W_t \tan \phi \cos \Gamma = W_t \tan \phi \cos \gamma
```
```latex
W_x = W_t \tan \phi \sin \Gamma = W_t \tan \phi \sin \gamma
```

**Step 2d: Chain Sprocket Forces**
```latex
F_{shaft} = \frac{2T}{D}
```

**Step 2e: V-Belt Sheave Forces**
```latex
\frac{F_1}{F_2} = k \quad \text{(assume } k = 5 \text{ if not given)}
```
```latex
F_{shaft} = \frac{2T}{D} \frac{k + 1}{k - 1}
```

**Step 2f: Flat Belt Pulley Forces**
```latex
\frac{F_1}{F_2} = k \quad \text{(assume } k = 3 \text{ if not given)}
```
```latex
F_{shaft} = \frac{2T}{D} \frac{k + 1}{k - 1}
```

**Step 4: Equilibrium Equations**
```latex
\sum F_x = 0
```
```latex
\sum F_y = 0
```
```latex
\sum M_x = 0
```
```latex
\sum M_y = 0
```
```latex
\sum F_z = 0 \quad \text{(if axial load present)}
```

**Step 8: Resultant Forces**
```latex
V = \sqrt{V_x^2 + V_y^2}
```
```latex
M = \sqrt{M_x^2 + M_y^2}
```

**Step 16: Actual Endurance Limit**
```latex
s'_n = s_n C_m C_{st} C_R C_s
```

**Step 17a: Minimum Diameter (with torque/bending)**
```latex
D = \left( \frac{32N}{\pi} \sqrt{\left(\frac{K_t M}{s'_n}\right)^2 + \frac{3}{4}\left(\frac{T}{s_y}\right)^2} \right)^{1/3}
```

**Step 17b: Minimum Diameter (shear only, or if larger)**
```latex
D = \sqrt{\frac{2.94 K_t V N}{s'_n}}
```

**Step 18: Retaining Ring Diameter Adjustment**
```latex
D_{retaining} = 1.06 D
```

**Step 21a: Load Life Calculation**
```latex
L_2 = (\text{rpm}) \cdot \left(\frac{60 \text{ min}}{\text{hr}}\right) \left(\frac{\# \text{ hours}}{\text{day}}\right) \left(\frac{\# \text{ days}}{\text{month}}\right) \left(\frac{12 \text{ month}}{\text{year}}\right) (\# \text{ years})
```

**Step 21b: Adjusted Catalog Life**
```latex
L_1 = C_R \cdot 10^6
```

**Step 21c: Bearing Capacity Calculation**
```latex
\frac{L_2}{L_1} = \left(\frac{C}{P_2}\right)^k
```
```latex
C = P_2 \left(\frac{L_2}{C_R \cdot 10^6}\right)^{1/k}
```
Where: k = 3.00 for ball bearings, k = 3.33 for tapered roller bearings

**Step 28a: Key Length (square key, weakest material)**
```latex
L_{min} = \frac{4TN}{DW s_y}
```

**Step 28b: Key Length (rectangular key or shaft weakest)**
```latex
L_{min} = \max\left(\frac{4TN}{DH s_y}, \frac{4TN}{DW s_y}\right)
```

**Step 31: Keyseat/Keyway Geometry**

Chordal height:
```latex
Y = \frac{D - \sqrt{D^2 - W^2}}{2}
```

Depth of shaft keyseat:
```latex
S = D - Y - \frac{H}{2} = \frac{D - H + \sqrt{D^2 - W^2}}{2}
```

Depth of hub keyseat:
```latex
T = D - Y + \frac{H}{2} + C = \frac{D + H + \sqrt{D^2 - W^2}}{2} + C
```

Where:
- C = +0.005 in for parallel keys
- C = -0.020 in for taper keys
- D = Nominal shaft or bore diameter
- H = Nominal key height
- W = Nominal key width
- Y = Chordal height

### 3.2 Piecewise/Conditional Equations

**Stress Concentration Factor (K_t)**

Depends on feature type:
- **Keyseats**:
  - Profile keyseat: K_t = 2.0 (default)
  - Sled runner keyseat: K_t = 1.6

- **Shoulder Fillets**:
  - Sharp fillet: K_t = 2.5
  - Well-rounded fillet: K_t = 1.5 (assume for gears/sheaves)

- **Bearing Fits**:
  - Small diameter side: K_t = 1.5 (well-rounded fillet)
  - Bearing seat (middle): K_t = 1.0 (press fit)
  - Large diameter side: K_t = 2.5 (sharp fillet)

- **Retaining Rings**:
  - K_t = 3.0

**Material Factor (C_m)**

| Steel Type | C_m |
|------------|-----|
| Wrought steel | 1.00 |
| Cast steel | 0.80 |
| Powdered steel | 0.76 |
| Malleable cast iron | 0.80 |
| Gray cast iron | 0.70 |
| Ductile cast iron | 0.66 |

**Type-of-Stress Factor (C_st)**
- Bending stress: C_st = 1.0
- Axial tension: C_st = 0.8

**Reliability Factor (C_R)** - Table 5-3

| Desired Reliability | C_R |
|---------------------|-----|
| 0.50 | 1.0 |
| 0.90 | 0.90 |
| 0.99 | 0.81 |
| 0.999 | 0.75 |

**Bearing Reliability Constant (C_R_bearing)** - Table 14-6

| Reliability (%) | C_R | Life Designation |
|-----------------|-----|------------------|
| 90 | 1.0 | L₁₀ |
| 95 | 0.62 | L₅ |
| 96 | 0.53 | L₄ |
| 97 | 0.44 | L₃ |
| 98 | 0.33 | L₂ |
| 99 | 0.21 | L₁ |

**Size Factor (C_s)** - From Graph or Table 5-4

For D in inches:
- D ≤ 0.30: C_s = 1.0
- 0.30 < D ≤ 2.0: C_s = (D/0.3)^(-0.11)
- 2.0 < D < 10.0: C_s = 0.859 - 0.021·25·D

For D in mm:
- D ≤ 7.62: C_s = 1.0
- 7.62 < D ≤ 50: C_s = (D/7.62)^(-0.11)
- 50 < D < 250: C_s = 0.859 - 0.000·837·D

---

## 4. PROCEDURE STEPS

### 4.1 Main Design Procedure (31 Steps)

| Step | Action Type | Description | Inputs Required | Outputs Generated | Notes |
|------|-------------|-------------|-----------------|-------------------|-------|
| 1 | Calculate | Find torque at each component from power and speed; verify ΣT = 0 | P, n | T at each location | T = 63000P/n; balance check critical |
| 2 | Calculate | Compute forces from each component based on type | T, D_pitch, φ, φ_n, ψ, γ, Γ, k, component type | W_t, W_r, W_x, F_shaft | Different formulas for spur, helical, bevel, chain, V-belt, flat belt |
| 3 | Manual | Draw free body diagram of shaft with all forces decomposed into X and Y components | All forces from step 2 | FBD sketch | User action, not calculated |
| 4 | Calculate | Calculate reaction forces at bearings using equilibrium equations | All applied forces, shaft geometry | R_x, R_y, R_z at each bearing | ΣFx=0, ΣFy=0, ΣMx=0, ΣMy=0, ΣFz=0 |
| 5 | Manual | Draw shear diagrams for horizontal (x) and vertical (y) forces | Reaction forces, applied forces | V_x and V_y diagrams | User action with calculator support |
| 6 | Manual | Draw bending moment diagrams for x and y moments (integral of shear) | V_x, V_y diagrams | M_x and M_y diagrams | User action with calculator support |
| 7 | Manual | Draw torque diagram for shaft | Torques at components | T diagram | Gains/drops at gears, not bearings |
| 8 | Calculate | Compute resultant shear and moment at each critical point | V_x, V_y, M_x, M_y at points | V, M at critical points | V = √(Vx²+Vy²), M = √(Mx²+My²) |
| 9 | Lookup/Input | Find stress concentration factors for each feature | Feature types, locations | K_t values | Keyseats, fillets, bearings, retaining rings |
| 10 | Input/Lookup | Specify shaft material and find s_u, s_y | Material designation | s_u, s_y | Material tables (Mott's or guide) |
| 11 | Lookup | Apply material factor C_m based on steel type | Material type | C_m | Default wrought steel C_m=1.0 |
| 12 | Input | Apply type-of-stress factor C_st | Stress type | C_st | Usually bending, C_st=1.0 |
| 13 | Graph Lookup | Get base endurance limit s_n from graph based on manufacturing and s_u | s_u, manuf_type | s_n | Yellow input; default cold drawn |
| 14 | Table Lookup | Get reliability factor C_R from table | Desired reliability | C_R | Default 99%, C_R=0.81 |
| 15 | Input/Graph | Guess initial size factor C_s | Initial diameter estimate | C_s | Default C_s=0.8 (~2" diameter) |
| 16 | Calculate | Compute estimated actual endurance limit | s_n, C_m, C_st, C_R, C_s | s'_n | s'_n = sn·Cm·Cst·CR·Cs |
| 17 | Calculate | Compute minimum shaft diameter at each critical point | K_t, M, T, V, s'_n, s_y, N | D_min at each point | Two formulas: with torque/bending, or shear only |
| 18 | Calculate | Multiply retaining ring diameters by 1.06 | D_min at retaining rings | D_retaining | Only for locations with K_t=3 |
| 19 | Select | Identify most restrictive diameter at each shaft location | All D_min at same location | D_final per location | Take maximum of overlapping values |
| 20 | Decision | Consider 2nd iteration if diameters differ significantly from 2" guess | D_final values | Updated C_s | Recommended if diameters >> 2" |
| 21a | Calculate | Determine load life L_2 from usage description | rpm, hrs/day, days/month, years | L_2 (total revolutions) | Full lifetime calculation |
| 21b | Lookup/Calculate | Adjust catalog life L_1 for reliability if not 90% | Bearing reliability | C_R_bearing, L_1 | L_1 = CR · 10⁶ |
| 21c | Calculate | Calculate bearing capacity C using load/life relationship | L_2, L_1, P_2, k_bearing | C | k=3.00 ball, k=3.33 tapered |
| 22 | Table Lookup | Select suitable bearing from table using C and D_min | C, D_min at bearing | Bearing number | Table 14-3 (pages 9-11) |
| 23 | Lookup | Use selected bearing's bore diameter as minimum shaft diameter | Bearing number | D_bore | Overrides calculated D_min |
| 24 | Adjust | Round up shaft diameters to match or exceed bearing bore | D_bore, D_final | Updated D_final | Diameters must accommodate bearing |
| 25 | Table Lookup | Round shaft diameters (except bearing location) to standard sizes | D_final values | D_standard | Table A2-1; bearing stays at D_bore |
| 26 | Input/Lookup | Select key material and get s_y_key | Key material designation | s_y_key | Recommend SAE 1018; key should be weaker than shaft |
| 27 | Table Lookup | Select key size (W, H) using shaft diameter at component | D_standard at component | W_key, H_key | Table 11-1; yellow input |
| 28 | Calculate | Compute minimum required key length | T, D, W, H, N, s_y_key, key type | L_min_key | Formula depends on square vs rectangular, weakest material |
| 29 | Check | Verify key length is suitable (<2", ideally <1") | L_min_key | Pass/Fail | May need to increase shaft diameter |
| 30 | Table Lookup | Round key length up to standard size | L_min_key | L_key | Table A2-1 |
| 31 | Calculate | Compute keyseat and keyway dimensions | D, W, H, C_allowance | Y, S, T | Formulas for chordal height, depths |

### 4.2 Iteration Notes

**First Iteration Completion**: After step 19, user has initial minimum diameters

**Second Iteration (Step 20)**: If D_final values are significantly different from 2":
- Return to step 15
- Use actual D_final values to look up better C_s from graph/table
- Recalculate s'_n (step 16)
- Recalculate D_min (step 17)
- Continue through step 19 again

**Convergence Criterion**: Shaft diameters stabilize within acceptable tolerance

---

## 5. ADDITIONAL NOTES

### 5.1 Table References

**Table 5-3**: Approximate Reliability Factors, C_R (page 6)

**Table 5-4**: Size Factors (page 7) - Formulas for C_s vs diameter

**Table 14-3**: Dimensions for Single Row, Deep-groove Ball Bearings (pages 9-11)
- Columns: Bearing number, Bore d, Outside dia. D, Width B, Static C_0, Dynamic C, etc.
- Contains bearing specifications 6000-6326 series

**Table 14-6**: Life Adjustment Factors for Reliability, C_R (page 8)

**Table 15-5**: Shaft and Housing Fits for Bearings (pages 12-13)
- Section A: Shaft fits (ISO tolerance grades)
- Section B: Housing fits

**Table A2-1**: Preferred Basic Sizes (page 14)
- Fractional inches, decimal inches, SI metric mm
- For rounding diameters and lengths

**Table 11-4**: Examples of Materials Used for Keys (page 15)
- Material designation, tensile strength s_u, yield strength s_y

**Table 11-1**: Key Size vs. Shaft Diameter (page 16)
- US inch sizes and SI metric sizes
- Columns: Nominal shaft diameter, Key dimensions (Width W, Height H)

### 5.2 Graph References

**Endurance Limit Graphs** (page 5):
- (a) U.S. customary units: s_n (ksi) vs s_u (ksi)
- (b) SI metric units: s_n (MPa) vs s_u (MPa)
- Curves for: Polished, Ground, Machined or cold drawn, As-rolled, As-forged

**Size Factor Graph** (page 6):
- C_s vs Diameter (inches and mm)
- Non-linear decay curve from 1.0 to ~0.68

### 5.3 Diagrams and Visual Aids

**Spur Gear Force Direction Diagram** (page 1):
- Shows action forces (gear A drives gear B)
- Shows reaction forces (forces exerted on gear A by gear B)
- Illustrates W_r and W_t directions

**Shaft Assembly Diagram** (page 3):
- Shows typical shaft with components A, B, C, D
- Indicates locations of: gears, bearings, retaining rings, keyseats, fillets
- Diameter changes: D₁, D₂, D₃, D₄, D₅, D₆

**Keyseat/Keyway Geometry Diagrams** (page 17):
- (a) Chordal height Y
- (b) Depth of shaft keyseat S
- (c) Depth of hub keyseat T
- Includes formulas for each dimension

### 5.4 Special Considerations

**Wormgears**: Force calculation deferred to wormgear section (not detailed in this document)

**Axial Forces**: Bearings only have axial force component if there's an axial force from a gear/component

**Torque Diagram**: Only has gains/drops across gears/pinions, NOT at bearings

**Retaining Ring Placement**: Located on small diameter side of components (left of A, right of C in diagram)

**Bearing Fit K_t Values**: Three different values for left-center-right of bearing mounting

**Key Material Recommendation**: SAE 1018 Carbon steel suggested; key material should be weaker than shaft material for easier calculation

**Key Length Guideline**:
- >2" may be too large (consider increasing shaft diameter)
- 1-1.5" is good
- <1" is best (per professor's recommendation)

**Manufacturing Default**: If not specified, assume "cold drawn" for endurance limit lookup

---

## 6. IMPLEMENTATION READY CHECKLIST

### 6.1 Data Requirements
- [x] All equations documented in LaTeX format
- [x] All variables defined with units
- [x] Input/output relationships mapped
- [x] Table lookup requirements identified
- [x] Graph lookup requirements identified
- [x] Piecewise conditions documented

### 6.2 UI/UX Requirements
- [ ] Three-tier input system (white/yellow/green) implemented
- [ ] MathJax rendering for all equations
- [ ] PDF viewer with page navigation (17 pages)
- [ ] Session save/load/reset functionality
- [ ] Input validation for all user inputs
- [ ] Dynamic visibility for gear-type-specific inputs
- [ ] Visual feedback for iteration loops (steps 15-20)
- [ ] Table lookup helpers (9 tables total)
- [ ] Graph lookup helpers (2 graphs: s_n and C_s)

### 6.3 Calculation Logic
- [ ] Multi-instance variable handling (multiple gears/components)
- [ ] Equilibrium solver for reaction forces
- [ ] Shear/moment diagram calculation support
- [ ] Maximum value selection (step 19)
- [ ] Conditional formula selection (gear types, key types)
- [ ] Iteration detection and prompting (step 20)
- [ ] Unit consistency checks
- [ ] Torque balance verification (ΣT = 0)

### 6.4 Validation Requirements
- [ ] Constraint checks:
  - Torque balance (ΣT = 0)
  - Key length suitability (<2")
  - Bearing capacity C vs actual load P_2
- [ ] Range validation:
  - Safety factor N > 0
  - All diameters > 0
  - Reliability between 0 and 1
  - Angles in valid ranges
- [ ] Material compatibility (key weaker than shaft)

### 6.5 Output Requirements
- [ ] Minimum diameters at all critical points
- [ ] Final standard shaft diameters
- [ ] Selected bearing specifications
- [ ] Key dimensions (W, H, L)
- [ ] Keyseat/keyway dimensions (Y, S, T)
- [ ] Intermediate results (forces, moments, s'_n, etc.)

### 6.6 Known Challenges
- **High complexity**: 31-step procedure with nested dependencies
- **Multiple instances**: Need to handle multiple gears/components along shaft
- **Manual steps**: Steps 3, 5, 6, 7 involve diagram drawing (may provide calculation support)
- **Large tables**: Table 14-3 spans 3 pages (~40 bearing entries)
- **Graph lookups**: Two graphs require visual interpolation (could provide formula approximations)
- **Iteration**: Step 20 requires user judgment on when to re-iterate
- **Gear type branching**: Different force formulas for 7 component types
- **Key calculation branching**: Different formulas based on square/rectangular and weakest material

---

## 7. REVIEW CHECKLIST

- [x] All pages of PDF reviewed (17 pages)
- [x] All equations extracted and formatted
- [x] All variables catalogued with units
- [x] All tables identified and referenced
- [x] All graphs identified
- [x] Procedure steps sequenced correctly
- [x] Piecewise conditions documented
- [x] Special cases noted
- [x] Implementation challenges identified
- [x] Cross-references validated

---

**Specification Status**: ✅ COMPLETE - Ready for HTML implementation

**Estimated Implementation Complexity**: VERY HIGH
- 80+ variables
- 30+ equations
- 31-step sequential procedure
- 9 table lookups
- 2 graph lookups
- Multiple conditional branches
- Iteration requirement
- Multi-instance variable management

**Recommended Implementation Approach**:
1. Start with single-component simplified version
2. Add multi-component support incrementally
3. Implement diagram calculation helpers (not full graphical drawing)
4. Provide formula-based approximations for graph lookups where possible
5. Use accordion/tab structure for 31-step workflow
6. Clear visual indication of current step and completed steps
7. Separate sections for each component (gear A, gear B, etc.)
