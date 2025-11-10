# CALCULATOR SPECIFICATION: Power Screws and Ball Screws

**Calculator Name**: fizz_power
**Source Document**: `documents/screws/fizz_power.pdf`
**Pages**: 5 (pages 123-127 in original document)
**Date Created**: 2025-11-09
**Specification Version**: 1.0

---

## 1. CALCULATOR METADATA

### 1.1 Purpose
This calculator provides design procedures for power screws (7-step procedure) and ball screws (2-step procedure, partial). For power screws, it determines screw selection based on tensile and shear stress, calculates nut length, lead angle, raising/lowering torques, efficiency, and maximum speed. For ball screws, it calculates travel life for selection purposes.

### 1.2 Scope
- **Inputs**: Load force, tensile/shear stress limits, friction coefficients, thread type, input power, usage parameters
- **Outputs**: Screw designation, nut length, torques, efficiency, speeds, travel life
- **Target Users**: Mechanical engineers designing power transmission systems using screws

### 1.3 Limitations
- Ball screw section incomplete (cuts off after step 2)
- Requires table lookups for screw specifications
- Thread angles fixed by screw type (Square, Acme, Trapezoidal)
- Nut length must be rounded to nearest 1/4 inch

### 1.4 Assumptions
- Roller bearing present: collar friction = 0 (unless stated otherwise)
- Thread angles: Square φ=0°, Acme φ=14.5°, Trapezoidal φ=15°
- Rounding: nut length to nearest 1/4"

---

## 2. VARIABLES

### 2.1 Variable Definitions Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Input Variables - Load and Stress** |
| F | Applied load | Load applied to screw | lbf | User Input | Step 1, 3, 5, 6, 7 | Yes | - |
| σ_d | Max tensile stress | Maximum allowable tensile stress | psi | User Input | Step 1 | Yes | - |
| τ_d | Max shear stress | Maximum allowable shear stress | psi | Step 3a | User Input | Yes | - |
| **Calculated - Stress Areas** |
| A_t | Tensile stress area | Required tensile stress area | in² | Calculated | Step 1: A_t = F/σ_d | Yes | - |
| A_s_calc | Calculated shear area | Required shear area | in² | Calculated | Step 3a: A_s = F/τ_d | Yes | - |
| **Table Lookup Variables - Screw Selection** |
| screw_type | Screw type | Type of power screw | - | User Input | Step 2 | Yes | Acme or Trapezoidal |
| screw_designation | Screw designation | Selected screw size | - | Table Lookup | Step 2 | Yes | Yellow input, e.g., "1/4-16" |
| D | Nominal major diameter | Major diameter of screw | in (or mm) | Table Lookup | Step 2: From table | Yes | Yellow input |
| n_threads | Threads per inch | Thread count | tpi | Table Lookup | Step 2: From table | Yes | Yellow input |
| p | Pitch | Thread pitch | in (or mm) | Table Lookup | Step 2, 4, 6, 7 | Yes | Yellow input, p=1/n |
| D_r | Minimum minor diameter | Minimum minor diameter | in (or mm) | Table Lookup | Step 2: From table | Yes | Yellow input |
| D_p | Pitch diameter | Pitch diameter | in (or mm) | Table Lookup | Step 2, 4, 5 | Yes | Yellow input |
| A_t_rated | Rated tensile stress area | Rated tensile area from table | in² (or mm²) | Table Lookup | Step 2: From table | Yes | Yellow input |
| A_s_rated | Rated shear stress area | Rated shear area per inch | in²/in (or mm²/mm) | Table Lookup | Step 2: From table | Yes | Yellow input |
| **Calculated - Nut Length** |
| h | Nut length | Required length of nut/yoke | in | Calculated | Step 3b: h = A_s_calc/A_s_rated | Yes | Round up to nearest 1/4" |
| h_rounded | Rounded nut length | Nut length rounded to 1/4" | in | Calculated | Step 3b: Round up | Yes | - |
| **Calculated - Lead Angle** |
| λ | Lead angle | Lead angle of thread | deg | Calculated | Step 4: tan⁻¹(p/πD_p) | Yes | - |
| **Input Variables - Friction** |
| f | Friction coefficient | Thread friction coefficient | - | User Input | Step 5 | Yes | - |
| f_c | Collar friction | Collar friction coefficient | - | User Input | Step 5c (optional) | Yes | 0 if roller bearing |
| R_c | Collar friction radius | Mean radius of collar | in | User Input | Step 5c (optional) | Yes | Only if collar friction |
| bearing_type | Bearing type | Roller bearing or plain | - | User Input | Step 5c | Yes | Affects collar friction |
| **Thread Angle** |
| φ | Thread angle | Half-angle of thread profile | deg | User Input / Fixed | Step 5 | Yes | Square=0°, Acme=14.5°, Trap=15° |
| **Calculated - Torques** |
| T_u | Raising torque | Torque to raise load | lbf·in | Calculated | Step 5a: Formula | Yes | With or without collar |
| T_d | Lowering torque | Torque to lower load | lbf·in | Calculated | Step 5b: Formula | Yes | With or without collar |
| **Calculated - Efficiency** |
| e | Efficiency | Mechanical efficiency of screw | - | Calculated | Step 6: e = Fp/(2πT_u) | Yes | Dimensionless ratio |
| e_percent | Efficiency percent | Efficiency as percentage | % | Calculated | Step 6: e × 100 | Yes | - |
| **Input/Calculated - Speed and Power** |
| P | Input power | Power supplied to screw | hp | User Input | Step 7a | Yes | - |
| n | Rotational speed | Screw rotation speed | rpm | Calculated | Step 7a: n = 63000P/T | Yes | - |
| V | Linear speed | Linear velocity of load | in/s | Calculated | Step 7b: V = np/60 | Yes | - |
| **Ball Screw Variables** |
| distance_per_cycle | Distance per cycle | Travel distance per cycle | in | User Input | Section 5.2, Step 1 | Yes | - |
| cycles_per_hour | Cycles per hour | Operating cycles per hour | cycles/hr | User Input | Section 5.2, Step 1 | Yes | - |
| hours_per_day | Hours per day | Operating hours per day | hr/day | User Input | Section 5.2, Step 1 | Yes | - |
| days_per_year | Days per year | Operating days per year | day/yr | User Input | Section 5.2, Step 1 | Yes | Typically 365 |
| years_lifetime | Lifetime in years | Expected lifetime | years | User Input | Section 5.2, Step 1 | Yes | - |
| travel_life | Travel life | Total travel distance over life | in | Calculated | Step 1: Formula | Yes | - |
| ball_screw_load | Ball screw load | Load on ball screw | lbf | User Input | Section 5.2, Step 2 | Yes | - |
| ball_screw_designation | Ball screw model | Selected ball screw | - | Table Lookup | Section 5.2, Step 2 | Yes | Incomplete in PDF |

### 2.2 Variable Categories by Source Type

**User Inputs (White)**: F, σ_d, τ_d, screw_type, f, f_c (optional), R_c (optional), bearing_type, φ (or thread type), P, distance_per_cycle, cycles_per_hour, hours_per_day, days_per_year, years_lifetime, ball_screw_load

**Table/Graph Lookups (Yellow)**: screw_designation, D, n_threads, p, D_r, D_p, A_t_rated, A_s_rated, ball_screw_designation

**Calculated (Green)**: A_t, A_s_calc, h, h_rounded, λ, T_u, T_d, e, e_percent, n, V, travel_life

---

## 3. EQUATIONS

### 3.1 Core Equations

**Step 1: Required Tensile Stress Area**
```latex
A_t = \frac{F}{\sigma_d}
```
Where: F in lbf, σ_d in psi, A_t in in²

**Step 3a: Required Shear Area**
```latex
A_s = \frac{F}{\tau_d}
```
Where: F in lbf, τ_d in psi, A_s in in²

**Step 3b: Required Nut Length**
```latex
h = A_{sc} \frac{1 \text{ in}}{A_{sr}}
```
Where:
- A_sc = calculated shear area (from step 3a)
- A_sr = rated shear area per inch from table
- Round h up to nearest 1/4"

**Step 4: Lead Angle**
```latex
\lambda = \tan^{-1}\left(\frac{p}{\pi D_p}\right)
```
Where: p in inches, D_p in inches, λ in degrees

**Step 5a: Raising Torque (without collar friction)**
```latex
T_u = \frac{F D_p}{2} \left[\frac{\cos \phi \tan \lambda + f}{\cos \phi - f \tan \lambda}\right]
```
Where: F in lbf, D_p in inches, T_u in lbf·in

**Step 5b: Lowering Torque (without collar friction)**
```latex
T_d = \frac{F D_p}{2} \left[\frac{f - \cos \phi \tan \lambda}{\cos \phi + f \tan \lambda}\right]
```
Where: F in lbf, D_p in inches, T_d in lbf·in

**Step 5c: Raising Torque (with collar friction)**
```latex
T_u = \frac{F D_p}{2} \left[\frac{\cos \phi \tan \lambda + f}{\cos \phi - f \tan \lambda}\right] + f_c F R_c
```

**Step 5c: Lowering Torque (with collar friction)**
```latex
T_d = \frac{F D_p}{2} \left[\frac{f - \cos \phi \tan \lambda}{\cos \phi + f \tan \lambda}\right] + f_c F R_c
```

**Step 6: Efficiency**
```latex
e = \frac{F p}{2 \pi T_u}
```
Where: F in lbf, p in inches, T_u in lbf·in, e is dimensionless

**Step 7a: Rotational Speed**
```latex
n = \frac{63000 P}{T}
```
Where: P in hp, T in lbf·in, n in rpm

**Step 7b: Linear Speed**
```latex
V = \left(\frac{n \text{ rev}}{\text{min}}\right) \left(\frac{p \text{ in}}{\text{rev}}\right) \left(\frac{\text{min}}{60 \text{ sec}}\right)
```
Simplifies to:
```latex
V = \frac{n p}{60}
```
Where: n in rpm, p in inches, V in in/s

**Ball Screw Step 1: Travel Life**
```latex
\text{Travel Life} = \left(\frac{\text{distance (in)}}{\text{cycle}}\right) \left(\frac{\# \text{ of cycles}}{\text{hour}}\right) \left(\frac{24 \text{ hours}}{\text{day}}\right) \left(\frac{365 \text{ days}}{\text{year}}\right) (\# \text{ of years})
```

### 3.2 Piecewise/Conditional Equations

**Thread Angle φ by Screw Type**:

| Screw Type | Thread Angle φ |
|------------|----------------|
| Square threads | 0° |
| Acme threads | 14.5° |
| Trapezoidal threads | 15° |

**Collar Friction Handling**:

| Bearing Type | Collar Friction Term |
|--------------|---------------------|
| Roller bearing | f_c F R_c = 0 (omit term) |
| Plain bearing | f_c F R_c (include term) |

**Nut Length Rounding**:
- Round h up to nearest 1/4 inch
- Examples:
  - h = 1.10 → h_rounded = 1.25
  - h = 1.58 → h_rounded = 1.75
  - h = 2.01 → h_rounded = 2.25

---

## 4. PROCEDURE STEPS

### 4.1 Power Screw Design Procedure (7 Steps)

| Step | Action Type | Description | Inputs Required | Outputs Generated | Notes |
|------|-------------|-------------|-----------------|-------------------|-------|
| 1 | Calculate | Calculate required tensile stress area | F, σ_d | A_t | A_t = F/σ_d |
| 2 | Table Lookup | Select screw from table with A_t ≥ calculated | A_t, screw_type | screw_designation, D, n_threads, p, D_r, D_p, A_t_rated, A_s_rated | Yellow input, use Table 17-1 (Acme) or 17-1M (Metric) |
| 3a | Calculate | Calculate required shear area | F, τ_d | A_s_calc | A_s = F/τ_d |
| 3b | Calculate | Calculate required nut length and round up | A_s_calc, A_s_rated | h, h_rounded | h = A_s_calc/A_s_rated; round to 1/4" |
| 4 | Calculate | Compute lead angle | p, D_p | λ | λ = tan⁻¹(p/πD_p) |
| 5a | Calculate | Determine raising torque | F, D_p, φ, f, λ | T_u | Without collar: main formula |
| 5b | Calculate | Determine lowering torque | F, D_p, φ, f, λ | T_d | Without collar: main formula |
| 5c | Calculate | Add collar friction if applicable | f_c, R_c, bearing_type | T_u, T_d | Only if not roller bearing |
| 6 | Calculate | Compute efficiency | F, p, T_u | e, e_percent | e = Fp/(2πT_u) |
| 7a | Calculate | Find rotational speed from input power | P, T_u | n | n = 63000P/T; use T_u for raising |
| 7b | Calculate | Convert to linear speed | n, p | V | V = np/60 in/s |

### 4.2 Ball Screw Design Procedure (Partial - 2 Steps)

| Step | Action Type | Description | Inputs Required | Outputs Generated | Notes |
|------|-------------|-------------|-----------------|-------------------|-------|
| 1 | Calculate | Compute travel life in inches | distance_per_cycle, cycles_per_hour, hours_per_day, days_per_year, years_lifetime | travel_life | Full lifetime calculation |
| 2 | Table Lookup | Select ball screw based on load and travel life | ball_screw_load, travel_life | ball_screw_designation | INCOMPLETE - table not in PDF |

---

## 5. ADDITIONAL NOTES

### 5.1 Table References

**Table 17-1: Preferred Acme Screw Threads** (page 2):
- 30 rows: 1/4" to 5"
- Columns:
  - Nominal major diameter D (in)
  - Threads per inch n (tpi)
  - Pitch p = 1/n (in)
  - Minimum minor diameter D_r (in)
  - Minimum pitch diameter D_p (in)
  - Tensile stress area A_t (in²)
  - Shear stress area A_s (in²/in)
- Used in Step 2

**Table 17-1M: Examples of Power Screws with Metric Trapezoidal Screw Thread** (page 3):
- ISO thread system - External threads
- 24 rows: 8mm to 125mm
- Columns:
  - Major diameter D (mm)
  - Pitch ρ (mm)
  - Pitch diameter D_p (mm)
  - Minor diameter D_r (mm)
  - Tensile stress area (mm²)
- Used in Step 2 for metric screws

### 5.2 Diagrams and Visual Aids

**Power Screw Anatomy Diagram** (page 1):
- Top view showing d_m (mean diameter)
- Side view showing:
  - F (applied load)
  - ψ (helix angle, same as λ lead angle)
  - λ (lead angle)
  - p (pitch)
  - Nut position
  - F/2 reactions at nut

### 5.3 Special Considerations

**Screw Selection Criterion**:
- Choose screw where A_t_rated ≥ A_t_calculated
- Take note of A_s_rated for nut length calculation

**Thread Type Selection**:
- Square threads (φ=0°): Most efficient, but harder to manufacture
- Acme threads (φ=14.5°): Common, good balance of efficiency and manufacturability
- Trapezoidal threads (φ=15°): Metric standard, similar to Acme

**Roller Bearing Assumption**:
- If problem states "roller bearing" or doesn't mention collar friction, assume f_c = 0
- This eliminates the f_c F R_c term from torque equations

**Self-Locking Condition**:
- If T_d < 0, the screw is self-locking (won't back-drive under load)
- If T_d > 0, the screw will lower under load without applied torque

**Efficiency Interpretation**:
- e close to 1 (or 100%): High efficiency, less friction loss
- Typical power screws: e = 0.25 to 0.85 depending on thread type and friction

**Speed Calculation Context**:
- Step 7 calculates maximum speed for given power input
- Useful for determining if design meets speed requirements
- Linear speed V in in/s, may need conversion to other units

**Ball Screw Section**:
- PDF is incomplete - only shows travel life calculation and mentions selection
- Ball screw selection table/procedure not included
- Would typically involve comparing calculated travel life to rated life curves

**Units Consistency**:
- Acme table (17-1): U.S. customary (inches, in²)
- Metric table (17-1M): SI units (mm, mm²)
- Ensure consistent unit system throughout calculation

---

## 6. IMPLEMENTATION READY CHECKLIST

### 6.1 Data Requirements
- [x] All equations documented in LaTeX format
- [x] All variables defined with units
- [x] Input/output relationships mapped
- [x] Table lookup requirements identified (2 tables)
- [x] Piecewise conditions documented
- [x] Incomplete sections noted (ball screw)

### 6.2 UI/UX Requirements
- [ ] Three-tier input system (white/yellow/green) implemented
- [ ] MathJax rendering for all equations
- [ ] PDF viewer with page navigation (5 pages)
- [ ] Session save/load/reset functionality
- [ ] Screw type selector (Acme/Trapezoidal/Square)
- [ ] Unit system selector (U.S./Metric)
- [ ] Bearing type selector (Roller/Plain)
- [ ] Collar friction optional inputs (conditional visibility)
- [ ] Nut length rounding display (show before/after)
- [ ] Efficiency display as both decimal and percentage

### 6.3 Calculation Logic
- [ ] Table search for screw selection (A_t_rated ≥ A_t_calc)
- [ ] Thread angle lookup based on screw type
- [ ] Conditional collar friction term (roller bearing → 0)
- [ ] Nut length rounding to nearest 1/4"
- [ ] Self-locking detection (T_d < 0)
- [ ] Unit conversions between U.S. and metric
- [ ] Angle conversions (deg ↔ rad for trig functions)

### 6.4 Validation Requirements
- [ ] Constraint checks:
  - F > 0
  - σ_d > 0, τ_d > 0
  - Friction coefficients 0 ≤ f ≤ 1, 0 ≤ f_c ≤ 1
  - Power P > 0
  - All dimensions > 0
- [ ] Table availability checks (screw exists with sufficient A_t)
- [ ] Warning if T_d < 0 (self-locking)
- [ ] Warning if efficiency very low (e < 0.25)

### 6.5 Output Requirements
- [ ] Selected screw designation
- [ ] All screw dimensions (D, p, D_p, D_r)
- [ ] Nut length (calculated and rounded)
- [ ] Lead angle λ
- [ ] Raising torque T_u
- [ ] Lowering torque T_d
- [ ] Efficiency e (decimal and %)
- [ ] Rotational speed n (if power given)
- [ ] Linear speed V (if power given)
- [ ] Travel life (for ball screws)
- [ ] Intermediate results (A_t, A_s, etc.)

### 6.6 Known Challenges
- **Two table options**: Must select between Acme (Table 17-1) and Metric (Table 17-1M)
- **Conditional collar friction**: UI must show/hide f_c and R_c inputs based on bearing type
- **Thread angle**: Could be user-selectable or auto-set based on screw type
- **Rounding logic**: 1/4" rounding requires special implementation
- **Self-locking detection**: Need to handle T_d < 0 case gracefully
- **Ball screw incomplete**: Only partial procedure available; may need to note limitation or omit
- **Unit mixing**: Tables use different units (in vs mm); need consistent handling

---

## 7. REVIEW CHECKLIST

- [x] All pages of PDF reviewed (5 pages)
- [x] All equations extracted and formatted
- [x] All variables catalogued with units
- [x] All tables identified and referenced
- [x] Procedure steps sequenced correctly
- [x] Piecewise conditions documented
- [x] Special cases noted (self-locking, roller bearing)
- [x] Implementation challenges identified
- [x] Incomplete sections documented (ball screw)
- [x] Cross-references validated

---

**Specification Status**: ✅ COMPLETE - Ready for HTML implementation (Power Screw section)
⚠️ INCOMPLETE - Ball Screw section partial (only travel life calc available)

**Estimated Implementation Complexity**: MEDIUM-HIGH
- 25+ variables (power screws)
- 15+ equations
- 7-step main procedure
- 2 table lookups (30+ rows each)
- Conditional logic (collar friction, thread angle, self-locking)
- Rounding requirements (1/4" increments)
- Optional ball screw section (partial data)

**Recommended Implementation Approach**:
1. Implement power screw section fully (Steps 1-7)
2. Start with U.S. units (Acme screws, Table 17-1)
3. Add metric support later (Table 17-1M)
4. Implement screw type selector for thread angle
5. Add bearing type selector for collar friction conditional
6. Implement nut length rounding clearly (show before/after)
7. Add warning indicators for self-locking (T_d < 0) and low efficiency
8. Optionally implement partial ball screw (travel life only)
9. Note limitation that ball screw selection table not available
10. Use clear visual separation between power screw and ball screw sections
