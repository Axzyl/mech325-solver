# Fizz Flat Belt Design Calculator - Specification

## Overview
Complete flat belt drive design guide following the comprehensive fizz methodology. Organizes formulas from Section 1.3 into a structured 10-step design process.

## Design Guidelines
- **All fields editable**: No color-coding, all inputs can be modified by user
- **Bidirectional solving**: Edit any variable, dependent values recalculate automatically
- **Session persistence**: Save/Load/Reset functionality via localStorage
- **PDF reference**: Mech_325_Bible_Fizz-flat_belts.pdf displayed on left side

## Source Document
- **PDF**: `documents/belts/Mech_325_Bible_Fizz-flat_belts.pdf`
- **Section**: §1.3 Flat Belts (pages 5-8)
- **Reference Tables**:
  - Table 17-2: Belt material properties
  - Table 17-4: Pulley correction factor Cp
  - Graph: Velocity correction factor Cv vs belt speed

## Design Procedure (10 Steps)

### Step 1: Design Requirements
**Purpose**: Define system inputs and operating conditions

**Variables**:
- `Hnom` = Nominal power to transmit (hp) [User Input]
- `Ks` = Service factor (dimensionless) [User Input or Table]
- `nd` = Design safety factor (dimensionless) [User Input, typical 1.0-1.2]
- `n1` = Driver pulley speed (rpm) [User Input]
- `n2` = Driven pulley speed (rpm) [User Input]
- `d1` = Driver pulley diameter (in) [User Input]
- `d2` = Driven pulley diameter (in) [User Input]
- `C` = Center distance (in) [User Input]

**Equations**:
```
Hd = Hnom * Ks * nd
Ha = Hnom * Ks * nd
```

**Notes**: Hd and Ha are equivalent design power formulas

---

### Step 2: Belt Material Selection
**Purpose**: Choose belt material and determine properties

**Variables**:
- `Material` = Belt material type [Dropdown: Leather, Polyamide F-0 through A-5, Urethane]
- `Specification` = Material specification [Dropdown: depends on material]
- `t` = Belt thickness (in) [Table 17-2]
- `γ` = Specific weight (lb/in³) [Table 17-2]
- `Fa` = Allowable tension per unit width at 600 ft/min (lbf/in) [Table 17-2]
- `f` = Maximum coefficient of friction (dimensionless) [Table 17-2]
- `dmin` = Minimum pulley diameter (in) [Table 17-2]

**Equations**: None (table lookups)

**Tables Required**:
- Table 17-2: Properties of flat and round-belt materials

**Validation**:
- Check d1 ≥ dmin and d2 ≥ dmin

---

### Step 3: Geometry and Velocity
**Purpose**: Calculate belt speed and verify velocity ratio

**Variables**:
- `V` = Belt speed (ft/min) [Equation]
- `VR` = Velocity ratio (dimensionless) [Equation]

**Equations**:
```
V = π * d1 * n1 / 12
VR = d2 / d1 = n1 / n2
```

**Notes**:
- Belt speed V is based on driver pulley
- VR can be calculated from either diameters or speeds

---

### Step 4: Wrap Angle Calculation
**Purpose**: Determine contact angle on smaller pulley

**Variables**:
- `φ_helper` = Helper angle for geometry (degrees) [Equation]
- `θ1` = Wrap angle on driver pulley (degrees) [Equation]
- `θ2` = Wrap angle on driven pulley (degrees) [Equation]
- `θ` = Smaller wrap angle for design (degrees) [Equation]
- `φ` = Wrap angle in radians (rad) [Equation]

**Equations**:
```
φ_helper = arcsin((d2 - d1) / (2 * C))  [degrees]
θ1 = 180° - 2 * φ_helper
θ2 = 180° + 2 * φ_helper
θ = min(θ1, θ2)
φ = θ * π / 180  [convert to radians]
```

**Notes**: Use smaller wrap angle (typically on smaller pulley) for conservative design

---

### Step 5: Belt Weight Calculation
**Purpose**: Determine belt weight per unit length

**Variables**:
- `b` = Belt width (in) [User Input - initial guess, or solved]
- `w` = Belt weight per foot (lb/ft) [Equation]

**Equations**:
```
w = 12 * γ * b * t
```

**Notes**:
- Belt width `b` may be initially guessed and later verified
- Factor of 12 converts in³/ft to proper units

---

### Step 6: Centrifugal Tension
**Purpose**: Calculate tension due to belt rotation

**Variables**:
- `Fc` = Centrifugal tension (lbf) [Equation]

**Equations**:
```
Fc = (w / 32.17) * (V / 60)²
```

**Notes**:
- 32.17 ft/s² is gravitational constant
- V/60 converts ft/min to ft/s

---

### Step 7: Correction Factors
**Purpose**: Apply pulley size and velocity corrections to allowable tension

**Variables**:
- `CP` = Pulley correction factor (dimensionless) [Table 17-4 or User]
- `CV` = Velocity correction factor (dimensionless) [Graph or User]
- `F1_a` = Largest allowable tension (lbf) [Equation]

**Equations**:
```
F1_a = b * Fa * CP * CV
```

**Tables/Graphs Required**:
- Table 17-4: Pulley correction factor vs small pulley diameter and material
- Graph (p. 8): Velocity correction factor vs belt speed (for leather belts)

**Notes**:
- CV = 1.0 for polyamide and urethane belts
- CP depends on small pulley diameter and belt material

---

### Step 8: Tension Analysis
**Purpose**: Calculate operating tensions and initial tension

**Variables**:
- `T` = Transmitted torque (lbf·in) [Equation]
- `F1` = Taut-side tension (lbf) [Equation or User]
- `F2` = Slack-side tension (lbf) [Equation]
- `Fi` = Initial tension (lbf) [Equation]
- `ΔF` = Tension difference (lbf) [Equation]

**Equations**:
```
T = 63025 * Hd / n1
ΔF = F1 - F2 = 2*T / d1
F1 - Fc / F2 - Fc = e^(f*φ)
F1 = Fc + (2*Fi*e^(f*φ)) / (e^(f*φ) + 1)
F2 = Fc + (2*Fi) / (e^(f*φ) + 1)
Fi = (F1 + F2)/2 - Fc = (T/d1) * (e^(f*φ) + 1) / (e^(f*φ) - 1)
```

**Alternative approach**:
```
F1 - F2 = 2*T/d1
F1/F2 = e^(f*φ)  [after accounting for Fc]
```

**Notes**:
- These equations are coupled - may need iterative solution
- Can specify F1 at operation limit: F1 = F1_a

---

### Step 9: Friction Verification
**Purpose**: Verify required friction coefficient is achievable

**Variables**:
- `f_prime` = Required coefficient of friction (dimensionless) [Equation]
- `friction_check` = Pass/fail status [Validation]

**Equations**:
```
f' = (1/φ) * ln((F1_a - Fc) / (F2 - Fc))
```

**Validation**:
```
Require: f' < f
```

**Notes**:
- f' is the minimum friction needed for the design
- Must be less than maximum available friction f from material properties

---

### Step 10: Belt Sag and Final Verification
**Purpose**: Calculate belt dip and verify power transmission

**Variables**:
- `dip` = Belt sag at midspan (in) [Equation]
- `H_transmitted` = Actual transmitted power (hp) [Equation]
- `nfs` = Factor of safety (dimensionless) [Equation]

**Equations**:
```
dip = (C² * w) / (96 * Fi)
H_transmitted = (F1 - F2) * V / 33000
nfs = Ha / (Hnom * Ks)
```

**Validation**:
- Verify H_transmitted ≥ Hd
- Verify F1 ≤ F1_a
- Verify f' < f
- Check reasonable belt dip (typically < 2-3% of C)

---

## General Equations Tab

All equations organized by category:

### Power and Torque
```
Hd = Hnom * Ks * nd
Ha = Hnom * Ks * nd
H = (F1 - F2) * V / 33000
T = 63025 * Hd / n1
nfs = Ha / (Hnom * Ks)
```

### Kinematics
```
V = π * d1 * n1 / 12
VR = d2 / d1 = n1 / n2
```

### Geometry
```
φ_helper = arcsin((d2 - d1) / (2*C))
θ1 = 180 - 2*φ_helper
θ2 = 180 + 2*φ_helper
θ = min(θ1, θ2)
φ = θ * π/180
```

### Belt Weight
```
w = 12 * γ * b * t
```

### Tensions
```
Fc = (w / 32.17) * (V/60)²
F1 - F2 = 2*T/d1
(F1 - Fc)/(F2 - Fc) = e^(f*φ)
F1 = Fc + (2*Fi*e^(f*φ)) / (e^(f*φ) + 1)
F2 = Fc + (2*Fi) / (e^(f*φ) + 1)
Fi = (F1 + F2)/2 - Fc
F1_a = b * Fa * CP * CV
```

### Friction
```
f' = (1/φ) * ln((F1_a - Fc)/(F2 - Fc))
Require: f' < f
```

### Belt Sag
```
dip = (C² * w) / (96 * Fi)
```

---

## Variables Summary

Total variables: ~30

### User Inputs (9)
- Hnom, Ks, nd, n1, n2, d1, d2, C, b (initial guess)

### Table Lookups (6)
- Material, Specification, t, γ, Fa, f, dmin (from Table 17-2)
- CP (from Table 17-4)
- CV (from graph or = 1.0 for some materials)

### Calculated (15+)
- Hd, Ha, V, VR, φ_helper, θ1, θ2, θ, φ, w, Fc, F1_a, T, F1, F2, Fi, ΔF, f', dip, H_transmitted, nfs

---

## Implementation Notes

### Variable Syncing
- All instances of the same variable must sync bidirectionally
- Use `varInstanceMap` to track all input fields for each variable
- `syncAllInstances(varName, newValue)` updates all fields

### User Override Protection
- Track user-edited values in `userValues` Set
- Don't auto-calculate if user has manually entered a value
- Clear from `userValues` when Save is clicked

### Iterative Solving
Steps 7-8 may require iteration:
1. Guess b (belt width)
2. Calculate w, Fc, F1_a
3. Set F1 = F1_a (at operation limit)
4. Solve for F2 using tension equations
5. Verify ΔF = 2T/d1
6. Adjust b if needed

### Table Integration
- Table 17-2: Material properties (requires manual lookup)
- Table 17-4: CP values (requires manual lookup)
- Velocity graph: CV values (manual lookup or assume 1.0)

### Validation Indicators
- Show green checkmark when f' < f
- Show red warning when F1 > F1_a
- Show warning if dip > 0.03*C (3% sag limit)

---

## UI Layout

### Three Tabs
1. **Procedure**: 10 steps with equations in order
2. **General Equations**: All formulas organized by category
3. **Variables**: Complete variable list with current values

### Session Controls
- Save Button: Saves all values to localStorage
- Load Button: Restores from localStorage
- Reset Button: Clears all values and localStorage

### PDF Display
- Left side: Mech_325_Bible_Fizz-flat_belts.pdf
- Right side: Calculator interface
- Resizable split view

---

## Testing Scenarios

### Test 1: Basic Design
- Hnom = 10 hp
- Ks = 1.2
- nd = 1.1
- n1 = 1750 rpm
- d1 = 4 in
- n2 = 875 rpm
- d2 = 8 in
- C = 24 in
- Material: Polyamide A-2

Expected: Calculate belt width, verify tensions, check friction

### Test 2: High Speed
- V > 4000 ft/min
- Verify CV factor reduction (for leather)
- Check centrifugal tension significance

### Test 3: Small Pulley
- d1 = 2 in
- Verify CP correction factor
- Check against dmin requirement

---

## LaTeX Formatting

Use MathJax 3 for all equations:

```latex
H_d = H_{nom} \cdot K_s \cdot n_d

V = \frac{\pi d_1 n_1}{12}

F_c = \frac{w}{32.17} \left(\frac{V}{60}\right)^2

\frac{F_1 - F_c}{F_2 - F_c} = e^{f\phi}

f' = \frac{1}{\phi} \ln\left(\frac{(F_1)_a - F_c}{F_2 - F_c}\right)

\text{dip} = \frac{C^2 w}{96 F_i}
```

---

## File Paths
- **Specification**: `specifications/belts/fizz_flat_belts_spec.md`
- **Calculator**: `calculators/belts/fizz_flat_belts.html`
- **PDF**: `../../documents/belts/Mech_325_Bible_Fizz-flat_belts.pdf`
