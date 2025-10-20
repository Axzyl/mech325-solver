# Calculator Specification: Chain Drives Design (Fizz Guide)

---

## 1. Metadata

- **Calculator Name:** Chain Drives Design Calculator (Fizz Guide)
- **Source PDF:** Mech_325_Bible_Fizz-chains.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Fizz-chains.pdf`
- **Description:** Calculates chain drive parameters using the Fizz Guide 11-step methodology, including design power, chain size selection, sprocket geometry, chain length, wrap angles, and factor of safety.
- **Complexity:** Medium-High (extensive table interpolation, iterative selection)
- **Tabs to Include:**
  - [x] Procedure (11-step process)
  - [x] General Equations (11 equations)
  - [x] Variables (25+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 11-step sequential procedure
- **General Equations Tab**: Contains 11 equations that benefit from reference section
- **Variables Tab**: 25+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P<sub>in</sub> | Input Power | Power requirement | hp | User Input | - | No | Starting value |
| n<sub>1</sub> | Small Sprocket Speed | Driving sprocket speed | rpm | User Input | - | No | Faster shaft |
| n<sub>2</sub> | Output Speed | Driven sprocket speed (or range) | rpm | User Input | - | No | Can use midpoint if range |
| **Manual Lookups** |
| Load Type | Load Classification | Application category | - | Dropdown | Table 7-17 | No | Smooth/Moderate/Heavy |
| Driver Type | Prime Mover Type | Motor/engine type | - | Dropdown | Table 7-17 | No | Hydraulic/Electric/IC |
| SF | Service Factor | Load adjustment factor | - | Table | Table 7-17 | No | Based on load and driver |
| Chain Number | Chain Designation | Chain size | - | Dropdown | Table 7-12 | Maybe | 25, 35, 40, 50, 60, 80, etc. |
| p | Chain Pitch | Pitch of chain | in | Table | Table 7-12 | Maybe | From chain number |
| Tensile Strength | Average Tensile Strength | Breaking strength | lbf | Table | Table 7-12 | Maybe | From chain number |
| N<sub>1</sub> | Small Sprocket Teeth | Number of teeth on driver | teeth | User Input | - | No | ≥17, <120/VR |
| Strands | Number of Strands | Multiple strands | - | Dropdown | - | No | 1, 2, 3, or 4 |
| Strand Factor | Multi-Strand Capacity | Power multiplier | - | Constant | - | No | 1.0, 1.7, 2.5, 3.3 |
| P<sub>rated</sub> | Rated Power | Power from table | hp | Table | Tables 7-14/15/16 | Maybe | Requires interpolation |
| Lubrication Type | Lubrication Required | Type A/B/C | - | Table | Tables 7-14/15/16 | Maybe | From same tables |
| **Auto-Calculated** |
| P<sub>des</sub> | Design Power | Adjusted power | hp | Equation | EQ1 | No | P<sub>des</sub> = SF × P<sub>in</sub> |
| VR | Velocity Ratio | Speed ratio | - | Equation | EQ2 | No | VR = n<sub>1</sub>/n<sub>2</sub> |
| P<sub>per strand</sub> | Power per Strand | Design power per strand | hp | Equation | EQ3 | No | P<sub>des</sub>/Strand Factor |
| N<sub>2</sub> | Large Sprocket Teeth | Number of teeth on driven | teeth | Equation | EQ4 | No | Round(N<sub>1</sub> × VR) |
| n<sub>2,actual</sub> | Actual Output Speed | Recalculated speed | rpm | Equation | EQ5 | No | n<sub>1</sub> × (N<sub>1</sub>/N<sub>2</sub>) |
| PD<sub>1</sub> | Small Pitch Diameter | Pitch diameter of driver | in | Equation | EQ6 | No | p/sin(180°/N<sub>1</sub>) |
| PD<sub>2</sub> | Large Pitch Diameter | Pitch diameter of driven | in | Equation | EQ7 | No | p/sin(180°/N<sub>2</sub>) |
| CD<sub>nom</sub> | Nominal Center Distance | Initial center distance | pitches | User Input | - | No | 40 recommended (30-50) |
| L<sub>C</sub> | Chain Length | Length in pitches | pitches | Equation | EQ8 | No | Round to integer |
| CD | Actual Center Distance | Recalculated center | pitches | Equation | EQ9 | No | From rounded L<sub>C</sub> |
| θ<sub>1</sub> | Small Wrap Angle | Contact angle on driver | degrees | Equation | EQ10 | No | Must be ≥ 120° |
| θ<sub>2</sub> | Large Wrap Angle | Contact angle on driven | degrees | Equation | EQ11 | No | Reference only |
| P<sub>allowed</sub> | Allowed Power | Total rated power | hp | Equation | EQ12 | No | P<sub>rated</sub> × Strand Factor |
| FS | Factor of Safety | Safety factor | - | Equation | EQ13 | No | P<sub>allowed</sub>/P<sub>des</sub> |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_{des} = SF \times P_{in}` | Design Power | P<sub>in</sub>, SF | P<sub>des</sub> | - |
| EQ2 | `VR = \frac{n_1}{n_2}` | Velocity Ratio | n<sub>1</sub>, n<sub>2</sub> | VR | n<sub>1</sub> > n<sub>2</sub> |
| EQ3 | `P_{per\ strand} = \frac{P_{des}}{\text{Strand Factor}}` | Power per Strand | P<sub>des</sub>, Strand Factor | P<sub>per strand</sub> | For validation |
| EQ4 | `N_2 = \text{Round}(N_1 \times VR)` | Large Sprocket Teeth | N<sub>1</sub>, VR | N<sub>2</sub> | Round to integer |
| EQ5 | `n_{2,actual} = n_1 \times \frac{N_1}{N_2}` | Actual Output Speed | n<sub>1</sub>, N<sub>1</sub>, N<sub>2</sub> | n<sub>2,actual</sub> | After rounding N<sub>2</sub> |
| EQ6 | `PD_1 = \frac{p}{\sin(180°/N_1)}` | Small Pitch Diameter | p, N<sub>1</sub> | PD<sub>1</sub> | Angle in degrees |
| EQ7 | `PD_2 = \frac{p}{\sin(180°/N_2)}` | Large Pitch Diameter | p, N<sub>2</sub> | PD<sub>2</sub> | Angle in degrees |
| EQ8 | `L_C = 2CD + \frac{N_2+N_1}{2} + \frac{(N_2-N_1)^2}{4\pi^2 CD}` | Chain Length | CD<sub>nom</sub>, N<sub>1</sub>, N<sub>2</sub> | L<sub>C</sub> | Round to integer |
| EQ9 | `CD = \frac{1}{4}\left[L_C - \frac{N_2+N_1}{2} + \sqrt{\left(L_C - \frac{N_2+N_1}{2}\right)^2 - \frac{8(N_2-N_1)^2}{4\pi^2}}\right]` | Actual Center Distance | L<sub>C</sub>, N<sub>1</sub>, N<sub>2</sub> | CD | Use rounded L<sub>C</sub> |
| EQ10 | `\theta_1 = 180° - 2\sin^{-1}\left[\frac{PD_2-PD_1}{2 \cdot CD}\right]` | Small Wrap Angle | PD<sub>1</sub>, PD<sub>2</sub>, CD | θ<sub>1</sub> | Result in degrees, CD in pitches |
| EQ11 | `\theta_2 = 180° + 2\sin^{-1}\left[\frac{PD_2-PD_1}{2 \cdot CD}\right]` | Large Wrap Angle | PD<sub>1</sub>, PD<sub>2</sub>, CD | θ<sub>2</sub> | Result in degrees |
| EQ12 | `P_{allowed} = P_{rated} \times \text{Strand Factor}` | Allowed Power | P<sub>rated</sub>, Strand Factor | P<sub>allowed</sub> | Total capacity |
| EQ13 | `FS = \frac{P_{allowed}}{P_{des}}` | Factor of Safety | P<sub>allowed</sub>, P<sub>des</sub> | FS | Should be > 1.0 |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1a | Input Parameters | User Input | Enter P<sub>in</sub> [hp], n<sub>1</sub> [rpm], n<sub>2</sub> [rpm or range] | If range, use midpoint |
| 1b | Calculate Velocity Ratio | Equation | VR = n<sub>1</sub>/n<sub>2</sub> (EQ2) | Auto-calculated |
| 2a | Select Load and Driver Type | Dropdown | Choose from Table 7-17 categories | 9 combinations |
| 2b | Find Service Factor | Table | Look up SF from Table 7-17 | Based on selections |
| 2c | Calculate Design Power | Equation | P<sub>des</sub> = SF × P<sub>in</sub> (EQ1) | Auto-calculated |
| 3a | Select Number of Strands | Dropdown | Choose 1, 2, 3, or 4 strands | Start with 1 |
| 3b | Select Chain Number | Table | Choose from Table 7-12 | Affects pitch and strength |
| 3c | Get Chain Properties | Table | Extract p, tensile strength from Table 7-12 | Yellow inputs |
| 3d | Select Small Sprocket Teeth | User Input | Choose N<sub>1</sub> ≥ 17, N<sub>1</sub> × VR < 120 | Constraint validation |
| 3e | Find Rated Power | Table | Look up P<sub>rated</sub> from Tables 7-14/15/16 | Requires interpolation |
| 3f | Validate Power | Validation | Check P<sub>rated</sub> > P<sub>des</sub> | If fails, adjust chain/teeth/strands |
| 4 | Calculate Large Sprocket Teeth | Equation | N<sub>2</sub> = Round(N<sub>1</sub> × VR) (EQ4) | Auto-calculated |
| 5 | Calculate Actual Output Speed | Equation | n<sub>2,actual</sub> = n<sub>1</sub> × (N<sub>1</sub>/N<sub>2</sub>) (EQ5) | Verify in acceptable range |
| 6a | Calculate Small Pitch Diameter | Equation | PD<sub>1</sub> = p/sin(180°/N<sub>1</sub>) (EQ6) | Auto-calculated |
| 6b | Calculate Large Pitch Diameter | Equation | PD<sub>2</sub> = p/sin(180°/N<sub>2</sub>) (EQ7) | Auto-calculated |
| 7 | Input Nominal Center Distance | User Input | Enter CD<sub>nom</sub> in pitches | 40 recommended (30-50) |
| 8a | Calculate Chain Length | Equation | L<sub>C</sub> using EQ8 | Auto-calculated |
| 8b | Round Chain Length | Rounding | Round L<sub>C</sub> to nearest integer | Manual or auto |
| 9 | Recalculate Center Distance | Equation | CD using EQ9 with rounded L<sub>C</sub> | Auto-calculated |
| 10a | Calculate Small Wrap Angle | Equation | θ<sub>1</sub> using EQ10 (EQ10) | Auto-calculated |
| 10b | Calculate Large Wrap Angle | Equation | θ<sub>2</sub> using EQ11 (EQ11) | Auto-calculated |
| 10c | Validate Wrap Angle | Validation | Check θ<sub>1</sub> ≥ 120° | If fails, increase CD |
| 11a | Calculate Allowed Power | Equation | P<sub>allowed</sub> = P<sub>rated</sub> × Strand Factor (EQ12) | Auto-calculated |
| 11b | Calculate Factor of Safety | Equation | FS = P<sub>allowed</sub>/P<sub>des</sub> (EQ13) | Final result |

---

## 5. Additional Notes

### Validation Rules
- **Minimum Teeth**: N<sub>1</sub> ≥ 17 (except if n<sub>1</sub> < 100 rpm)
- **Maximum Teeth**: N<sub>2</sub> ≤ 120 (enforced via N<sub>1</sub> × VR < 120)
- **Integer Values**: N<sub>2</sub>, L<sub>C</sub> must be integers
- **Power Adequacy**: P<sub>rated</sub> > P<sub>des</sub>
- **Center Distance Range**: 30 ≤ CD<sub>nom</sub> ≤ 50 pitches (recommended)
- **Minimum Wrap Angle**: θ<sub>1</sub> ≥ 120°
- **Velocity Ratio Convention**: n<sub>1</sub> > n<sub>2</sub>
- **Tensile Load**: If chain supports static load, use only 10% of average tensile strength

### Standard Values / Constants
- **Recommended CD<sub>nom</sub>**: 40 pitches
- **Strand Factors**: 1 strand = 1.0, 2 strands = 1.7, 3 strands = 2.5, 4 strands = 3.3
- **π**: Use 3.14159 or Math.PI

### Dropdown Options

**Load Type:**
- Smooth (Agitators, fans, generators, centrifugal pumps, light conveyors)
- Moderate Shock (Bucket elevators, machine tools, cranes, heavy conveyors, mixers)
- Heavy Shock (Punch presses, crushers, logging hoists, rolling mills)

**Driver Type:**
- Hydraulic drive
- Electric motor or turbine
- Internal combustion engine with mechanical drive

**Service Factor (SF) - Table 7-17:**

| Load Type | Hydraulic | Electric | IC Engine |
|-----------|-----------|----------|-----------|
| Smooth | 1.0 | 1.0 | 1.2 |
| Moderate Shock | 1.2 | 1.3 | 1.4 |
| Heavy Shock | 1.4 | 1.5 | 1.7 |

**Chain Numbers (from Table 7-12):**
- 25 (p = 0.25 in)
- 35 (p = 0.375 in)
- 40 (p = 0.5 in) - Most common
- 41 (p = 0.5 in)
- 50 (p = 0.625 in)
- 60 (p = 0.75 in) - Very reliable
- 80 (p = 1.0 in)
- 100, 120, 140, 160, 180, 200, 240 (larger sizes)

**Number of Strands:**
- 1 strand (factor: 1.0)
- 2 strands (factor: 1.7)
- 3 strands (factor: 2.5)
- 4 strands (factor: 3.3)

### Future Enhancement Opportunities
- **Table 7-12**: Implement dropdown with automatic pitch and strength lookup
- **Table 7-17**: Already implemented as dropdowns
- **Tables 7-14/15/16**: Could implement 2D interpolation for automatic P<sub>rated</sub> calculation
- **Lubrication Type**: Could auto-determine based on speed zones in rating tables
- **Chain Comparison**: Show multiple chain options that meet requirements
- **Optimization**: Suggest optimal N<sub>1</sub> and strands for minimum cost/weight

### Special Considerations
- **Power Rating Interpolation**: Tables 7-14/15/16 require 2D interpolation (teeth × speed). This is the most complex part of the calculator.
- **Iterative Step 3**: User may need to try different chain numbers, teeth counts, or strand configurations to satisfy P<sub>rated</sub> > P<sub>des</sub>
- **Integer Rounding**: Both N<sub>2</sub> and L<sub>C</sub> must be rounded, which affects downstream calculations
- **Wrap Angle Validation**: If θ<sub>1</sub> < 120°, user must increase center distance or adjust sprocket sizes
- **Lubrication Zones**: Tables 7-14/15/16 show Type A, B, C lubrication zones based on speed
- **Tensile vs Power**: Document notes that for tensile loading (not power transmission), use only 10% of average tensile strength

### Calculation Order / Dependencies

```
P_in, n1, n2 (user) →
VR = n1/n2 →
Load Type, Driver Type (dropdowns) →
SF (Table 7-17) →
P_des = SF × P_in →
Strands (dropdown) →
Chain Number (Table 7-12) →
p, tensile strength (Table 7-12) →
N1 (user, with constraints) →
P_rated (Tables 7-14/15/16 - interpolation) →
Validate: P_rated > P_des →
N2 = Round(N1 × VR) →
n2_actual = n1 × (N1/N2) →
PD1 = p/sin(180°/N1) →
PD2 = p/sin(180°/N2) →
CD_nom (user, 40 recommended) →
LC = 2CD + (N2+N1)/2 + (N2-N1)²/(4π²CD) →
Round LC to integer →
CD = recalculate from rounded LC →
θ1 = 180° - 2sin⁻¹[(PD2-PD1)/(2CD)] →
θ2 = 180° + 2sin⁻¹[(PD2-PD1)/(2CD)] →
Validate: θ1 ≥ 120° →
P_allowed = P_rated × Strand Factor →
FS = P_allowed / P_des
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (N/A)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against PDF
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has medium-high complexity due to power rating table interpolation requirements. Tables 7-14/15/16 require 2D interpolation (teeth × speed). Manual lookups with info boxes are recommended unless full table digitization is performed.

**Specification Author:** Claude Code
**Date Created:** 2025-10-20
**Last Updated:** 2025-10-20

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
