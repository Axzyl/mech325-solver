# Fizz Timing Belt (Synchronous Belt) Design Calculator - Specification

## Overview
Complete timing belt (synchronous belt) design guide following the comprehensive fizz methodology. Organizes the GT-style timing belt selection process from Section 1.5 into a structured 11-step design procedure.

## Design Guidelines
- **All fields editable**: No color-coding, all inputs can be modified by user
- **Bidirectional solving**: Edit any variable, dependent values recalculate automatically
- **Session persistence**: Save/Load/Reset functionality via localStorage
- **PDF reference**: Mech_325_Bible_Fizz-time_belts.pdf displayed on left side

## Source Document
- **PDF**: `documents/belts/Mech_325_Bible_Fizz-time_belts.pdf`
- **Section**: §1.5 Synchronous Belt Drives (pages 18-25)
- **Reference Tables**:
  - Table 7-8: Service Factor
  - Graph: GT style belt pitch selection guide
  - Table 7-4: Sprockets with 8 mm Belt Pitch
  - Table 7-5: Taper-Lock Bushing
  - Table 7-7: 8-mm Pitch GT Drive Selection Table
  - Table 7-9: 8M GT Style Belt Power Rating Table (30-mm width)
  - Table 7-10: 8M GT Style Belt Power Rating Table (50-mm width)
  - Table 7-11: 8M GT Style Belt Length Correction Factor

## Design Procedure (11 Steps)

### Step 1: Design Power Calculation
**Purpose**: Determine design power with service factor

**Variables**:
- `Prated` = Rated power (hp) [User Input]
- `SF` = Service factor (dimensionless) [Table 7-8]
- `Pdes` = Design power (hp) [Equation]

**Equations**:
```
Pdes = Prated * SF
```

**Tables Required**:
- Table 7-8: Service Factor (based on driven machine type and motor type)

**Notes**:
- Service factor ranges from 1.0 to 2.5 depending on application
- Different values for AC motors vs DC motors
- Different values for intermittent vs continuous service

---

### Step 2: Belt Pitch Selection
**Purpose**: Select belt pitch from graph based on design power and RPM

**Variables**:
- `pitch` = Belt pitch (mm) [Graph or Dropdown: 5mm, 8mm, 14mm, 20mm]
- `RPM_faster_shaft` = Speed of faster shaft (rpm) [User Input]

**Graph**: GT style belt pitch selection guide (page 20)
- X-axis: Design horsepower
- Y-axis: RPM of faster shaft
- Regions: 5mm, 8mm, 14mm, 20mm

**Notes**:
- Textbook note: "You're probably going to get an 8mm pitch because that is all this textbook has data for..."
- Most subsequent tables assume 8mm pitch

---

### Step 3: Velocity Ratio
**Purpose**: Calculate speed ratio between driver and driven sprockets

**Variables**:
- `ndriving` = Driving sprocket speed (rpm) [User Input]
- `ndriven` = Driven sprocket speed (rpm) [User Input]
- `VR` = Velocity ratio (dimensionless) [Equation]

**Equations**:
```
VR = ndriving / ndriven
```

**Notes**:
- VR used to select sprocket tooth combinations
- VR = (teeth on driven) / (teeth on driver)

---

### Step 4: Sprocket Selection
**Purpose**: Select candidate combinations of driver and driven sprockets

**Variables**:
- `Ndriver_candidates` = Number of teeth on driver sprocket options [User Input - list]
- `Ndriven_candidates` = Number of teeth on driven sprocket options [User Input - list]

**Notes**:
- List multiple possible combinations
- VR = Ndriven / Ndriver
- Will eliminate some in next step

---

### Step 5: Sprocket Elimination - Shaft and Space Constraints
**Purpose**: Eliminate sprockets that don't meet physical constraints

**Sub-step 5a: Bushing and Bore Check**

**Variables**:
- `motor_shaft_diameter` = Motor shaft diameter (in) [User Input]
- `bushing_size` = Bushing size for driver sprocket [Table 7-7]
- `max_bore` = Maximum bore for bushing (in) [Table 7-5]

**Tables Required**:
- Table 7-7: Find bushing size based on driver sprocket teeth and belt width
- Table 7-5: Taper-Lock Bushing - find max bore from bushing size

**Validation**:
```
Require: max_bore > motor_shaft_diameter
```

**Sub-step 5b: Diameter Limit Check**

**Variables**:
- `flange_diameter_limit` = Maximum allowed sprocket diameter (in) [User Input]
- `flange_diameter_driver` = Driver sprocket flange diameter (in) [Table 7-4]
- `flange_diameter_driven` = Driven sprocket flange diameter (in) [Table 7-4]

**Table Required**:
- Table 7-4: Sprockets with 8 mm Belt Pitch - flange diameter column

**Validation**:
```
Require: flange_diameter_driver ≤ flange_diameter_limit (if specified)
Require: flange_diameter_driven ≤ flange_diameter_limit (if specified)
```

**Sub-step 5c: Final Selection**

**Variables**:
- `Ndriver_final` = Final driver sprocket teeth [User Input]
- `Ndriven_final` = Final driven sprocket teeth [User Input]

**Notes**:
- Should have one combination remaining
- Otherwise choose one that meets all requirements

---

### Step 6: Pitch Diameter Determination
**Purpose**: Find pitch diameters of selected sprockets

**Variables**:
- `PD1` = Pitch diameter of driver sprocket (in) [Table 7-4]
- `PD2` = Pitch diameter of driven sprocket (in) [Table 7-4]

**Table Required**:
- Table 7-4: Sprockets with 8 mm Belt Pitch
- Columns: Pitch diameter for 20-mm, 30-mm, 50-mm, 85-mm widths

**Notes**:
- Pitch diameter depends on belt width and number of teeth
- Use appropriate column based on belt width selection

---

### Step 7: Belt Length Selection
**Purpose**: Select belt with appropriate center distance

**Variables**:
- `CD_min` = Minimum center distance (in) [User Input]
- `CD_max` = Maximum center distance (in) [User Input]
- `belt_designation` = Belt pitch/length designation [Table 7-7]
- `CD_actual` = Actual center distance (in) [Table 7-7]

**Table Required**:
- Table 7-7: 8-mm Pitch GT Drive Selection Table
- Find row matching driver/driven teeth
- Select belt length with CD in acceptable range

**Notes**:
- Table shows center distances for various belt lengths
- Belt designation format: e.g., "800-8MGT", "1040-8MGT"
- Number indicates belt pitch length, 8M indicates 8mm pitch

---

### Step 8: Belt Width and Rated Power
**Purpose**: Select belt width and find base rated power

**Variables**:
- `belt_width` = Belt width (mm) [Dropdown: 20mm, 30mm, 50mm, 85mm]
- `Prated_table` = Rated power from table (hp) [Table 7-9 or 7-10]

**Tables Required**:
- Table 7-9: 8M GT Style Belt Power Rating Table — 30-mm Belt Width
- Table 7-10: 8M GT Style Belt Power Rating Table — 50-mm Belt Width
- Rows indexed by RPM of faster shaft
- Columns indexed by number of grooves on small sprocket and pitch diameter

**Lookup Process**:
- Row: RPM of faster shaft
- Column: Number of teeth on smaller sprocket (Ndriver typically) and pitch diameter
- Value: Base rated horsepower

**Notes**:
- Different tables for different belt widths
- Interpolation may be needed if exact values not in table

---

### Step 9: Belt Length Correction Factor
**Purpose**: Find correction factor based on belt length

**Variables**:
- `CL` = Length correction factor (dimensionless) [Table 7-11]

**Table Required**:
- Table 7-11: 8M GT Style Belt Length Correction Factor
- Indexed by pitch/length designation (e.g., 800-8MGT)

**Notes**:
- Typical values range from 0.70 to 1.20
- Shorter belts have lower correction factors

---

### Step 10: Adjusted Rated Power
**Purpose**: Calculate adjusted rated power with length correction

**Variables**:
- `Padj` = Adjusted rated power (hp) [Equation]

**Equations**:
```
Padj = Prated_table * CL
```

**Notes**:
- Value may differ significantly from Pdes (this is acceptable)
- Should verify Padj ≥ Pdes for adequate capacity

---

### Step 11: Belt Speed Verification
**Purpose**: Verify belt speed is within acceptable limits

**Variables**:
- `omega1` = Angular velocity of driver (rpm) [User Input, same as ndriving]
- `vbelt` = Belt speed (ft/min) [Equation]

**Equations**:
```
vbelt = (PD1/2) * omega1 * 2π * (1/12)
```

Simplified:
```
vbelt = π * PD1 * omega1 / 12
```

**Validation**:
```
Require: vbelt ≤ 6500 ft/min
```

**Notes**:
- PD1 in inches, omega1 in rpm
- Factor of 1/12 converts inches to feet
- 6500 ft/min is maximum recommended belt speed

---

## General Equations Tab

All equations organized by category:

### Design Power
```
Pdes = Prated * SF
```

### Kinematics
```
VR = ndriving / ndriven
VR = Ndriven / Ndriver
vbelt = π * PD1 * omega1 / 12
```

### Power Calculations
```
Padj = Prated_table * CL
```

### Validation
```
max_bore > motor_shaft_diameter
flange_diameter ≤ limit (if specified)
vbelt ≤ 6500 ft/min
Padj ≥ Pdes (recommended)
```

---

## Variables Summary

Total variables: ~25

### User Inputs (8)
- Prated, RPM_faster_shaft, ndriving, ndriven, motor_shaft_diameter, CD_min, CD_max, omega1

### Table Lookups (10+)
- SF (Table 7-8)
- pitch (Graph)
- bushing_size, max_bore (Tables 7-7, 7-5)
- flange_diameter_driver, flange_diameter_driven, PD1, PD2 (Table 7-4)
- belt_designation, CD_actual (Table 7-7)
- belt_width (Dropdown)
- Prated_table (Table 7-9/7-10)
- CL (Table 7-11)

### Calculated (7)
- Pdes, VR, Ndriver_final, Ndriven_final, Padj, vbelt

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
This design has extensive table lookups:
1. Service factor table (7-8)
2. Pitch selection graph
3. Sprocket properties table (7-4) - large table with multiple columns
4. Bushing table (7-5)
5. Belt selection table (7-7) - very large table with center distances
6. Power rating tables (7-9, 7-10) - large matrices
7. Length correction table (7-11)

**Note**: All table lookups require manual entry by user

### Validation Indicators
- Show green checkmark when max_bore > motor_shaft_diameter
- Show green checkmark when vbelt ≤ 6500 ft/min
- Show warning if flange_diameter exceeds limit
- Show status indicator if Padj ≥ Pdes

---

## UI Layout

### Three Tabs
1. **Procedure**: 11 steps with equations in order
2. **General Equations**: All formulas organized by category
3. **Variables**: Complete variable list with current values

### Session Controls
- Save Button: Saves all values to localStorage
- Load Button: Restores from localStorage
- Reset Button: Clears all values and localStorage

### PDF Display
- Left side: Mech_325_Bible_Fizz-time_belts.pdf
- Right side: Calculator interface
- Resizable split view

---

## Testing Scenarios

### Test 1: Basic Timing Belt Design
- Prated = 5 hp
- SF = 1.3 (light machinery)
- RPM = 1750 rpm
- VR = 2.0
- Expected: 8mm pitch, driver/driven selection

### Test 2: High Speed Application
- omega1 = 3450 rpm
- Verify vbelt < 6500 ft/min limit
- May need smaller pitch diameter

### Test 3: Space-Constrained Design
- Limit on flange diameter
- Small motor shaft
- Verify bore and diameter constraints

---

## LaTeX Formatting

Use MathJax 3 for all equations:

```latex
P_{des} = P_{rated} \cdot SF

VR = \frac{n_{driving}}{n_{driven}} = \frac{N_{driven}}{N_{driver}}

P_{adj} = P_{rated,table} \cdot C_L

v_{belt} = \frac{\pi \cdot PD_1 \cdot \omega_1}{12}
```

---

## File Paths
- **Specification**: `specifications/belts/fizz_time_belts_spec.md`
- **Calculator**: `calculators/belts/fizz_time_belts.html`
- **PDF**: `../../documents/belts/Mech_325_Bible_Fizz-time_belts.pdf`
