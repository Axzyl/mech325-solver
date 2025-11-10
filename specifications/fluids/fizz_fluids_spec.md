# CALCULATOR SPECIFICATION: Fluid Power System Design

**Calculator Name**: fizz_fluids
**Source Document**: `documents/fluids/fizz_fluids.pdf`
**Pages**: 16 (pages 76-91 in original document)
**Date Created**: 2025-11-09
**Specification Version**: 1.0

---

## 1. CALCULATOR METADATA

### 1.1 Purpose
This calculator provides a comprehensive 12-step procedure for designing hydraulic/fluid power systems. It determines minimum rod diameter based on load forces, selects bore sizes and working pressures, calculates flow rates for push/pull cycles (both regular and regenerative circuits), sizes pipes, and selects motors for hydraulic pumps.

### 1.2 Scope
- **Inputs**: Load forces, velocities, distances, mounting configuration, circuit type (regular/regenerative), working pressure
- **Outputs**: Minimum rod diameter, bore diameter, flow rates, pipe size, motor horsepower
- **Target Users**: Mechanical engineers designing hydraulic/pneumatic systems

### 1.3 Limitations
- Assumes standard mounting configurations (6 types)
- Maximum pipe flow velocity fixed at 15 ft/s
- Assumed pump efficiency of 85%
- Graph lookups require interpolation (rod diameter, acceleration factor g)
- Large tables for push/pull forces (bore/rod combinations)
- Accumulator calculations not covered

### 1.4 Assumptions
- Default pump efficiency: 85%
- Default maximum pipe velocity: 15 ft/s
- Default pipe schedule: 80 (medium pressure)
- Coefficient of friction µ provided or assumed negligible
- Standard rod and bore sizes from tables

---

## 2. VARIABLES

### 2.1 Variable Definitions Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Input Variables - System Configuration** |
| circuit_type | Circuit type | Regular or Regenerative | - | User Input | Section 3.1 | Yes | Affects force/flow calculations |
| mounting_style | Mounting style | Cylinder mounting configuration | - | User Input | Step 2 | Yes | Determines stroke factor k |
| **Input Variables - Load Forces** |
| F_w | Weight force | Force due to weight of load | lbf | User Input | Step 1b | Yes | - |
| µ | Coefficient of friction | Friction coefficient | - | User Input | Step 1c | Yes | May be omitted if not given |
| F_f | Friction force | Force due to friction | lbf | Calculated | Step 1c: F_f = µ·F_w | Yes | Only if µ provided |
| v | Speed | Required velocity | ft/min | User Input | Step 1d | Yes | - |
| s | Distance | Distance to achieve velocity | in | User Input | Step 1d | Yes | For acceleration calculation |
| g | Acceleration constant | Acceleration factor from table/formula | - | Graph Lookup / Calculated | Step 1e: g = (v²/s)(0.0000517) | Yes | Yellow input for graph, green for formula |
| F_a | Acceleration force | Force from acceleration | lbf | Calculated | Step 1f: F_a = F_w·g | Yes | - |
| F | Total force (thrust) | Total force on piston | lbf | Calculated | Step 1g: F = F_w + F_f + F_a | Yes | - |
| **Stroke and Length Variables** |
| L | Stroke length | Cylinder stroke length | in | User Input | Step 3 | Yes | Given in problem |
| k | Stroke factor | Factor based on mounting style | - | Table Lookup | Step 2 | Yes | Yellow input, from mounting table |
| L_b | Basic length | Basic cylinder length | in | Calculated | Step 3: L_b = kL | Yes | - |
| **Rod and Bore Variables** |
| d_rod_min | Minimum rod diameter | Minimum rod diameter from graph | in | Graph Lookup | Step 4 | Yes | Yellow input, function of L_b and F |
| d_rod | Selected rod diameter | Chosen standard rod diameter | in | Table Lookup | Step 4 | Yes | Round up to standard size |
| P | Working pressure | System working pressure | psi | User Input | Step 5 | Yes | - |
| d_bore_calc | Calculated bore diameter | Calculated minimum bore | in | Calculated | Step 5: √(4F/πP) | Yes | For regular push piston |
| d_bore | Selected bore diameter | Chosen standard bore diameter | in | Table Lookup | Step 5 | Yes | Round up to standard size |
| A_1 | Piston area (bore-rod) | Annular area (bore - rod) | in² | Calculated | π/4·(d_bore² - d_rod²) | Yes | - |
| A_2 | Rod area | Cross-sectional area of rod | in² | Calculated | π/4·d_rod² | Yes | - |
| F_push | Push force | Force developed on push stroke | lbf | Calculated | Depends on circuit type | Yes | Regular: P(A₁+A₂), Regen: P·A₂ |
| F_pull | Pull force | Force developed on pull stroke | lbf | Calculated | Depends on circuit type | Yes | Both: P·A₁ |
| **Flow Rate Variables** |
| v_push | Push velocity | Piston velocity during push | ft/min | User Input | Step 6 | Yes | May be same as v or different |
| v_pull | Pull velocity | Piston velocity during pull | ft/min | User Input | Step 6 | Yes | May be same as v or different |
| Q_push | Push flow rate | Flow rate during push stroke | gpm | Calculated | Step 7: Formulas vary by circuit | Yes | - |
| Q_pull | Pull flow rate | Flow rate during pull stroke | gpm | Calculated | Step 7: Formulas vary by circuit | Yes | - |
| Q | Pump flow rate | Selected pump capacity | gpm | User Input / Calculated | Step 7 | Yes | Use maximum of Q_push, Q_pull |
| **Pipe Sizing Variables** |
| v_pipe_max | Max pipe velocity | Maximum pipe flow velocity | ft/s | User Input | Step 8 | Yes | Default 15 ft/s |
| d_pipe_min | Min pipe diameter | Minimum pipe diameter | in | Calculated | Step 8: √(4Q/πv)·(231/720) | Yes | - |
| pipe_schedule | Pipe schedule | Pipe wall thickness schedule | - | User Input | Step 9 | Yes | 40/80/160; default 80 |
| d_pipe_inside | Pipe inside diameter | Selected inside diameter | in | Table Lookup | Step 9/10 | Yes | Yellow input from table |
| nominal_pipe_size | Nominal pipe size | Standard nominal pipe size | in | Table Lookup | Step 9/10 | Yes | Yellow input from table |
| **Motor Selection Variables** |
| η | Pump efficiency | Pump efficiency | - | User Input | Step 11 | Yes | Default 0.85 |
| HP_required | Required horsepower | Motor horsepower required | hp | Calculated | Step 11: Q·P/(1714·η) | Yes | - |
| HP_motor | Selected motor HP | Standard motor horsepower | hp | Table Lookup | Step 12 | Yes | Yellow input, round up |
| motor_voltage | Motor voltage | Voltage (single/3-phase) | V | User Input | Step 12 | Yes | Affects motor selection |
| **Circuit Analysis Variables** |
| A_piston | Piston area | Area for flow calculation | in² | Calculated | Section 3.3.1 | Yes | Context-dependent |
| v_piston | Piston velocity | Velocity for flow calc | in/s or ft/min | User Input | Section 3.3.1 | Yes | - |
| Q_analysis | Flow rate | Q = A·v | in³/s or gpm | Calculated | Section 3.3.1 | Yes | - |
| Power | Hydraulic power | Power = PQ/1714 | hp | Calculated | Section 3.3.2 | Yes | - |

### 2.2 Variable Categories by Source Type

**User Inputs (White)**: circuit_type, mounting_style, F_w, µ, v, s, L, P, v_push, v_pull, v_pipe_max, pipe_schedule, η, motor_voltage, v_piston

**Table/Graph Lookups (Yellow)**: g (graph option), k, d_rod_min, d_rod, d_bore, d_pipe_inside, nominal_pipe_size, HP_motor

**Calculated (Green)**: F_f, g (formula option), F_a, F, L_b, d_bore_calc, A_1, A_2, F_push, F_pull, Q_push, Q_pull, Q, d_pipe_min, HP_required, Q_analysis, Power

---

## 3. EQUATIONS

### 3.1 Core Equations

**Step 1c: Friction Force**
```latex
F_f = \mu F_w
```

**Step 1e: Acceleration Constant (Formula)**
```latex
g = \left(\frac{v^2}{s}\right)(0.0000517)
```
Where: v in ft/min, s in inches

**Step 1f: Acceleration Force**
```latex
F_a = F_w g
```

**Step 1g: Total Force (Thrust)**
```latex
F = F_w + F_f + F_a
```

**Step 3: Basic Length**
```latex
L_b = kL
```

**Step 5: Bore Diameter Calculation (Regular Push Piston)**
```latex
d_{bore} = \sqrt{\frac{4F}{\pi P}}
```

**Step 5: Piston Areas**
```latex
A_1 = \frac{\pi}{4}(d_{bore}^2 - d_{rod}^2)
```
```latex
A_2 = \frac{\pi}{4} d_{rod}^2
```

**Step 5: Rated Force from Chosen Bore**
```latex
F = \frac{\pi}{4} P d_{bore}^2
```

**Step 7: Pump Capacity Calculation**

For Regular Push:
```latex
Q_p = \frac{\pi}{4} v_{push} d_{bore}^2 \left(\frac{12}{231}\right)
```

For Regular Pull:
```latex
Q_p = \frac{\pi}{4} v_{push} (d_{bore}^2 - d_{rod}^2) \left(\frac{12}{231}\right)
```

For Regenerative Push:
```latex
Q_p = \frac{\pi}{4} v_{pull} d_{rod}^2 \left(\frac{12}{231}\right)
```

For Regenerative Pull:
```latex
Q_p = \frac{\pi}{4} v_{push} (d_{bore}^2 - d_{rod}^2) \left(\frac{12}{231}\right)
```

**Step 8: Minimum Pipe Diameter**
```latex
d_p = \sqrt{\frac{4Q}{\pi v} \left(\frac{231}{720}\right)}
```
Where: v = 15 ft/s (default), Q in gpm

**Step 11: Required Horsepower**
```latex
HP = \frac{Q \cdot P}{1714 \cdot 0.85}
```
Or with variable efficiency:
```latex
HP = \frac{Q \cdot P}{1714 \cdot \eta}
```

**Section 3.3.1: Flow Rate from Area and Velocity**
```latex
Q = Av
```
Conversion:
```latex
1 \frac{\text{in}^3}{\text{s}} = \frac{60}{231} \text{ GPM}
```

**Section 3.3.2: Hydraulic Power**
```latex
\text{Power} = P \frac{dV}{dt} = \frac{PQ}{1714}
```

### 3.2 Piecewise/Conditional Equations

**Push and Pull Forces by Circuit Type**

| Circuit Type | Push Force | Pull Force |
|-------------|------------|------------|
| Regular | F = P·(A₁ + A₂) | F = P·A₁ |
| Regenerative | F = P·A₂ | F = P·A₁ |

**Flow Rate by Circuit Type and Stroke**

|  | Regular | Regenerative |
|---|---------|--------------|
| **Push** | Q = (π/4)·v_push·d_bore²·(12/231) | Q = (π/4)·v_pull·d_rod²·(12/231) |
| **Pull** | Q = (π/4)·v_push·(d_bore² - d_rod²)·(12/231) | Q = (π/4)·v_push·(d_bore² - d_rod²)·(12/231) |

**Stroke Factor (k) by Mounting Style** - From table on page 5:

| Class/Style | Configuration | Rod End Connection | Stroke Factor k |
|-------------|---------------|-------------------|-----------------|
| Class 1, Group 1/3 | Long stroke, heavy-duty | Fixed (not rigidly guided) | 0.50 |
| Class 1, Group 1/3 | Same | Pivoted (not rigidly guided) | 0.70 |
| Class 1, Group 1/3 | Same | Supported but not rigidly guided | 2.00 |
| Class 2, Group 2 | Trunnion on Head | Pivoted and rigidly guided | 1.00 |
| Class 2, Group 2 | Intermediate Trunnion | Pivoted and rigidly guided | 1.50 |
| Class 2, Group 2 | Trunnion on Cap/Clevis on Cap | Pivoted and rigidly guided | 2.00 |

**Pipe Schedule Selection** (Step 9):
- Low pressure: Schedule 40
- Medium pressure: Schedule 80 (default)
- High pressure: Schedule 160

---

## 4. PROCEDURE STEPS

### 4.1 Main Design Procedure (12 Steps)

| Step | Action Type | Description | Inputs Required | Outputs Generated | Notes |
|------|-------------|-------------|-----------------|-------------------|-------|
| 1 | Calculate | Find total force acting on piston | F_w, µ (optional), v, s | F_f, g, F_a, F | Substeps: weight, friction, acceleration, sum |
| 1a | Manual | Free body analysis for complex systems | System diagram | Forces and velocities | User analysis, not automated |
| 1b | Input | Find force due to weight of load | F_w | - | Direct input |
| 1c | Calculate | Find force due to friction (if µ given) | µ, F_w | F_f | F_f = µ·F_w; omit if no friction |
| 1d/e | Graph/Calculate | Get acceleration constant g from table or formula | v, s | g | Yellow (graph) or green (formula) |
| 1f | Calculate | Compute acceleration force | F_w, g | F_a | F_a = F_w·g |
| 1g | Calculate | Sum forces to get total thrust | F_w, F_f, F_a | F | F = F_w + F_f + F_a |
| 2 | Table Lookup | Determine stroke factor k from mounting style | mounting_style | k | Yellow input, table on page 5 |
| 3 | Calculate | Compute basic length L_b | L, k | L_b | L_b = kL |
| 4 | Graph Lookup | Choose minimum rod diameter from graph | L_b, F | d_rod_min, d_rod | Yellow input, round up to standard |
| 5 | Calculate/Lookup | Choose bore size using pressure and force | F, P, d_rod (optional) | d_bore_calc, d_bore, F_push, F_pull | Table lookup or formula; depends on circuit type |
| 6 | Input | Convert velocity to ft/min if needed | v | v in ft/min | Ensure consistent units |
| 7 | Calculate | Compute pump capacity Q for push and pull | v_push, v_pull, d_bore, d_rod, circuit_type | Q_push, Q_pull, Q | Different formulas for regular/regenerative |
| 8 | Calculate | Compute minimum pipe diameter | Q, v_pipe_max | d_pipe_min | v_max typically 15 ft/s |
| 9 | Table Lookup | Select pipe size using schedule 80 table | d_pipe_min, pipe_schedule | nominal_pipe_size, d_pipe_inside | Yellow input, Method 1, page 10 |
| 10 | Table Lookup | Alternative: Select pipe size from flow table | d_pipe_min, d_bore | nominal_pipe_size | Yellow input, Method 2, pages 11-12 |
| 11 | Calculate | Compute required motor horsepower | Q, P, η | HP_required | η default 0.85 |
| 12 | Table Lookup | Select standard motor based on HP and voltage | HP_required, motor_voltage | HP_motor | Yellow input, page 13 |

### 4.2 Circuit Analysis Procedures (Section 3.3)

**Flowrate Analysis**:
- Calculate Q = A·v
- Convert units: 1 in³/s = 60/231 GPM

**Power Analysis**:
- Calculate Power = PQ/1714
- Q in GPM, P in psi, Power in hp

### 4.3 Conceptual Understanding (Section 3.1)

**Regenerative vs Non-Regenerative Circuits**:

- **Non-Regenerative**: Always has weaker/faster retraction; ideal for hydraulic presses where push is primary goal
- **Regenerative**: Allows balancing push/pull forces by making A₁ = A₂; greater complexity but more control

### 4.4 Decision Points

**Step 4 vs Step 5 Note**: Rod diameter from buckling (step 4) gives minimum. Design requirements for balancing push/pull or using regenerative circuits may require larger bore (step 5).

**Step 9 vs Step 10**: Two methods for pipe sizing:
- Method 1 (Step 9): Simple schedule-based table (page 10)
- Method 2 (Step 10): Detailed flow velocity table (pages 11-12)

**Circuit Type Selection**: User must decide between regular and regenerative based on:
- Force balance requirements (equal push/pull vs asymmetric)
- System complexity tolerance
- Application type (press vs balanced actuator)

---

## 5. ADDITIONAL NOTES

### 5.1 Table References

**Acceleration Factor Graph** (page 4):
- X-axis: g (acceleration constant) from 0.01 to 2.0
- Y-axis: v (velocity in ft/min) from 10 to 300
- Diagonal lines: s (distance in inches) from 0.25 to 1.75
- Used in Step 1d/e

**Stroke Factor Table** (page 5):
- 6 mounting configurations with stroke factors
- Class 1 (Groups 1 or 3): k = 0.50, 0.70, 2.00
- Class 2 (Group 2): k = 1.00, 1.50, 2.00

**Piston Rod - Stroke Selection Graph** (page 6):
- X-axis: Thrust (pounds) from 100 to 100,000 (log scale)
- Y-axis: Basic Length (inches) from 1 to 100
- Diagonal lines: Rod diameter from 5/8" to 5.5"
- Used in Step 4

**Theoretical Push and Pull Forces Table (U.S. Units)** (page 7):
- Rows: Cylinder bore/piston rod diameter combinations (5/8" to 12")
- Columns: Pressures (50 to 3000 psi)
- Values: Force in pounds
- Last column: Displacement per inch of stroke (gallons)
- Used in Step 5

**Theoretical Push and Pull Forces Table (SI Units)** (page 8):
- Same structure as U.S. table
- Metric units: mm, bars, newtons, liters

**Hydraulic Pipe Specifications Table** (page 10):
- Nominal Size (1/4" to 3")
- Pipe OD
- Inside Diameter for Schedule 40, 80, 160
- Used in Step 9 (Method 1)

**Fluid Velocity Table** (pages 11-12):
- Rows: Cylinder bore (1" to 14")
- Columns: Piston rod sizes, area, displacement at 10 ft/min, flow velocities at pipe sizes 1/4" to 2-1/2"
- Very detailed, multi-page table
- Used in Step 10 (Method 2)

**Electric Motor Horsepower Table** (page 13):
- Rows: GPM (1/2 to 65)
- Columns: Pressures (100 to 3000 psi)
- Values: Required horsepower
- Used in Step 11

**Motor Starters Tables** (page 14):
- Three tables: 1/2 to 20 HP, 25 to 200 HP, 1/6 to 5 HP
- Single-phase and three-phase configurations
- Voltage, amps, wire sizes
- Used in Step 12

### 5.2 Diagrams and Visual Aids

**Non-Regenerative Circuit Diagram** (page 2):
- Shows pump, adjustable relief valve, directional control valve, cylinder
- Push cycle: full force on bore area
- Pull cycle: faster retraction, lower force

**Regenerative Circuit Diagram** (pages 2, 9):
- Figure 13-22
- Shows recycling fluid path
- Piston area diagram showing A₁ (annular area) and A₂ (rod area)
- Push: equal pressure both sides, net force = P·A₂
- Pull: operates like non-regen, force = P·A₁

**Sequence Valve Circuit Diagram** (page 15):
- Shows two cylinders with sequence valves
- Controls sequential actuation
- Left extends, then right extends
- Right retracts, then left retracts

**Accumulator Diagram** (page 16):
- Visual showing three accumulator states
- Hydraulic equivalent of capacitors
- Calculations beyond course scope

### 5.3 Special Considerations

**Standard Rod Sizes** (from Step 4):
5/8", 1", 1-3/8", 1-3/4", 2", 2-1/2", 3", 3-1/2", 4", 4-1/2", 5", 5-1/2"

**Standard Bore Sizes** (from tables):
5/8", 1", 1-3/8", 1-1/2", 1-3/4", 2", 2-1/2", 3", 3-1/4", 3-1/2", 4", 5", 5-1/2", 6", 7", 8", 8-1/2", 10", 12"

**Conversion Factors**:
- 1 in³/s = (60/231) GPM
- Flow rate to HP: divide by 1714
- Pipe diameter formula includes (231/720) conversion

**Push vs Pull Force Asymmetry** (Non-Regenerative):
- Push: Acts on full bore area (A₁ + A₂)
- Pull: Acts only on annular area (A₁)
- Result: Pull is weaker but faster for same flow rate

**Regenerative Circuit Balance**:
- To equalize forces: Set A₁ = A₂
- This means: π/4(d_bore² - d_rod²) = π/4·d_rod²
- Simplifies to: d_bore² = 2·d_rod²
- Or: d_bore = d_rod·√2 ≈ 1.414·d_rod

**Sequence Valves** (Section 3.3.3):
- Actuate when minimum pressure reached
- Typically when piston reaches max extension and stops moving
- Controls order of cylinder operation

**Accumulators** (Section 3.3.3):
- Store pressure using air
- Provide intermittent pressure during outages
- Dampen shocks (like signal filtering)
- Not covered in calculation procedures

---

## 6. IMPLEMENTATION READY CHECKLIST

### 6.1 Data Requirements
- [x] All equations documented in LaTeX format
- [x] All variables defined with units
- [x] Input/output relationships mapped
- [x] Table lookup requirements identified (9 tables)
- [x] Graph lookup requirements identified (2 graphs)
- [x] Piecewise conditions documented

### 6.2 UI/UX Requirements
- [ ] Three-tier input system (white/yellow/green) implemented
- [ ] MathJax rendering for all equations
- [ ] PDF viewer with page navigation (16 pages)
- [ ] Session save/load/reset functionality
- [ ] Circuit type selector (Regular/Regenerative)
- [ ] Mounting style selector (6 configurations)
- [ ] Dynamic formula switching based on circuit type
- [ ] Push vs Pull calculation sections
- [ ] Method selection for pipe sizing (Method 1 vs 2)
- [ ] Voltage selection for motor (single vs 3-phase)

### 6.3 Calculation Logic
- [ ] Friction force optional (if µ provided)
- [ ] Acceleration constant: graph lookup OR formula calculation
- [ ] Conditional push/pull force formulas (regular vs regenerative)
- [ ] Conditional flow rate formulas (4 combinations)
- [ ] Maximum selection (Q = max(Q_push, Q_pull))
- [ ] Standard size rounding logic (rod, bore, pipe, motor HP)
- [ ] Unit conversions (ft/min ↔ in/s, GPM ↔ in³/s)

### 6.4 Validation Requirements
- [ ] Constraint checks:
  - All forces > 0
  - Velocities > 0
  - Pressure > 0
  - Bore > Rod diameter
  - Stroke length > 0
- [ ] Unit consistency checks
- [ ] Standard size availability checks
- [ ] Graph interpolation for non-tabulated values

### 6.5 Output Requirements
- [ ] Total force F (thrust)
- [ ] Selected rod diameter d_rod
- [ ] Selected bore diameter d_bore
- [ ] Push force F_push and Pull force F_pull
- [ ] Flow rates Q_push and Q_pull
- [ ] Pump capacity Q
- [ ] Pipe size (nominal and inside diameter)
- [ ] Required horsepower HP_required
- [ ] Selected motor HP_motor
- [ ] Intermediate results (L_b, A_1, A_2, etc.)

### 6.6 Known Challenges
- **Graph lookups**: 2 graphs require visual interpolation (could provide formula approximations)
- **Large force table**: Page 7 has extensive bore/rod/pressure combinations (~200 entries)
- **Large flow velocity table**: Pages 11-12 span many bore/rod/pipe combinations
- **Circuit type complexity**: Different formulas for Regular vs Regenerative, Push vs Pull (4 combinations)
- **Two pipe sizing methods**: User must choose Method 1 or Method 2
- **Standard sizes**: Must validate chosen standard sizes exist in tables
- **Motor selection**: Different tables for single-phase vs three-phase

---

## 7. REVIEW CHECKLIST

- [x] All pages of PDF reviewed (16 pages)
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

**Estimated Implementation Complexity**: HIGH
- 40+ variables
- 20+ equations
- 12-step main procedure
- 9 table lookups
- 2 graph lookups
- Circuit type branching (2 types × 2 strokes = 4 formula sets)
- 2 alternative methods for pipe sizing
- Extensive standard size tables

**Recommended Implementation Approach**:
1. Start with force calculation section (Steps 1-4)
2. Add bore/pressure selection (Step 5) with both regular and regenerative options
3. Implement flow rate calculations (Steps 6-7) with circuit type switching
4. Add pipe sizing (Steps 8-10) with method selection
5. Complete with motor selection (Steps 11-12)
6. Provide clear visual distinction between Regular and Regenerative circuit modes
7. Use tabs or sections to separate Push vs Pull calculations
8. Implement graph formula approximations where possible to reduce manual lookups
9. Provide table search/filter functionality for large tables
