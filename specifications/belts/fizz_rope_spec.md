# Fizz Wire Rope Design Calculator - Specification

## Overview
Complete wire rope design guide following the comprehensive fizz methodology. Organizes formulas from Section 1.7 into a structured 8-step design process covering both static and fatigue analysis.

## Design Guidelines
- **All fields editable**: No color-coding, all inputs can be modified by user
- **Bidirectional solving**: Edit any variable, dependent values recalculate automatically
- **Session persistence**: Save/Load/Reset functionality via localStorage
- **PDF reference**: Mech_325_Bible_Fizz-rope.pdf displayed on left side

## Source Document
- **PDF**: `documents/belts/Mech_325_Bible_Fizz-rope.pdf`
- **Section**: §1.7 Wire Rope (pages 31-34)
- **Reference Tables**:
  - Table 17-24: Wire-Rope Data
  - Table 17-27: Useful Properties of 6×7, 6×19, and 6×37 Wire Ropes
  - Table 17-26: Maximum Allowable Bearing Pressures
  - Graph: Pressure-strength ratio vs number of bends to failure

## Rope Types
- **Regular-lay**: Wires in strand twisted in one direction, strands in rope twisted in opposite direction
- **Lang-lay**: Wires and strands twisted in same direction (more flexible)

## Design Procedure (8 Steps)

### Step 1: Loading Requirements
**Purpose**: Define load conditions and rope configuration

**Variables**:
- `W` = Weight at end of rope (load) (lbf) [User Input]
- `m` = Number of ropes supporting load (dimensionless) [User Input]
- `w` = Weight per foot of rope (lb/ft) [User Input or Table]
- `l` = Maximum suspended length of rope (ft) [User Input]
- `a` = Maximum acceleration/deceleration (ft/s²) [User Input]
- `g` = Acceleration of gravity = 32.17 (ft/s²) [Constant]

**Equations**: None (inputs only)

**Notes**:
- For static loads, set a = 0
- Multiple ropes share load equally

---

### Step 2: Rope Selection and Properties
**Purpose**: Select rope type and determine material properties from tables

**Variables**:
- `rope_type` = Rope construction [Dropdown: 6×7 haulage, 6×19 standard, 6×37 special, 8×19 extra, 7×7 aircraft, 7×9 aircraft, 19-wire aircraft]
- `rope_lay` = Lay type [Dropdown: Regular-lay, Lang-lay]
- `material_type` = Wire material [Dropdown: Monitor steel, Plow steel, Mild plow steel, Corrosion-resistant steel, Carbon steel]
- `d` = Nominal wire rope size (in) [User Input or Table]
- `dw` = Diameter of individual wire (in) [Table 17-27]
- `Am` = Metal cross-sectional area (in²) [Table 17-27]
- `Er` = Young's modulus (psi) [Table 17-27]
- `Su` = Ultimate tensile strength (kpsi) [Table 17-24]
- `D_min` = Minimum sheave diameter (in) [Table 17-24]
- `D_better` = Better sheave diameter (in) [Table 17-27]

**Equations**: None (table lookups)

**Tables Required**:
- Table 17-24: Wire-Rope Data (weight, min sheave, strength)
- Table 17-27: Some Useful Properties (dw, Am, Er, better sheave diameter)

**Notes**:
- Lang-lay is more flexible but requires different sheave material
- Better sheave diameter extends rope life

---

### Step 3: Rope Tension Calculation
**Purpose**: Calculate actual tension in rope during operation

**Variables**:
- `Ft` = Tensile force on rope (lbf) [Equation]

**Equations**:
```
Ft = (W/m + w*l) * (1 + a/g)
```

**Notes**:
- First term (W/m + w*l) accounts for load and rope weight
- Second term (1 + a/g) accounts for dynamic loading
- For static: a = 0, so factor = 1.0

---

### Step 4: Sheave/Drum Diameter Selection
**Purpose**: Select drum or sheave diameter

**Variables**:
- `D` = Sheave or drum diameter (in) [User Input]

**Equations**: None (design choice)

**Validation**:
- Check D ≥ D_min from table
- Preferably D ≥ D_better for longer life

**Notes**:
- Larger diameter reduces bending stress
- Smaller diameter saves space but shortens rope life

---

### Step 5: Ultimate Strength Verification
**Purpose**: Verify rope can handle static load without yielding

**Variables**:
- `Fu` = Ultimate strength capacity (lbf) [Equation]
- `F_static` = Static force (for reference) [Equation]

**Equations**:
```
Fu = Su * d * D / 2000
F_static = (W/m + w*l)  [for comparison]
```

**Notes**:
- Su is in kpsi, convert to lbf
- This is the breaking strength
- Should have substantial safety factor

---

### Step 6: Fatigue Analysis
**Purpose**: Calculate fatigue strength and equivalent bending load

**Variables**:
- `p_over_Su` = Specified life pressure ratio (dimensionless) [Graph or User Input]
- `Ff` = Fatigue tension capacity (lbf) [Equation]
- `Fb` = Equivalent bending load (lbf) [Equation]

**Equations**:
```
Ff = (p/Su) * Su * D * d / 2
Fb = Er * dw * Am / D
```

**Notes**:
- p/Su from graph (page 34) based on number of bends to failure
- Er typically 12-13 × 10⁶ psi for steel wire rope
- Fb represents stress equivalent from bending around sheave

---

### Step 7: Factor of Safety Verification
**Purpose**: Calculate and verify safety factors

**Variables**:
- `nf` = Fatigue factor of safety (dimensionless) [Equation]
- `ns` = Static factor of safety (dimensionless) [Equation]

**Equations**:
```
nf = (Ff - Fb) / Ft
ns = (Fu - Fb) / Ft
```

**Validation**:
- Fatigue FOS: nf ≥ 2.0 recommended
- Static FOS: ns ≥ 5.0 typical for lifting

**Notes**:
- Both factors account for bending load reduction
- Lower FOS acceptable for non-lifting applications

---

### Step 8: Bearing Pressure Check
**Purpose**: Verify sheave material can handle bearing pressure

**Variables**:
- `P` = Bearing pressure (psi) [Equation]
- `P_allow` = Allowable bearing pressure (psi) [Table 17-26]
- `sheave_material` = Sheave material [Dropdown: Wood, Cast Iron, Cast Steel, Chilled Cast Iron, Manganese Steel]

**Equations**:
```
P = 2*Ft / (d*D)
```

**Tables Required**:
- Table 17-26: Maximum Allowable Bearing Pressures of Ropes on Sheaves

**Validation**:
```
Require: P ≤ P_allow
```

**Notes**:
- Higher bearing pressure requires harder sheave material
- Lang-lay requires better sheave material than regular-lay

---

## General Equations Tab

All equations organized by category:

### Rope Tension
```
Ft = (W/m + w*l) * (1 + a/g)
F_static = W/m + w*l
```

### Strength Calculations
```
Fu = Su * d * D / 2000
Ff = (p/Su) * Su * D * d / 2
Fb = Er * dw * Am / D
```

### Factors of Safety
```
nf = (Ff - Fb) / Ft
ns = (Fu - Fb) / Ft
```

### Bearing Pressure
```
P = 2*Ft / (d*D)
Require: P ≤ P_allow
```

---

## Variables Summary

Total variables: ~20

### User Inputs (6)
- W, m, l, a, D, d (some may come from tables)

### Constants (1)
- g = 32.17 ft/s²

### Table Lookups (10)
- rope_type, rope_lay, material_type (dropdowns)
- w, dw, Am, Er (from Table 17-27)
- Su, D_min, D_better (from Table 17-24)
- P_allow, sheave_material (from Table 17-26)
- p_over_Su (from graph)

### Calculated (8)
- Ft, Fu, F_static, Ff, Fb, nf, ns, P

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

### Table Integration
- Table 17-24: Basic rope data (requires manual lookup)
- Table 17-27: Detailed properties for common ropes (manual lookup)
- Table 17-26: Bearing pressure limits (manual lookup)
- Graph (p.34): p/Su ratio vs bends to failure (manual lookup)

### Validation Indicators
- Show green checkmark when nf ≥ 2.0 and ns ≥ 5.0
- Show red warning when P > P_allow
- Show warning if D < D_min or D < D_better

---

## UI Layout

### Three Tabs
1. **Procedure**: 8 steps with equations in order
2. **General Equations**: All formulas organized by category
3. **Variables**: Complete variable list with current values

### Session Controls
- Save Button: Saves all values to localStorage
- Load Button: Restores from localStorage
- Reset Button: Clears all values and localStorage

### PDF Display
- Left side: Mech_325_Bible_Fizz-rope.pdf
- Right side: Calculator interface
- Resizable split view

---

## Testing Scenarios

### Test 1: Static Lifting
- W = 5000 lbf
- m = 1 rope
- l = 100 ft
- a = 0 (static)
- Rope: 6×19 monitor steel, d = 1 in

Expected: Calculate Ft, verify ns ≥ 5

### Test 2: Dynamic Elevator
- W = 3000 lbf
- m = 4 ropes
- a = 8 ft/s² (acceleration)
- Verify both static and fatigue FOS

### Test 3: Aircraft Cable
- Use 7×7 or 7×9 aircraft cable
- Corrosion-resistant steel
- Verify high strength-to-weight ratio

---

## LaTeX Formatting

Use MathJax 3 for all equations:

```latex
F_t = \left(\frac{W}{m} + wl\right)\left(1 + \frac{a}{g}\right)

F_u = \frac{S_u \cdot d \cdot D}{2000}

F_f = \frac{(p/S_u) \cdot S_u \cdot D \cdot d}{2}

F_b = \frac{E_r \cdot d_w \cdot A_m}{D}

n_f = \frac{F_f - F_b}{F_t}

n_s = \frac{F_u - F_b}{F_t}

P = \frac{2F_t}{dD}
```

---

## File Paths
- **Specification**: `specifications/belts/fizz_rope_spec.md`
- **Calculator**: `calculators/belts/fizz_rope.html`
- **PDF**: `../../documents/belts/Mech_325_Bible_Fizz-rope.pdf`
