# Calculator Specification: Wire Rope Design (Mott & Shigley Methods)

---

## 1. Metadata

- **Calculator Name:** Wire Rope Design Calculator (Mott & Shigley Methods)
- **Source PDF:** Mech_325_Bible_Mech-rope.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Mech-rope.pdf`
- **Description:** Calculates wire rope design parameters including drum/sheave geometry, tensions, safety factors for standard design (Mott), general design (Shigley), and mine hoist applications (Shigley).
- **Complexity:** Very High (extensive table lookups required)
- **Tabs to Include:**
  - [x] Procedure (3 methods: Mott, Shigley, Mine Hoist)
  - [x] General Equations (20+ equations)
  - [x] Variables (40+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has three distinct methodologies requiring separate workflows
- **General Equations Tab**: Contains 20+ equations across all methods
- **Variables Tab**: 40+ variables require central management and overview

**Note:** This calculator requires extensive manual lookups from tables and graphs in the PDF. The PDF author notes: "designs simply require way too many table look-ups, and use very simple equations outside of the look-ups."

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **Common Variables** |
| d | Rope Diameter | Diameter of wire rope | in | User Input / Table | Table 7-19 | Maybe | Standard nominal sizes |
| D | Tread/Sheave Diameter | Diameter of drum/sheave tread | in | Equation / Table | Table 7-22 | Maybe | D = Ratio × d |
| rope_type | Rope Type | Classification of rope | - | User Input / Table | Table 7-20 | Maybe | 6×7, 6×19, 6×36, 6×61, etc. |
| D_d_ratio | D/d Ratio | Sheave to rope diameter ratio | - | Table | Table 7-22 | Maybe | Suggested or minimum value |
| **Mott Method - Drum Design** |
| h_drum | Drum Groove Depth | Minimum groove depth for drum | in | Equation | EQ1 | No | h_drum = 0.374d |
| drum_diameter | Drum Diameter | Overall drum diameter | in | Equation | EQ2 | No | D + 2h |
| fleet_angle | Fleet Angle | Angle of rope approach | degrees | User Input | - | No | 0.5° to 2° |
| r | Groove Radius | Radius of groove | in | Table | Table 7-23 | Maybe | New vs worn condition |
| p | Pitch Distance | Distance between grooves | in | User Input | - | No | 2.065r < p < 2.18r |
| **Mott Method - Sheave Design** |
| h_sheave | Sheave Groove Depth | Groove depth for sheave | in | User Input | - | No | 1.5d ≤ h_sheave ≤ 1.75d |
| throat_angle | Throat Angle | Sheave groove angle | degrees | User Input | - | No | 35° to 45° |
| pitch_diameter | Pitch Diameter | Sheave pitch diameter | in | Equation | EQ3 | No | D + 2d |
| rim_diameter | Rim Diameter | Sheave rim diameter | in | Equation | EQ4 | No | D + 2h |
| **Mott Method - Working Load** |
| grade | Wire Rope Grade | Material grade selection | - | Dropdown | Table 7-24 | No | PS, IPS, XIP |
| min_breaking_force | Minimum Breaking Force | Breaking strength of rope | US tons | Table | Table 7-25 | Maybe | From diameter and grade |
| w | Weight per Foot | Weight of rope per foot | lbf/ft | Table | Table 7-25 | Maybe | From diameter and type |
| SF_mott | Service Factor (Mott) | Safety factor for working load | - | User Input | - | No | Minimum 5 for human safety |
| max_working_load | Maximum Working Load | Allowable working load | US tons | Equation | EQ5 | No | Min Breaking Force / SF |
| **Shigley Method - General** |
| material | Wire Material | Material type | - | Table | Table 17-24 | Maybe | Monitor steel, plow steel, etc. |
| d_w | Wire Diameter | Diameter of individual wires | in | Table | Table 17-24/17-27 | Maybe | Size of outer wires |
| E_r | Rope Modulus of Elasticity | Young's modulus of rope | Mpsi | Table | Table 17-24/17-27 | Maybe | ~12-14 Mpsi |
| S_u | Wire Strength | Ultimate tensile strength | kpsi | User Input | - | No | Ranges: 180-280 kpsi |
| A_m | Area of Metal | Cross-sectional metal area | in² | Table | Table 17-27 | Maybe | For some rope types |
| w_core | Weight with Core | Weight per foot including core | lbf/ft | Table | Table 17-27 | Maybe | If core is included |
| **Shigley Method - Loads & Safety** |
| F_u | Ultimate Wire Load | Reduced breaking strength | lbf | Graph | Strength loss graph | No | Based on D/d ratio |
| F_t | Largest Working Tension | Maximum tension in service | lbf | User Input | - | No | Operating load |
| n_shigley | Safety Factor (Shigley) | Static safety factor | - | Equation / Table | EQ6, Table 17-25 | Maybe | n = Fu/Ft, typically 5 |
| P | Bearing Pressure | Contact pressure on sheave | psi | Equation | EQ7 | No | P = 2F/(dD) |
| sheave_material | Sheave Material | Material for sheave | - | Table | Table 17-26 | Maybe | Based on pressure |
| p_Su_ratio | Pressure-Strength Ratio | Fatigue parameter | - | Graph | Fatigue graph | No | From bends-to-failure graph |
| F_f | Fatigue Tension | Allowable fatigue tension | lbf | Equation | EQ8 | No | Ff = (p/Su)SudD/2 |
| F_b | Bending Load | Load due to bending | lbf | Equation | EQ9 | No | Fb = ErdwAm/D |
| n_f | Fatigue Safety Factor | Safety against fatigue | - | Equation | EQ10 | No | nf = (Ff - Fb)/Ft |
| n_s | Static Safety Factor | Safety against static failure | - | Equation | EQ11 | No | ns = (Fu - Fb)/Ft |
| **Mine Hoist - Specific** |
| a | Acceleration/Deceleration | Maximum acceleration | ft/s² | User Input | - | No | For dynamic loading |
| W | Load Weight | Weight at end of rope | lbf | User Input | - | No | Suspended load |
| L | Suspended Length | Maximum rope length | ft | User Input | - | No | Length under load |
| m | Number of Ropes | Ropes supporting load | - | User Input | - | No | Integer value |
| F_t_hoist | Rope Tension (Hoist) | Tension with acceleration | lbf | Equation | EQ12 | No | Ft = (W/m + wL)(1 + a/32.2) |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Mott Method** |
| EQ1 | `h_{drum} = 0.374d` | Drum Groove Depth | d | h_drum | Minimum depth |
| EQ2 | `Drum\ Diameter = D + 2h` | Drum Diameter | D, h_drum | drum_diameter | Overall diameter |
| EQ3 | `Pitch\ Diameter = D + 2d` | Sheave Pitch Diameter | D, d | pitch_diameter | - |
| EQ4 | `Rim\ Diameter = D + 2h` | Sheave Rim Diameter | D, h_sheave | rim_diameter | - |
| EQ5 | `Max\ Working\ Load = \frac{Min\ Breaking\ Force}{SF}` | Maximum Working Load | min_breaking_force, SF_mott | max_working_load | In US tons |
| **Shigley Method** |
| EQ6 | `n = \frac{F_u}{F_t}` | Safety Factor | F_u, F_t | n_shigley | Static safety |
| EQ7 | `P = \frac{2F}{dD}` | Bearing Pressure | F_t, d, D | P | In psi |
| EQ8 | `F_f = \frac{(p/S_u)S_u dD}{2}` | Fatigue Tension | p_Su_ratio, S_u, d, D | F_f | Allowable fatigue |
| EQ9 | `F_b = \frac{E_r d_w A_m}{D}` | Bending Load | E_r, d_w, A_m, D | F_b | Bending component |
| EQ10 | `n_f = \frac{F_f - F_b}{F_t}` | Fatigue Safety Factor | F_f, F_b, F_t | n_f | Against fatigue |
| EQ11 | `n_s = \frac{F_u - F_b}{F_t}` | Static Safety Factor | F_u, F_b, F_t | n_s | Against static failure |
| **Mine Hoist** |
| EQ12 | `F_t = \left(\frac{W}{m} + wL\right)\left(1 + \frac{a}{32.2}\right)` | Rope Tension (Hoist) | W, m, w, L, a | F_t_hoist | With acceleration |

---

## 4. Procedure Steps

### Procedure Table

#### Method 1: Mott Wire Rope Design

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1a | Select Rope Diameter | User Input / Table | Choose d from Table 7-19 | Standard nominal diameters |
| 1b | Determine Rope Type | User Input / Table | If not given, use wires per strand from Table 7-20 | 6×7, 6×19, etc. |
| 2a | Find D/d Ratio | Table | Look up from Table 7-22 based on rope type | Suggested or minimum |
| 2b | Calculate Tread Diameter | Equation | D = Ratio × d (use ratio from step 2a) | - |
| 3a | Calculate Drum Groove Depth | Equation | h_drum = 0.374d (EQ1) | Minimum depth |
| 3b | Calculate Drum Diameter | Equation | Drum Diameter = D + 2h (EQ2) | - |
| 3c | Select Fleet Angle | User Input | 0.5° to 2° | Angle of approach |
| 3d | Find Groove Radius | Table | Look up r from Table 7-23 (use "new") | Based on rope diameter |
| 3e | Determine Pitch Distance | User Input | 2.065r < p < 2.18r | Range constraint |
| 4a | Determine Sheave Groove Depth | User Input | 1.5d ≤ h_sheave ≤ 1.75d | Range constraint |
| 4b | Select Throat Angle | User Input | 35° to 45° | Sheave groove angle |
| 4c | Calculate Pitch Diameter | Equation | Pitch Diameter = D + 2d (EQ3) | - |
| 4d | Calculate Rim Diameter | Equation | Rim Diameter = D + 2h (EQ4) | - |
| 5 | Choose Wire Rope Grade | Dropdown | PS, IPS, or XIP from Table 7-24 | Material grade |
| 6a | Find Minimum Breaking Force | Table | Look up from Table 7-25 based on d and grade | In US tons (2000 lb/ton) |
| 6b | Find Weight per Foot | Table | Look up from Table 7-25 | Optional |
| 6c | Select Service Factor | User Input | Minimum 5 for human safety applications | Higher for critical uses |
| 6d | Calculate Maximum Working Load | Equation | Max Working Load = Min Breaking Force / SF (EQ5) | In US tons |

#### Method 2: Shigley Wire Rope Design

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1a | Select Rope Type | User Input / Table | Choose from tables in PDF | - |
| 1b | Determine Rope Diameter | User Input / Table | Use Table 17-24 or 17-27 | - |
| 1c | Determine Sheave Diameter | User Input / Table | Use minimum or recommended from tables | - |
| 1d | Find Rope Properties | Table | Weight/foot, dw, Am, Er from Table 17-24/17-27 | Material properties |
| 2a | Determine Static Loads | User Input | Sum of all static loads on rope | - |
| 2b | Find Strength Reduction | Graph | Use D/d ratio graph for percent strength loss | % reduction |
| 2c | Calculate Ultimate Wire Load | Equation | Apply reduction to static loads | Fu |
| 2d | Select Safety Factor | User Input / Table | Use Table 17-25 or default 5 | Application-specific |
| 2e | Calculate Working Tension or SF | Equation | n = Fu/Ft (EQ6), solve for unknown | - |
| 3a | Calculate Bearing Pressure | Equation | P = 2F/(dD) (EQ7) | F is tensile force |
| 3b | Select Sheave Material | Table | Use Table 17-26 based on P and rope type | Wood, cast iron, steel, etc. |
| 4a | Input Ultimate Tensile Strength | User Input | Use ranges: IPS: 240-280, PS: 210-240, Mild: 180-210 kpsi | Material property |
| 4b | Find Pressure-Strength Ratio | Graph | Use fatigue graph with bends-to-failure | (p/Su) value ÷ 1000 |
| 4c | Calculate Fatigue Tension | Equation | Ff = (p/Su)SudD/2 (EQ8) | Allowable fatigue |
| 5a | Calculate Bending Load | Equation | Fb = ErdwAm/D (EQ9) | From rope properties |
| 5b | Calculate Fatigue Safety Factor | Equation | nf = (Ff - Fb)/Ft (EQ10) | Against fatigue |
| 5c | Calculate Static Safety Factor | Equation | ns = (Fu - Fb)/Ft (EQ11) | Against static failure |

#### Method 3: Mine Hoist Problems (Shigley)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1a | Input Given Parameters | User Input | a [ft/s²], W [lbf], L [ft], m, rope type | Typical problem inputs |
| 1b | Input or Select Sheave Diameter | User Input / Table | D [in] from Table 17-27 if not given | May use min/recommended |
| 2a | Find Rope Properties | Table | dw, Am, Er, w from Table 17-27 | Based on rope type |
| 2b | Note Properties | Display | Display properties for reference | May be in terms of d |
| 3a | Calculate Rope Tension | Equation | Ft = (W/m + wL)(1 + a/32.2) (EQ12) | With acceleration |
| 3b | Calculate Fatigue Tension | Equation | Ff = (p/Su)SudD/2 (EQ8) | May be in terms of d |
| 3c | Calculate Bending Tension | Equation | Fb = ErdwAm/D (EQ9) | May be in terms of d |
| 4a | Set Up Static Safety Equation | Equation | ns = (Fu - Fb)/Ft (EQ11) | May be in terms of d |
| 4b | Set Up Fatigue Safety Equation | Equation | nf = (Ff - Fb)/Ft (EQ10) | May be in terms of d |
| 4c | Solve for Optimal Diameter | Iteration | Use desired nf to solve for d | Use graphing tool |
| 4d | Select Standard Diameter | Table | Choose closest from Table 7-19 | Round appropriately |

---

## 5. Additional Notes

### Validation Rules
- **Fleet angle**: 0.5° ≤ fleet_angle ≤ 2°
- **Pitch distance**: 2.065r < p < 2.18r
- **Sheave groove depth**: 1.5d ≤ h_sheave ≤ 1.75d
- **Throat angle**: 35° ≤ throat_angle ≤ 45°
- **Service Factor (Mott)**: SF ≥ 5 for human safety
- **Safety Factor (Shigley)**: n ≥ 5 for average operations

### Standard Values / Constants
- **g** = 32.2 ft/s² (gravitational acceleration)
- **US Ton** = 2000 lbf
- **Service Factor minimum** = 5 (for overhead cranes, gantries, hoists)
- **Typical Su ranges**:
  - Improved Plow Steel (IPS): 240-280 kpsi
  - Plow Steel (PS): 210-240 kpsi
  - Mild Plow Steel: 180-210 kpsi

### Dropdown Options

**Wire Rope Grade (Mott):**
- Plow Steel (PS) - 1570 N/mm²
- Improved Plow Steel (IPS) - 1770 N/mm²
- Extra Improved Plow Steel (XIP) - 1960 N/mm²

**Common Rope Types:**
- 6×7 (7-15 wires per strand, 9 max outer wires)
- 6×19 (16-26 wires per strand, 12 max outer wires)
- 6×36 (27-49 wires per strand, 18 max outer wires)
- 6×61 (50-74 wires per strand, 24 max outer wires)
- Also: 6×19 Seale, 6×21 Filler Wire, 6×31 Warrington Seale, etc.

### Future Enhancement Opportunities
- **Table 7-19** (nominal diameters) - dropdown for standard sizes
- **Table 7-20** (rope classification) - interactive lookup
- **Table 7-22** (D/d ratios) - automatic ratio selection
- **Table 7-23** (groove radius) - automatic lookup based on diameter
- **Table 7-24** (wire rope grades) - already implemented as dropdown
- **Table 7-25** (6×19 and 6×36 technical data) - automatic property lookup
- **Table 17-24/17-27** (Shigley rope data) - automatic property lookup
- **Table 17-25** (safety factors) - automatic SF recommendation
- **Table 17-26** (bearing pressures) - automatic material selection
- **Strength loss graph** - digitized for automatic Fu calculation
- **Fatigue graph** - digitized for automatic (p/Su) lookup
- **Mine hoist solver** - iterative d optimization for target safety factor

### Special Considerations
- **Extensive Manual Lookups**: This design requires many table and graph lookups
- **Three Methods**: Mott (simple, working load), Shigley (detailed stress), Mine Hoist (dynamic)
- **Use Mott** for basic drum/sheave sizing and working load calculations
- **Use Shigley** when detailed stress analysis and safety factors are required
- **Use Mine Hoist** when acceleration/deceleration is present
- **Diameter in Mine Hoist**: Often the unknown - solve equations for optimal d
- **IWRC**: Independent Wire Rope Core (vs fiber core)
- **Weight per foot**: Use "No core" values unless specified
- **Conservative values**: Use lower bound Su, higher bends-to-failure for safety
- **Worn vs New**: Groove radius has "new" and "worn" specifications
- **Units**: Mix of inches, feet, lbf, psi, kpsi, Mpsi - careful conversions needed

### Calculation Order / Dependencies

**Mott Method:**
```
[Select: d, rope_type] →
[Table lookup: D/d ratio] →
D = ratio × d →
h_drum = 0.374d →
drum_diameter = D + 2h_drum →
[Table lookup: r from Table 7-23] →
[User input: p in range 2.065r to 2.18r] →
[User input: h_sheave in range 1.5d to 1.75d] →
pitch_diameter = D + 2d →
rim_diameter = D + 2h_sheave →
[Select: grade (PS/IPS/XIP)] →
[Table lookup: min_breaking_force, w from Table 7-25] →
[Input: SF_mott] →
max_working_load = min_breaking_force / SF_mott
```

**Shigley Method:**
```
[Select: rope_type, d, D] →
[Table lookup: dw, Am, Er, w, material] →
[Input: F_t, Su] →
[Graph lookup: strength_loss% from D/d] →
F_u = static_loads × (1 - strength_loss%) →
n = F_u / F_t →
P = 2F_t / (d×D) →
[Table lookup: sheave_material from Table 17-26] →
[Graph lookup: p_Su_ratio from bends-to-failure] →
F_f = (p_Su_ratio) × Su × d × D / 2 →
F_b = Er × dw × Am / D →
n_f = (F_f - F_b) / F_t →
n_s = (F_u - F_b) / F_t
```

**Mine Hoist:**
```
[Input: a, W, L, m, rope_type] →
[Table lookup: dw, Am, Er, w (functions of d)] →
F_t = (W/m + wL)(1 + a/32.2) →
[Input: Su, D, desired_nf] →
[Graph lookup: p_Su_ratio] →
F_f(d) = (p_Su_ratio) × Su × d × D / 2 →
F_b(d) = Er × dw × Am / D →
nf(d) = (F_f(d) - F_b(d)) / F_t →
[Solve: nf(d) = desired_nf for d] →
[Table lookup: closest standard d from Table 7-19]
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (3 methods)
- [x] Piecewise equations explained (N/A)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against PDF
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** Due to extensive table lookups required, this calculator will primarily serve as an equation reference and partial calculator. Full automation would require digitizing many tables and graphs from the PDF.

**Specification Author:** Claude Code
**Date Created:** 2025-10-20
**Last Updated:** 2025-10-20

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
