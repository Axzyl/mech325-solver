# Fizz V-Belt Design Calculator - Specification

## Overview
Complete V-belt drive design guide following the comprehensive fizz methodology. Organizes the V-belt selection process from Section 1.4 into a structured 15-step design procedure.

## Design Guidelines
- **All fields editable**: No color-coding, all inputs can be modified by user
- **Bidirectional solving**: Edit any variable, dependent values recalculate automatically
- **Session persistence**: Save/Load/Reset functionality via localStorage
- **PDF reference**: Mech_325_Bible_Fizz-v_belts.pdf displayed on left side

## Source Document
- **PDF**: `documents/belts/Mech_325_Bible_Fizz-v_belts.pdf`
- **Section**: §1.4 V-Belt Drives (pages 9-18)
- **Reference Tables/Graphs**:
  - Table 7-1: V-Belt Service Factors
  - Graph: Belt section selection (design power vs faster shaft RPM)
  - Graphs: Standard sheave sizes for 3V, 5V, 8V belts (rated power vs pitch diameter)
  - Graph: Power added for speed ratio > 1
  - Table 7-2: Standard Belt Lengths
  - Graph: Angle of wrap correction factor Cθ
  - Graph: Belt length correction factor CLp

## Design Procedure (15 Steps)

### Step 1: Design Power Calculation
**Purpose**: Determine design power with service factor

**Variables**:
- `Hin` = Input power (hp) [User Input]
- `SF` = Service factor (dimensionless) [Table 7-1]
- `Pdes` = Design power (hp) [Equation]

**Equations**:
```
Pdes = Hin * SF
```

**Tables Required**:
- Table 7-1: V-Belt Service Factors
  - Rows: Driven machine type (Smooth loading, Light shock, Moderate shock, Heavy shock, Machinery that can choke)
  - Columns: Driver type (AC/DC motor type) and hours of operation
  - Values range from 1.0 to 2.0

---

### Step 2: Belt Section Selection
**Purpose**: Select V-belt type (3V, 5V, or 8V) based on design power and RPM

**Variables**:
- `faster_shaft_RPM` = Speed of faster shaft (rpm) [User Input]
- `belt_section` = Belt type [Graph or Dropdown: 3V, 5V, 8V]

**Graph**: Belt section selection guide (page 11)
- X-axis: Design power (input power × service factor)
- Y-axis: Speed of faster shaft (rpm)
- Regions: 3V or 3VX, 5V or 5VX, 8V or 8VX

**Notes**:
- If at boundary, choose larger belt type (conservative)
- 3V for lower power/speed, 8V for higher power/speed

---

### Step 3: Nominal Speed Ratio
**Purpose**: Calculate speed ratio between shafts

**Variables**:
- `n1` = Faster shaft speed (rpm) [User Input]
- `n2` = Slower shaft speed (rpm) [User Input]
- `VR_nominal` = Nominal velocity ratio (dimensionless) [Equation]

**Equations**:
```
VR_nominal = n1 / n2    where n1 > n2
```

---

### Step 4: Driving Sheave Sizing
**Purpose**: Select driving sheave diameter for target belt speed of 4000 ft/min

**Variables**:
- `vb_target` = Target belt speed = 4000 (ft/min) [Constant]
- `D1_calculated` = Calculated driving sheave diameter (in) [Equation]

**Equations**:
```
vb = (D1 * n1) / (2 * 12 * (1/(2π)))
Solving for D1:
D1_calculated = (vb_target * 12 * 2) / (n1 * π)
```

Simplified:
```
D1_calculated = (vb_target * 24) / (n1 * π)
```

**Notes**:
- Standard belt speed should not exceed 5000 ft/min
- Hard maximum: 6500 ft/min
- 4000 ft/min is conservative target

---

### Step 5: Standard Sheave Selection
**Purpose**: Select standard sheave sizes from available options

**Sub-step 5a: Driver Sheave**

**Variables**:
- `D1_standard` = Standard driving sheave diameter (in) [Graph]

**Graphs Required** (pages 11-14):
- For 3V belts: Standard sizes from graph
- For 5V belts: Standard sizes from graph
- For 8V belts: Standard sizes from graph

**Selection**: Choose closest standard size to D1_calculated

**Sub-step 5b: Driven Sheave Calculation**

**Variables**:
- `D2_calculated` = Calculated driven sheave diameter (in) [Equation]

**Equations**:
```
D2_calculated = D1_standard * VR_nominal    where D2 > D1
```

**Sub-step 5c: Driven Sheave Standard**

**Variables**:
- `D2_standard` = Standard driven sheave diameter (in) [Graph]

**Selection**: Choose closest standard size to D2_calculated from same graphs as 5a

---

### Step 6: Actual Ratios and Speed
**Purpose**: Calculate actual values based on standard sheave sizes

**Variables**:
- `VR_actual` = Actual velocity ratio (dimensionless) [Equation]
- `vb_actual` = Actual belt speed (ft/min) [Equation]

**Equations**:
```
VR_actual = D2_standard / D1_standard

vb_actual = (D1_standard * n1) / (2 * 12 / (2π))
         = π * D1_standard * n1 / 12
```

---

### Step 7: Rated Power Per Belt
**Purpose**: Determine power capacity of single belt

**Sub-step 7a: Base Rated Power**

**Variables**:
- `Prated_base` = Base rated power per belt (hp) [Graph]

**Graphs Required** (pages 11-14):
- Same graphs as Step 5
- X-axis: Small sheave pitch diameter
- Y-axis: Rated power per belt (hp)
- Multiple curves for different RPM values

**Lookup**: Based on D1_standard (smaller sheave) and RPM

**Sub-step 7b: Speed Ratio Power Addition**

**Variables**:
- `Padd` = Power added for speed ratio (hp) [Graph]

**Graph Required** (page 15):
- X-axis: Speed ratio
- Y-axis: Power added to rated power (hp)
- Multiple curves for different RPM (870, 1160, 1750 rpm)

**Condition**: Only if VR_actual > 1.0

**Sub-step 7c: Total Rated Power**

**Variables**:
- `Prated_total` = Total rated power per belt (hp) [Equation]

**Equations**:
```
If VR_actual > 1.0:
    Prated_total = Prated_base + Padd
Else:
    Prated_total = Prated_base
```

---

### Step 8: Trial Center Distance
**Purpose**: Specify trial center distance within acceptable range

**Variables**:
- `CD_trial` = Trial center distance (in) [User Input]

**Constraints**:
```
D2_standard < CD_trial < 3 * (D2_standard + D1_standard)
```

**Validation**: Check CD_trial is within acceptable range

---

### Step 9: Required Belt Length
**Purpose**: Calculate required belt length based on geometry

**Variables**:
- `Lp_required` = Required belt length (in) [Equation]

**Equations**:
```
Lp_required = 2*CD_trial + 1.57*(D2_standard + D1_standard) + (D2_standard - D1_standard)² / (4*CD_trial)
```

---

### Step 10: Standard Belt Length Selection
**Purpose**: Select closest standard belt length

**Variables**:
- `Lp_standard` = Standard belt length (in) [Table 7-2]

**Table Required** (page 16):
- Table 7-2: Standard Belt Lengths for 3V, 5V, and 8V Belts
- Multiple columns for different belt types
- Lengths from 25 in to 500 in

**Selection**: Choose closest available standard length to Lp_required

---

### Step 11: Actual Center Distance
**Purpose**: Calculate actual center distance with standard belt length

**Variables**:
- `B` = Intermediate calculation variable [Equation]
- `CD_actual` = Actual center distance (in) [Equation]

**Equations**:
```
B = 4*Lp_standard - 6.28*(D2_standard + D1_standard)

CD_actual = (B + sqrt(B² - 32*(D2_standard - D1_standard)²)) / 16
```

---

### Step 12: Angle of Wrap
**Purpose**: Calculate contact angle on smaller sheave

**Variables**:
- `theta1` = Angle of wrap on small sheave (degrees) [Equation]
- `theta2` = Angle of wrap on large sheave (degrees) [Equation]

**Equations**:
```
theta1 = 180° - 2*arcsin((D2_standard - D1_standard) / (2*CD_actual))

theta2 = 180° + 2*arcsin((D2_standard - D1_standard) / (2*CD_actual))
```

**Notes**:
- theta1 is typically the critical value (smaller angle)
- theta2 rarely needed but included for completeness

---

### Step 13: Angle of Wrap Correction Factor
**Purpose**: Determine correction factor based on wrap angle

**Variables**:
- `Ctheta` = Angle of wrap correction factor (dimensionless) [Graph]

**Graph Required** (page 17):
- X-axis: Angle of wrap (degrees), range 80° to 180°
- Y-axis: Cθ correction factor, range 0.64 to 1.00
- Approximately linear relationship

**Lookup**: Based on theta1 (smaller wrap angle)

---

### Step 14: Belt Length Correction Factor
**Purpose**: Determine correction factor based on belt length

**Variables**:
- `CLp` = Belt length correction factor (dimensionless) [Graph]

**Graph Required** (page 17):
- X-axis: Belt length (in), range 0 to 450 in
- Y-axis: CLp correction factor, range 0.80 to 1.20
- Three curves: 3V, 5V, 8V
- Larger belts have smaller correction factors

**Lookup**: Based on Lp_standard and belt_section

---

### Step 15: Number of Belts Required
**Purpose**: Calculate how many belts needed to transmit power

**Sub-step 15a: Corrected Power Rating**

**Variables**:
- `Pcorrected` = Corrected power rating per belt (hp) [Equation]

**Equations**:
```
Pcorrected = Ctheta * CLp * Prated_total
```

**Sub-step 15b: Minimum Number of Belts**

**Variables**:
- `Nbelts_calc` = Calculated number of belts (dimensionless) [Equation]
- `Nbelts_required` = Required number of belts (integer) [Equation]

**Equations**:
```
Nbelts_calc = Pdes / Pcorrected

Nbelts_required = ceil(Nbelts_calc)
```

**Notes**:
- Always round up to next integer
- Cannot use fractional belts

---

## General Equations Tab

All equations organized by category:

### Design Power
```
Pdes = Hin * SF
```

### Kinematics
```
VR_nominal = n1 / n2  (where n1 > n2)
VR_actual = D2_standard / D1_standard
vb = π * D1 * n1 / 12
```

### Sheave Sizing
```
D1_calculated = (4000 * 24) / (n1 * π)
D2_calculated = D1_standard * VR_nominal
```

### Belt Length
```
Lp_required = 2*CD + 1.57*(D2 + D1) + (D2 - D1)² / (4*CD)
B = 4*Lp - 6.28*(D2 + D1)
CD_actual = (B + sqrt(B² - 32*(D2 - D1)²)) / 16
```

### Geometry
```
theta1 = 180° - 2*arcsin((D2 - D1)/(2*CD))
theta2 = 180° + 2*arcsin((D2 - D1)/(2*CD))
```

### Power Calculations
```
Prated_total = Prated_base + Padd  (if VR > 1)
Pcorrected = Ctheta * CLp * Prated_total
Nbelts_required = ceil(Pdes / Pcorrected)
```

### Validation
```
D2 < CD < 3*(D2 + D1)
vb ≤ 5000 ft/min (preferred)
vb ≤ 6500 ft/min (hard limit)
```

---

## Variables Summary

Total variables: ~30

### User Inputs (6)
- Hin, faster_shaft_RPM, n1, n2, CD_trial

### Table/Graph Lookups (15)
- SF (Table 7-1)
- belt_section (Graph)
- D1_standard, D2_standard (Graphs - sheave sizes)
- Prated_base (Graphs - sheave sizes)
- Padd (Graph - speed ratio)
- Lp_standard (Table 7-2)
- Ctheta (Graph)
- CLp (Graph)

### Calculated (15)
- Pdes, VR_nominal, D1_calculated, D2_calculated, VR_actual, vb_actual, Prated_total, Lp_required, B, CD_actual, theta1, theta2, Pcorrected, Nbelts_calc, Nbelts_required

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

### Table/Graph Integration
Extensive graph-based lookups:
1. Service factor table (7-1)
2. Belt section selection graph
3. Sheave size graphs (3V, 5V, 8V) - used for D1, D2, and Prated_base
4. Speed ratio power addition graph
5. Standard belt lengths table (7-2)
6. Wrap angle correction graph
7. Belt length correction graph

**Note**: All graph/table lookups require manual entry by user

### Validation Indicators
- Show green checkmark when CD is in acceptable range
- Show green checkmark when vb ≤ 5000 ft/min
- Show warning if 5000 < vb ≤ 6500 ft/min
- Show error if vb > 6500 ft/min
- Show final result: number of belts required

---

## UI Layout

### Three Tabs
1. **Procedure**: 15 steps with equations in order
2. **General Equations**: All formulas organized by category
3. **Variables**: Complete variable list with current values

### Session Controls
- Save Button: Saves all values to localStorage
- Load Button: Restores from localStorage
- Reset Button: Clears all values and localStorage

### PDF Display
- Left side: Mech_325_Bible_Fizz-v_belts.pdf
- Right side: Calculator interface
- Resizable split view

---

## Testing Scenarios

### Test 1: Basic V-Belt Design
- Hin = 10 hp
- SF = 1.3 (moderate shock)
- n1 = 1750 rpm
- Expected: 3V or 5V belt, calculate number required

### Test 2: High Power Application
- Hin = 50 hp
- Fast shaft speed
- Expected: 5V or 8V belt section

### Test 3: High Speed Ratio
- VR > 1.5
- Verify power addition calculation
- Check wrap angle adequate

---

## LaTeX Formatting

Use MathJax 3 for all equations:

```latex
P_{des} = H_{in} \cdot SF

VR = \frac{n_1}{n_2} = \frac{D_2}{D_1}

v_b = \frac{\pi D_1 n_1}{12}

L_p = 2CD + 1.57(D_2 + D_1) + \frac{(D_2 - D_1)^2}{4CD}

B = 4L_p - 6.28(D_2 + D_1)

CD = \frac{B + \sqrt{B^2 - 32(D_2 - D_1)^2}}{16}

\theta_1 = 180° - 2\sin^{-1}\left(\frac{D_2 - D_1}{2CD}\right)

P_{corrected} = C_\theta \cdot C_{Lp} \cdot P_{rated}

N_{belts} = \lceil \frac{P_{des}}{P_{corrected}} \rceil
```

---

## File Paths
- **Specification**: `specifications/belts/fizz_v_belts_spec.md`
- **Calculator**: `calculators/belts/fizz_v_belts.html`
- **PDF**: `../../documents/belts/Mech_325_Bible_Fizz-v_belts.pdf`
