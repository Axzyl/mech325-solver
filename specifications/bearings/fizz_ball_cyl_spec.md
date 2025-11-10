# Calculator Specification: Ball and Cylindrical Roller Bearings

---

## 1. Metadata

- **Calculator Name:** Ball and Cylindrical Roller Bearings - Selection and Life Analysis
- **Source PDF:** fizz_ball_cyl.pdf
- **PDF Location:** `documents/bearings/fizz_ball_cyl.pdf`
- **Description:** Comprehensive bearing selection calculator for deep-groove ball bearings, angular-contact ball bearings, and cylindrical roller bearings. Handles radial-only loads and combined radial+thrust loads. Uses Weibull reliability analysis to calculate required load ratings (C10) based on design life, application factors, and manufacturer parameters (Timken or SKF). Includes iterative procedure for combined loading with equivalent radial load factors.
- **Complexity:** Complex
- **Tabs to Include:**
  - [x] Procedure (Two main procedures: Radial Only and Radial+Thrust, plus Tapered Roller section)
  - [x] General Equations (10+ equations for loads, life, ratings)
  - [x] Variables (Essential for 40+ variables)

**Rationale for Tab Structure:**
The PDF contains two complete design procedures (Radial Load Only with 9 steps, Radial and Thrust Loads with 14 iterative steps), plus a section on Tapered Roller Bearings. With 40+ variables, 10+ equations, and 5+ major table lookups including an iterative convergence procedure, all three tabs are necessary for effective workflow management.

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Bearing Type** | | | | | | | |
| bearing_type | Bearing Type | Deep-groove ball, Angular-contact ball, or Cylindrical roller | - | Dropdown | Page 1 | No | Affects which table and a value |
| manufacturer | Manufacturer | Timken (Mfr 1) or SKF (Mfr 2) | - | Dropdown | Page 2, Weibull table | No | Affects Weibull parameters |
| **Loading** | | | | | | | |
| F<sub>x</sub> | Force X-Component | Radial force in x-direction | lbf or N | User Input | Page 1 | No | For radial load calc |
| F<sub>y</sub> | Force Y-Component | Radial force in y-direction | lbf or N | User Input | Page 1 | No | For radial load calc |
| F<sub>R</sub> | Radial Load | Magnitude of radial force | lbf or N | Equation | EQ-FR | No | FR = √(Fx² + Fy²) |
| F<sub>r</sub> | Radial Load (alias) | Same as FR | lbf or N | Equation | = FR | No | Used in combined loading |
| F<sub>a</sub> | Axial/Thrust Load | Axial force component | lbf or N | User Input | Page 4, Step 4.2.3 | No | Only for combined loading |
| F<sub>D</sub> | Design Load | Load with application factor | lbf or N | Equation | EQ-FD | No | FD = af × FR (or Fe) |
| F<sub>e</sub> | Equivalent Radial Load | Combined load equivalent | lbf or N | Equation | EQ-FE | No | Fe = Xi V Fr + Yi Fa |
| **Speed and Torque** | | | | | | | |
| n<sub>D</sub> | Angular Speed | Rotational speed of bearing | rpm | User Input/Equation | EQ-ND | No | Can be calculated from H and T |
| H | Design Horsepower | Power transmitted | hp | User Input | Page 1, Step 1b | No | Optional input |
| T | Transmitted Torque | Torque on shaft | lbf·in | User Input/Equation | Page 1, Step 1a-b | No | From FBD or from H and nD |
| d | Distance | Moment arm for torque | in | User Input | Page 1, Step 1a | No | For T = F × d |
| **Reliability** | | | | | | | |
| R<sub>tot</sub> | Total Reliability | Reliability of entire assembly | % or decimal | User Input | Page 1, Step 3 | No | e.g., 0.99 for 99% |
| n_bearings | Number of Bearings | Count of bearings in assembly | - | User Input | Page 1, Step 3 | No | Integer ≥ 1 |
| R<sub>i</sub> | Individual Reliability | Reliability per bearing | % or decimal | Equation | EQ-RI | No | Ri = ⁿ√Rtot |
| R<sub>D</sub> | Design Reliability (alias) | Same as Ri | % or decimal | Equation | = Ri | No | Used in C10 formula |
| **Life** | | | | | | | |
| application_type | Application Type | Machinery classification | - | Dropdown | Table 11-4 | No | Determines design life |
| 𝓛<sub>D</sub> | Design Life (hours) | Expected operating hours | hours or kh | Table/User Input | Table 11-4 | Maybe | Based on application type |
| L<sub>D</sub> | Design Life (revolutions) | Life in number of revolutions | revolutions | Equation | EQ-LD-REV | No | LD = 60 × 𝓛D × nD |
| L<sub>10</sub> | Rating Life | Standard rating life | 10⁶ revolutions | Table/Constant | Page 2, Weibull table | No | Mfr 1: 90×10⁶, Mfr 2: 1×10⁶ |
| x<sub>D</sub> | Multiple of Rating Life | Life multiplier | - | Equation | EQ-XD | No | xD = LD / L10 |
| **Application Factor** | | | | | | | |
| application_class | Application Class | Load/impact classification | - | Dropdown | Table 11-5 | No | Precision, commercial, impact, etc. |
| a<sub>f</sub> | Application Factor | Load factor | - | Table | Table 11-5 | Maybe | Based on application_class |
| **Weibull Parameters** | | | | | | | |
| x<sub>0</sub> | Weibull X-naught | Weibull parameter | - | Table | Page 2, Weibull table | Maybe | Mfr 1: 0, Mfr 2: 0.02 |
| θ | Weibull Theta | Weibull parameter | - | Table | Page 2, Weibull table | Maybe | Mfr 1: 4.48, Mfr 2: 4.459 |
| b | Weibull b | Weibull parameter | - | Table | Page 2, Weibull table | Maybe | Mfr 1: 1.5, Mfr 2: 1.483 |
| a | Load-Life Exponent | Ball or roller exponent | - | Constant/Dropdown | Page 3, Step 7 | No | 3 for ball, 10/3 for roller |
| **Load Rating** | | | | | | | |
| C<sub>10</sub> | Calculated Load Rating | Required basic load rating | kN or lbf | Equation | EQ-C10 | No | Must select bearing with C10 ≥ this |
| C<sub>10_table</sub> | Bearing Load Rating | Catalog load rating | kN | Table | Tables 11-2, 11-3 | Maybe | From bearing selection tables |
| C<sub>0</sub> | Static Load Rating | Basic static load rating | kN | Table | Tables 11-2, 11-3 | Maybe | For combined loading |
| **Combined Loading Factors** | | | | | | | |
| V | Rotation Factor | Inner vs outer ring rotation | - | User Input/Constant | Page 4, Step 1 | No | V = 1 (inner rotates), V = 1.2 (outer rotates) |
| F<sub>a</sub>/C<sub>0</sub> | Normalized Axial Load | Ratio for factor lookup | - | Equation | EQ-FA-C0 | No | Used to find e, X, Y |
| e | Load Ratio Threshold | Threshold parameter | - | Table | Table 11-1 | Maybe | Based on Fa/C0 |
| X<sub>1</sub> | Radial Factor 1 | Low thrust radial factor | - | Table | Table 11-1 | Maybe | When Fa/(VFr) ≤ e |
| Y<sub>1</sub> | Thrust Factor 1 | Low thrust axial factor | - | Table | Table 11-1 | Maybe | When Fa/(VFr) ≤ e |
| X<sub>2</sub> | Radial Factor 2 | High thrust radial factor | - | Table/User Input | Table 11-1 | Maybe | When Fa/(VFr) > e, initially 0.56 |
| Y<sub>2</sub> | Thrust Factor 2 | High thrust axial factor | - | Table/User Input | Table 11-1, interpolate | Maybe | When Fa/(VFr) > e, initially 1.63 |
| X<sub>i</sub> | Selected Radial Factor | X1 or X2 based on condition | - | Conditional | Step 9 | No | Xi = X1 or X2 |
| Y<sub>i</sub> | Selected Thrust Factor | Y1 or Y2 based on condition | - | Conditional | Step 9 | No | Yi = Y1 or Y2 |
| F<sub>a</sub>/(VF<sub>r</sub>) | Load Ratio | Thrust to radial ratio | - | Equation | EQ-FA-VFR | No | Used to determine X, Y selection |
| **Bearing Dimensions** | | | | | | | |
| Bore | Bore Diameter | Inner diameter of bearing | mm | Table | Tables 11-2, 11-3 | Maybe | From bearing selection |
| OD | Outer Diameter | Outside diameter of bearing | mm | Table | Tables 11-2, 11-3 | Maybe | From bearing selection |
| Width | Bearing Width | Axial width of bearing | mm | Table | Tables 11-2, 11-3 | Maybe | From bearing selection |
| Fillet_Radius | Fillet Radius | Corner radius | mm | Table | Table 11-2 | Maybe | From bearing selection |
| d<sub>s</sub> | Shoulder Diameter (small) | Smaller shoulder diameter | mm | Table | Table 11-2 | Maybe | From bearing selection |
| d<sub>H</sub> | Shoulder Diameter (large) | Larger shoulder diameter | mm | Table | Table 11-2 | Maybe | From bearing selection |
| **Tapered Roller Bearing Variables** | | | | | | | |
| R<sub>xA</sub> | Reaction X at Bearing A | X-component of reaction | lbf or N | User Input | Page 6, Step 1 | No | For tapered roller |
| R<sub>yA</sub> | Reaction Y at Bearing A | Y-component of reaction | lbf or N | User Input | Page 6, Step 1 | No | For tapered roller |
| F<sub>rA</sub> | Radial Load at A | Radial load on bearing A | lbf or N | Equation | EQ-FRA | No | FrA = √(RxA² + RyA²) |
| R<sub>xB</sub> | Reaction X at Bearing B | X-component of reaction | lbf or N | User Input | Page 6, Step 1 | No | For tapered roller |
| R<sub>yB</sub> | Reaction Y at Bearing B | Y-component of reaction | lbf or N | User Input | Page 6, Step 1 | No | For tapered roller |
| F<sub>rB</sub> | Radial Load at B | Radial load on bearing B | lbf or N | Equation | EQ-FRB | No | FrB = √(RxB² + RyB²) |
| **Iteration Tracking** | | | | | | | |
| iteration | Iteration Count | Number of iterations | - | Counter | Steps 6-14 | No | For convergence tracking |
| old_Fe | Previous Equivalent Load | Fe from previous iteration | lbf or N | Storage | Step 12 | No | For ratio calculation |
| old_C10 | Previous Load Rating | C10 from previous iteration | kN | Storage | Step 12 | No | For ratio calculation |
| converged | Convergence Flag | Same bearing selected twice? | boolean | Check | Step 14 | No | Stop condition |

**Notes:**
- Many variables only apply for specific procedures (radial only vs. radial+thrust)
- Combined loading procedure requires iteration until convergence
- Weibull parameters depend on manufacturer selection
- Table 11-1 requires interpolation for Y2 factor
- a value (3 or 10/3) depends on bearing type

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Speed and Torque** | | | | | |
| EQ-ND | `n_D = \frac{63025H}{T}` | Angular speed from power | H, T | nD | If H given |
| **Loading - Radial** | | | | | |
| EQ-FR | `F_R = \sqrt{F_x^2 + F_y^2}` | Radial load magnitude | Fx, Fy | FR | Pythagorean |
| EQ-FD | `F_D = a_f F_R` | Design load (radial only) | af, FR | FD | With application factor |
| **Loading - Combined** | | | | | |
| EQ-FE | `F_e = X_i V F_r + Y_i F_a` | Equivalent radial load | Xi, V, Fr, Yi, Fa | Fe | Combined loading |
| EQ-FD-COMB | `F_D = a_f F_e` | Design load (combined) | af, Fe | FD | With application factor |
| EQ-FA-C0 | `\frac{F_a}{C_0}` | Normalized axial load | Fa, C0 | Fa/C0 | For table lookup |
| EQ-FA-VFR | `\frac{F_a}{V F_r}` | Load ratio | Fa, V, Fr | Fa/(VFr) | Compared to e |
| **Reliability** | | | | | |
| EQ-RI | `R_i = \sqrt[n]{R_{tot}}` | Individual bearing reliability | Rtot, n | Ri | n-th root |
| **Life** | | | | | |
| EQ-LD-REV | `L_D = 60 \mathcal{L}_D n_D` | Design life in revolutions | 𝓛D, nD | LD | 𝓛D in hours |
| EQ-XD | `x_D = \frac{L_D}{L_{10}} = \frac{60 \mathcal{L}_D n_D}{L_{10}}` | Multiple of rating life | LD (or 𝓛D, nD), L10 | xD | - |
| **Load Rating** | | | | | |
| EQ-C10 | `C_{10} = a_f F_D \left[\frac{x_D}{x_0 + (\theta - x_0)[\ln(1/R_D)]^{1/b}}\right]^{1/a}` | Required load rating | af, FD, xD, x0, θ, b, RD, a | C10 | Weibull-based |
| **Iteration (Combined Loading)** | | | | | |
| EQ-C10-UPDATE | `\text{new } C_{10} = \frac{\text{new } F_e}{\text{old } F_e}(\text{old } C_{10})` | Updated load rating | new Fe, old Fe, old C10 | new C10 | Shortcut calculation |
| **Interpolation for Y2** | | | | | |
| EQ-INTERP-M | `m = \frac{y_1 - y_2}{x_1 - x_2}` | Interpolation slope | y1, y2, x1, x2 | m | Linear interpolation |
| EQ-INTERP-B | `b = y - mx` | Interpolation intercept | y, m, x | b | Using one point |
| EQ-INTERP-Y2 | `Y_2 = m \times (F_a/C_0) + b` | Interpolated Y2 value | m, Fa/C0, b | Y2 | Final interpolation |
| **Tapered Roller Bearings** | | | | | |
| EQ-FRA | `F_{rA} = \sqrt{R_{xA}^2 + R_{yA}^2}` | Radial load at bearing A | RxA, RyA | FrA | For tapered roller |
| EQ-FRB | `F_{rB} = \sqrt{R_{xB}^2 + R_{yB}^2}` | Radial load at bearing B | RxB, RyB | FrB | For tapered roller |

**Notes:**
- EQ-C10 has piecewise constant a: 3 for ball bearings, 10/3 for roller bearings
- Weibull parameters (x0, θ, b, L10) are manufacturer-specific from table
- Interpolation equations (EQ-INTERP-*) are used only in combined loading procedure
- Fe calculation uses conditional Xi and Yi based on Fa/(VFr) > e check

---

## 4. Procedure Steps

### 4.2.2 Design Selection - Radial Load Only

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Step 1** | **Find angular speed nD** | | | |
| 1a | Option A: Calculate torque from FBD | User Input | T = F × d from free body diagram | Torque causing shaft rotation |
| 1b-1 | Option B: Input design horsepower | User Input | Enter H in hp | If H is given |
| 1b-2 | Calculate nD from horsepower | Equation | nD = 63025H / T | Use EQ-ND |
| **Step 2** | **Find radial load FR** | | | |
| 2-1 | Input radial force components | User Input | Enter Fx, Fy | In lbf or N |
| 2-2 | Calculate radial load magnitude | Equation | FR = √(Fx² + Fy²) | Use EQ-FR |
| **Step 3** | **Calculate individual bearing reliability** | | | |
| 3-1 | Input total assembly reliability | User Input | Enter Rtot (e.g., 0.99 for 99%) | If given |
| 3-2 | Input number of bearings | User Input | Enter n_bearings | Integer ≥ 1 |
| 3-3 | Calculate individual reliability | Equation | Ri = ⁿ√Rtot | Use EQ-RI |
| **Step 4** | **Get bearing design life** | | | |
| 4-1 | Select application type | Dropdown | Choose from Table 11-4 | Instruments, Aircraft, Machinery, etc. |
| 4-2 | Get design life 𝓛D | Table Lookup | Table 11-4 based on application type | Yellow input, in kh (kilohours) |
| **Step 5** | **Calculate multiple of rating life** | | | |
| 5-1 | Select manufacturer | Dropdown | Timken (Mfr 1) or SKF (Mfr 2) | Affects Weibull parameters |
| 5-2 | Get Weibull parameters | Table | Get L10, x0, θ, b from Weibull table | Yellow input, based on manufacturer |
| 5-3 | Calculate LD in revolutions | Equation | LD = 60 × 𝓛D × nD | Use EQ-LD-REV |
| 5-4 | Calculate xD | Equation | xD = LD / L10 | Use EQ-XD |
| **Step 6** | **Get application factor** | | | |
| 6-1 | Select application class | Dropdown | Choose from Table 11-5 | Precision, Commercial, Impact, etc. |
| 6-2 | Get application factor af | Table Lookup | Table 11-5 based on application class | Yellow input |
| **Step 7** | **Calculate load rating C10** | | | |
| 7-1 | Calculate design load FD | Equation | FD = af × FR | Use EQ-FD |
| 7-2 | Set RD = Ri | Equation | RD = Ri from Step 3 | Design reliability |
| 7-3 | Select bearing type | Dropdown | Deep-groove ball, Angular-contact ball, Cylindrical roller | Determines a value |
| 7-4 | Set a value | Conditional | a = 3 (ball), a = 10/3 (roller) | Based on bearing type |
| 7-5 | Calculate C10 | Equation | Use EQ-C10 with Weibull parameters | Complex formula |
| **Step 8** | **Select bearing from tables** | | | |
| 8-1 | Choose appropriate table | Conditional | Table 11-2 (ball) or Table 11-3 (cylindrical roller) | Based on bearing type |
| 8-2 | Select bearing with C10 ≥ calculated | Table Lookup | Find row where C10_table ≥ C10 | Yellow input, multiple options |
| 8-3 | Record bearing dimensions | Table | Note Bore, OD, Width, (and shoulders for ball bearings) | For reference |
| **Step 9** | **Done!** | | | |
| 9 | Display selected bearing | Output | Show bearing specs and calculated values | - |

### 4.2.3 Design Selection - Radial and Thrust Loads

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Step 1** | **Determine rotation factor V** | | | |
| 1-1 | Select which ring rotates | Dropdown | Inner ring or Outer ring | User choice |
| 1-2 | Set V value | Conditional | V = 1 (inner rotates), V = 1.2 (outer rotates) | - |
| **Step 2** | **Initial assumptions (first iteration)** | | | |
| 2-1 | Assume Fa/(VFr) > e | Assumption | Assume high thrust condition | Will verify later |
| 2-2 | Set initial X2 | User Input/Default | X2 = 0.56 (middle value) | Initial guess |
| 2-3 | Set initial Y2 | User Input/Default | Y2 = 1.63 (middle value) | Initial guess |
| 2-4 | Set Xi = X2, Yi = Y2 | Assignment | Use assumed values | For first iteration |
| **Step 3** | **Calculate equivalent load with assumptions** | | | |
| 3-1 | Input radial load Fr | User Input | Enter Fr (or calculate from Fx, Fy) | In lbf or N |
| 3-2 | Input axial load Fa | User Input | Enter Fa | In lbf or N |
| 3-3 | Calculate Fe | Equation | Fe = Xi V Fr + Yi Fa | Use EQ-FE |
| **Step 4** | **Calculate load rating C10** | | | |
| 4-1 | Follow Steps 1-7 from Radial Only | Reference | Use Fe instead of FR for FD calculation | FD = af × Fe |
| 4-2 | Calculate C10 | Equation | Use EQ-C10 with FD = af × Fe | Same Weibull formula |
| **Step 5** | **Select initial bearing** | | | |
| 5-1 | Select bearing with C10 ≥ calculated | Table Lookup | Tables 11-2 or 11-3 | Yellow input |
| **Step 6** | **Find C0 for chosen bearing** | | | |
| 6-1 | Get C0 from table | Table Lookup | Tables 11-2 or 11-3, same row as C10 | Yellow input |
| **Step 7** | **Calculate Fa/C0** | | | |
| 7-1 | Calculate ratio | Equation | Fa/C0 | Use EQ-FA-C0 |
| **Step 8** | **Find e from Table 11-1** | | | |
| 8-1 | Find closest Fa/C0 in table | Table Lookup | Table 11-1, match or interpolate | Yellow input |
| 8-2 | Get corresponding e value | Table | From same row as Fa/C0 | Yellow input |
| **Step 9** | **Check condition and select factors** | | | |
| 9-1 | Calculate Fa/(VFr) | Equation | Use EQ-FA-VFR | - |
| 9-2 | Compare to e | Conditional Check | Is Fa/(VFr) > e? | Determines X, Y selection |
| 9-3a | If Fa/(VFr) > e: Use X2, Y2 | Conditional | Will look for Y2 values | High thrust |
| 9-3b | If Fa/(VFr) ≤ e: Use X1, Y1 | Conditional | Will look for Y1 values | Low thrust |
| **Step 10** | **Interpolate to find Y2 (if high thrust)** | | | |
| 10-1 | Find two closest Fa/C0 values in table | Table Lookup | Table 11-1, bracket actual Fa/C0 | x1, x2 |
| 10-2 | Get corresponding Y2 values | Table | Y2 values for x1, x2 | y1, y2 |
| 10-3 | Calculate slope m | Equation | m = (y1 - y2) / (x1 - x2) | Use EQ-INTERP-M |
| 10-4 | Calculate intercept b | Equation | b = y - mx (using one point) | Use EQ-INTERP-B |
| 10-5 | Calculate interpolated Y2 | Equation | Y2 = m × (Fa/C0) + b | Use EQ-INTERP-Y2 |
| 10-6 | Get X2 from table | Table | X2 = 0.56 (constant for all rows) | From Table 11-1 |
| 10-7 | Set Xi = X2, Yi = Y2 | Assignment | Use interpolated values | - |
| **Step 11** | **Recalculate Fe with new Y2** | | | |
| 11-1 | Calculate new Fe | Equation | Fe = Xi V Fr + Yi Fa | Use EQ-FE with new Yi |
| **Step 12** | **Calculate new C10** | | | |
| 12-1 | Use shortcut formula | Equation | new C10 = (new Fe / old Fe) × old C10 | Use EQ-C10-UPDATE |
| **Step 13** | **Select bearing with new C10** | | | |
| 13-1 | Select bearing | Table Lookup | Tables 11-2 or 11-3, C10_table ≥ new C10 | Yellow input |
| **Step 14** | **Check for convergence** | | | |
| 14-1 | Compare to previous selection | Comparison | Same bearing as in Step 5 (or previous iteration)? | - |
| 14-2a | If same: Converged! | Success | Done! | Display final bearing |
| 14-2b | If different: Repeat | Iteration | Go back to Step 6 with new bearing | Iterate until convergence |

### 4.3 Tapered Roller Bearings (Partial - page 6 only)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Step 1** | **Calculate radial loads on each bearing** | | | |
| 1-1 | Input reaction components at A | User Input | Enter RxA, RyA | In lbf or N |
| 1-2 | Calculate FrA | Equation | FrA = √(RxA² + RyA²) | Use EQ-FRA |
| 1-3 | Input reaction components at B | User Input | Enter RxB, RyB | In lbf or N |
| 1-4 | Calculate FrB | Equation | FrB = √(RxB² + RyB²) | Use EQ-FRB |
| **Step 2+** | **(Procedure continues beyond visible pages)** | | | |
| ... | ... | ... | ... | PDF continues |

**Notes:**
- Procedure 4.2.2 (Radial Only) is straightforward with no iteration
- Procedure 4.2.3 (Radial+Thrust) requires iteration until same bearing selected twice (Steps 6-14 loop)
- Tapered Roller Bearings section (4.3) continues beyond visible pages
- Interpolation in Step 10 is critical for accurate Y2 value

---

## 5. Additional Notes

### Piecewise Equations

**Load-Life Exponent (a):**
- a = 3 for ball bearings (deep-groove, angular-contact)
- a = 10/3 for roller bearings (cylindrical, tapered)

**Rotation Factor (V):**
- V = 1 when inner ring rotates
- V = 1.2 when outer ring rotates

**Equivalent Load Factors (Xi, Yi):**
- Use X1, Y1 when Fa/(VFr) ≤ e (low thrust)
- Use X2, Y2 when Fa/(VFr) > e (high thrust)
- From Table 11-1, piecewise based on Fa/C0 ratio

**Weibull Parameters:**
- **Manufacturer 1 (Timken):** L10 = 90×10⁶ rev, x0 = 0, θ = 4.48, b = 1.5
- **Manufacturer 2 (SKF):** L10 = 1×10⁶ rev, x0 = 0.02, θ = 4.459, b = 1.483

### Validation Rules

**Convergence (Combined Loading):**
- Iteration must continue until same bearing is selected twice in a row
- Maximum iterations should be limited (suggest 10-20 to prevent infinite loops)

**C10 Selection:**
- C10_table ≥ C10_calculated (bearing must meet or exceed requirement)

**Positive Values:**
- All loads (FR, Fa, Fe) > 0
- All speeds (nD) > 0
- Reliability: 0 < Ri, Rtot < 1 (or 0% < value < 100%)

**Reasonable Ranges:**
- xD typically 0.1 to 100 (multiple of rating life)
- af typically 1.0 to 3.0 (application factor)
- V = 1 or 1.2 only

### Standard Values / Constants

**Typical Values:**
- Rtot = 0.90 to 0.99 (90% to 99% reliability)
- af = 1.0 to 1.5 for most machinery
- V = 1 (most common - inner ring rotates)
- Initial guesses for combined loading: X2 = 0.56, Y2 = 1.63

**Constants:**
- Conversion: 63025 (for horsepower to rpm with torque in lbf·in)

### Dropdown Options

**bearing_type (Bearing Type):**
- Deep-groove ball bearing (a = 3, Table 11-2)
- Angular-contact ball bearing (a = 3, Table 11-2)
- Cylindrical roller bearing (a = 10/3, Table 11-3)
- Tapered roller bearing (a = 10/3, separate section)

**manufacturer (Manufacturer):**
- Timken (Manufacturer 1) - Common for tapered roller bearings
- SKF (Manufacturer 2) - Common for ball and straight roller bearings

**application_type (from Table 11-4):**
- Instruments and apparatus for infrequent use (0.5 kh)
- Aircraft engines (0.5-2 kh)
- Machines for short/intermittent operation, minor importance (4-8 kh)
- Machines for intermittent service, reliable operation important (8-14 kh)
- Machines for 8-h service, not always fully utilized (14-20 kh)
- Machines for 8-h service, fully utilized (20-30 kh)
- Machines for continuous 24-h service (50-60 kh)
- Machines for continuous 24-h service, extreme reliability (100-200 kh)

**application_class (from Table 11-5):**
- Precision gearing (af = 1.0-1.1)
- Commercial gearing (af = 1.1-1.3)
- Applications with poor bearing seals (af = 1.2)
- Machinery with no impact (af = 1.0-1.2)
- Machinery with light impact (af = 1.2-1.5)
- Machinery with moderate impact (af = 1.5-3.0)

**ring_rotation (for V factor):**
- Inner ring rotates (V = 1)
- Outer ring rotates (V = 1.2)

### Future Enhancement Opportunities

**Interactive Table Lookups:**
- 𝓛D from Table 11-4 (could auto-fill based on application_type)
- Weibull parameters from table (could auto-fill based on manufacturer)
- af from Table 11-5 (could auto-fill based on application_class)
- C10, C0 from Tables 11-2 or 11-3 (dropdown of available bearings meeting C10 requirement)
- e, X, Y from Table 11-1 (auto-lookup and interpolation based on Fa/C0)

**Iteration Automation:**
- Auto-run iteration loop for combined loading procedure
- Display convergence progress (iteration count, Fe changes, bearing changes)
- Auto-stop when converged

**Bearing Comparison:**
- Show multiple bearing options meeting C10 requirement
- Compare dimensions, costs, availability
- Highlight optimal selection

**Visualization:**
- Plot C10 requirement vs. available bearings
- Show reliability curves (Weibull distribution)
- Display bearing cross-section with dimensions

### Special Considerations

**Table Complexity:**
- **Table 11-1:** Requires interpolation for Y2 - 12 rows, complex lookup
- **Table 11-2:** Large table with multiple columns (Bore 10-95mm, multiple C10 values for deep-groove vs. angular-contact)
- **Table 11-3:** Large table for cylindrical roller bearings (Bore 25-150mm, 02-Series and 03-Series)
- All tables use metric units (mm, kN) - may need unit conversion if user inputs imperial

**Iteration Convergence:**
- Combined loading procedure (4.2.3) requires iteration until same bearing selected twice
- Could take 2-10 iterations typically
- Need safeguard against infinite loops
- Fe and C10 values should converge

**Interpolation Precision:**
- Y2 interpolation (Step 10) is critical for accuracy
- Linear interpolation sufficient per PDF
- Need to handle edge cases (Fa/C0 outside table range)

**Manufacturer Selection:**
- Weibull parameters significantly different between manufacturers
- Choice affects C10 calculation substantially
- PDF notes Timken for tapered roller, SKF for ball/straight roller

**Incomplete Section:**
- Tapered Roller Bearings section (4.3) starts on page 6 but clearly continues
- Specification covers visible portion only
- May need to update when full PDF available

### Calculation Order / Dependencies

**Radial Load Only (4.2.2):**
```
Inputs: Fx, Fy, H (or T), Rtot, n_bearings, application_type, manufacturer, application_class, bearing_type →

FR = √(Fx² + Fy²) →
nD = 63025H / T (if H given) →
Ri = ⁿ√Rtot →

Table 11-4 → 𝓛D
Table (Weibull) → L10, x0, θ, b
Table 11-5 → af →

LD = 60 × 𝓛D × nD →
xD = LD / L10 →
FD = af × FR →
Set a (3 or 10/3) based on bearing_type →

C10 = af FD [xD / (x0 + (θ - x0)[ln(1/Ri)]^(1/b))]^(1/a) →

Tables 11-2 or 11-3 → Select bearing with C10_table ≥ C10 →
Done!
```

**Radial and Thrust Loads (4.2.3) - Iterative:**
```
Inputs: Fr, Fa, V (or ring_rotation), ... (same as radial only) →

Initial: X2 = 0.56, Y2 = 1.63, assume Fa/(VFr) > e →

LOOP:
  Fe = X2 V Fr + Y2 Fa →
  FD = af × Fe →
  C10 = [formula with FD] →

  Select bearing from table →
  Get C0 from table →

  Fa/C0 = Fa / C0 →
  Table 11-1 → e (based on Fa/C0) →

  Fa/(VFr) > e? →
    If yes: Use X2, Y2 columns
    If no: Use X1, Y1 columns

  Interpolate Y2 from Table 11-1 (based on exact Fa/C0) →
  Get X2 = 0.56 →

  new Fe = X2 V Fr + Y2 Fa →
  new C10 = (new Fe / old Fe) × old C10 →

  Select bearing with C10_table ≥ new C10 →

  Same bearing as previous iteration?
    If yes: CONVERGED! Done!
    If no: old_Fe = new_Fe, old_C10 = new_C10, repeat LOOP
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized (40+ variables)
- [x] Source references noted for all table/graph variables (5 tables)
- [x] All equations extracted and converted to LaTeX (15+ equations)
- [x] Procedure steps documented (2 main procedures: 9 steps + 14 iterative steps)
- [x] Piecewise equations explained (a value, V factor, Xi/Yi selection, Weibull parameters)
- [x] Validation rules documented (convergence, C10 selection, positive values)
- [x] Future enhancements noted (interactive tables, iteration automation, visualization)
- [x] Tab structure decision justified (3 tabs for complex procedures + equations + variables)
- [x] Specification reviewed against PDF (pages 1-6 covered, continues beyond)
- [x] Consistent naming and formatting throughout

**Note:** Tapered Roller Bearings section (4.3) is incomplete as PDF continues beyond page 6. This specification covers the visible portion.

---

## 7. Implementation Ready

**Specification Status:** [x] Complete (for visible pages)  [ ] Ready for Implementation

**Specification Author:** Claude Code
**Date Created:** 2025-01-09
**Last Updated:** 2025-01-09

**Implementation Notes for HTML Calculator:**

**Complexity Assessment:**
- This is a **Complex** calculator
- Two distinct procedures with different complexity levels
- Iterative convergence procedure for combined loading
- Multiple table lookups with interpolation
- Manufacturer-specific parameters

**Major Features Needed:**

1. **Mode Selection:**
   - Radio buttons or tabs for "Radial Load Only" vs. "Radial and Thrust Loads"
   - Conditional visibility based on mode

2. **Iteration Engine (for Combined Loading):**
   - Automatic iteration loop (Steps 6-14)
   - Display iteration progress (iteration number, Fe values, bearing selection)
   - Convergence detection (same bearing twice)
   - Maximum iteration limit (prevent infinite loops)

3. **Interpolation Calculator:**
   - Linear interpolation for Y2 from Table 11-1
   - Automatic based on calculated Fa/C0
   - Show interpolation calculation details

4. **Table Implementations:**
   - **Table 11-4** (Bearing-Life Recommendations): Simple dropdown, 8 rows
   - **Weibull Parameters**: Auto-fill based on manufacturer selection
   - **Table 11-5** (Load-Application Factors): Simple dropdown, 6 rows
   - **Table 11-2** (Ball Bearings): Large table, multiple columns for Deep Groove vs. Angular Contact
   - **Table 11-3** (Cylindrical Roller): Large table, 02-Series and 03-Series
   - **Table 11-1** (Equivalent Radial Load Factors): Complex table with Fa/C0, e, X1, Y1, X2, Y2

**Recommended Calculator Structure:**

1. **Procedure Tab:**
   - Mode selector at top
   - **Radial Only mode:** Steps 1-9, straightforward workflow
   - **Combined Loading mode:** Steps 1-14, with iteration display
     - Show iteration table: Iteration #, Fe, C10, Selected Bearing, Converged?
     - "Run Next Iteration" button or auto-run
   - Real-time validation indicators

2. **General Equations Tab:**
   - Grouped by purpose (Speed, Loading, Reliability, Life, Rating)
   - All equations editable
   - Show which procedure they apply to

3. **Variables Tab:**
   - Organized by: Input / Table Lookup / Calculated / Iteration Tracking
   - Highlight mode-specific variables
   - Show defaults and initial guesses

**Special Implementation Considerations:**

**Iteration Display:**
```
Iteration 1: Fe = 1250 lbf → C10 = 5.5 kN → Bearing: Bore 30mm → Not Converged
Iteration 2: Fe = 1310 lbf → C10 = 5.8 kN → Bearing: Bore 35mm → Not Converged
Iteration 3: Fe = 1305 lbf → C10 = 5.77 kN → Bearing: Bore 35mm → CONVERGED!
```

**Interpolation Helper:**
- Show calculation steps for Y2 interpolation
- Display: "Finding Y2 for Fa/C0 = 0.065"
- Show: "Bracketing values: 0.056 (Y2=1.71) and 0.070 (Y2=1.63)"
- Show: "Slope m = -0.571, Intercept b = 2.04, Y2 = 1.66"

**Unit Consistency:**
- Tables use metric (mm, kN)
- Users may input imperial (in, lbf)
- Need clear unit selection and conversion

**Table 11-1 Edge Cases:**
- If Fa/C0 < 0.014: Use row 1 (Fa/C0 = 0.014*)
- If Fa/C0 > 0.56: Use row 12 (Fa/C0 = 0.56)
- Interpolation only between valid rows

**Testing Priorities:**
- Verify Weibull C10 calculation for both manufacturers
- Test iteration convergence (should converge in 2-5 iterations typically)
- Validate Y2 interpolation accuracy
- Test mode switching (radial only ↔ combined)
- Verify bearing selection from tables

**Incomplete Section:**
- Note that Tapered Roller Bearings (section 4.3) continues beyond visible pages
- May need to update specification when complete PDF is available
- Consider placeholder or "Coming Soon" message

---

## Template Version

Template Version: 1.0
Specification Version: 1.0 (Partial - covers pages 1-6)
