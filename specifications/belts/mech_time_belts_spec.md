# Calculator Specification: Timing Belt Design (Mott Method)

---

## 1. Metadata

- **Calculator Name:** Timing Belt Design Calculator (Mott Method)
- **Source PDF:** Mech_325_Bible_Mech-time_belts.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Mech-time_belts.pdf`
- **Description:** Calculates timing belt design parameters including design power, belt pitch selection, sprocket teeth combinations, belt width, and validates speed constraints following Mott's 9-step methodology.
- **Complexity:** Medium-High (simple equations, extensive table lookups, iterative validation)
- **Tabs to Include:**
  - [x] Procedure (9-step process)
  - [x] General Equations (4 equations + validations)
  - [x] Variables (20+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 9-step numbered procedure with decision points
- **General Equations Tab**: Contains equations and validation constraints that need reference
- **Variables Tab**: 20+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| H<sub>nom</sub> | Nominal Horsepower | Input power requirement | hp | User Input | - | No | Starting value |
| n | Input Speed | Driving sprocket speed | rpm | User Input | - | No | Speed of faster shaft |
| VR | Velocity Ratio | Speed ratio | - | User Input | - | No | VR = n<sub>driving</sub>/n<sub>driven</sub> |
| CD | Center Distance | Nominal center distance | in | User Input | - | No | Often given with tolerance |
| D<sub>max</sub> | Max Sprocket Diameter | Design constraint for sprocket size | in | User Input | - | No | Optional constraint |
| d<sub>shaft,driver</sub> | Driver Shaft Diameter | Driving shaft bore requirement | in | User Input | - | No | Optional for validation |
| d<sub>shaft,driven</sub> | Driven Shaft Diameter | Driven shaft bore requirement | in | User Input | - | No | Optional for validation |
| **Manual Lookups** |
| SF | Service Factor | Load type adjustment factor | - | Table | Table 7-8 | No | Based on machine type and motor |
| pitch | Belt Pitch | Belt pitch selection | mm | Graph | GT Belt Pitch Chart | No | 3mm, 8mm, 14mm, or 20mm |
| N<sub>d</sub> | Driver Sprocket Teeth | Number of teeth on driver | teeth | Table | Appendix A, Table 7-4 | Maybe | From VR tables and sprocket specs |
| N<sub>D</sub> | Driven Sprocket Teeth | Number of teeth on driven | teeth | Table | Appendix A | Maybe | Based on VR = N<sub>D</sub>/N<sub>d</sub> |
| PD<sub>d</sub> | Driver Pitch Diameter | Pitch diameter of driver | in | Table | Table 7-4 | Maybe | From sprocket specifications |
| PD<sub>D</sub> | Driven Pitch Diameter | Pitch diameter of driven | in | Table | Table 7-4 | Maybe | From sprocket specifications |
| bushing_style_d | Driver Bushing Style | Bushing type for driver | - | Table | Table 7-4 | Maybe | From sprocket table |
| bushing_style_D | Driven Bushing Style | Bushing type for driven | - | Table | Table 7-4 | Maybe | From sprocket table |
| bore<sub>min,d</sub> | Driver Min Bore | Minimum bore for driver bushing | in | Table | Table 7-5 | Maybe | From bushing table |
| bore<sub>max,d</sub> | Driver Max Bore | Maximum bore for driver bushing | in | Table | Table 7-5 | Maybe | From bushing table |
| bore<sub>min,D</sub> | Driven Min Bore | Minimum bore for driven bushing | in | Table | Table 7-5 | Maybe | From bushing table |
| bore<sub>max,D</sub> | Driven Max Bore | Maximum bore for driven bushing | in | Table | Table 7-5 | Maybe | From bushing table |
| belt_designation | Belt Designation | Belt part number | - | Table | Appendix A | Maybe | Specific belt model |
| belt_length | Belt Length | Physical belt length | mm or in | Table | Appendix A | Maybe | From belt selection table |
| C<sub>L</sub> | Length Correction Factor | Correction for belt length | - | Table | Table 7-11 | Maybe | Based on designation and teeth |
| P<sub>rated</sub> | Rated Power | Base power rating | hp | Table | Table 7-9 or 7-10 | Maybe | Based on N<sub>d</sub>, n, width |
| belt_width | Belt Width | Belt width selection | mm | Dropdown | - | No | 30mm or 50mm (for 8mm pitch) |
| **Auto-Calculated** |
| H<sub>d</sub> | Design Power | Adjusted power for design | hp | Equation | EQ1 | No | H<sub>d</sub> = H<sub>nom</sub> × SF |
| P<sub>adjusted</sub> | Adjusted Power | Length-corrected power rating | hp | Equation | EQ2 | No | P<sub>adjusted</sub> = P<sub>rated</sub> × C<sub>L</sub> |
| V | Belt Speed | Linear belt velocity | fpm | Equation | EQ3 | No | V = (π × n × PD<sub>d</sub>)/12 |
| VR<sub>actual</sub> | Actual Velocity Ratio | Calculated from selected teeth | - | Equation | EQ4 | No | VR<sub>actual</sub> = N<sub>D</sub>/N<sub>d</sub> |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `H_d = H_{nom} \times SF` | Design Power | H<sub>nom</sub>, SF | H<sub>d</sub> | - |
| EQ2 | `P_{adjusted} = P_{rated} \times C_L` | Adjusted Power | P<sub>rated</sub>, C<sub>L</sub> | P<sub>adjusted</sub> | Length correction |
| EQ3 | `V = \frac{\pi n PD_d}{12}` | Belt Speed | n, PD<sub>d</sub> | V | Result in fpm |
| EQ4 | `VR_{actual} = \frac{N_D}{N_d}` | Actual Velocity Ratio | N<sub>D</sub>, N<sub>d</sub> | VR<sub>actual</sub> | Should match target VR |
| VAL1 | `\text{bore}_{min} < d_{shaft} < \text{bore}_{max}` | Bore Compatibility | bore<sub>min</sub>, bore<sub>max</sub>, d<sub>shaft</sub> | Pass/Fail | Check both sprockets |
| VAL2 | `PD \leq D_{max}` | Diameter Limit | PD<sub>d</sub>, PD<sub>D</sub>, D<sub>max</sub> | Pass/Fail | Optional constraint |
| VAL3 | `P_{adjusted} > H_d` | Power Adequacy | P<sub>adjusted</sub>, H<sub>d</sub> | Pass/Fail | Should be well above |
| VAL4 | `V < 6500` | Speed Limit | V | Pass/Fail | Safety constraint (fpm) |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Receive Design Specifications | User Input | Enter H<sub>nom</sub> [hp], n [rpm], VR, CD [in] | Optional: D<sub>max</sub>, d<sub>shaft</sub> |
| 2a | Select Service Factor | Table | Look up SF from Table 7-8 based on machine type, motor type, service | Dropdown implementation recommended |
| 2b | Calculate Design Power | Equation | H<sub>d</sub> = H<sub>nom</sub> × SF (EQ1) | Auto-calculated |
| 3 | Select Belt Pitch | Graph | Use GT Belt Pitch Selection Chart with H<sub>d</sub> and n | Yellow input: pitch (3, 8, 14, or 20 mm); 8mm most common |
| 4a | Choose Sprocket Combinations | Table | From Appendix A, find valid N<sub>d</sub> and N<sub>D</sub> for VR | Multiple candidates possible |
| 4b | Validate Velocity Ratio | Equation | VR<sub>actual</sub> = N<sub>D</sub>/N<sub>d</sub> (EQ4), compare to target VR | Select closest match |
| 5a | Look Up Sprocket Specifications | Table | From Table 7-4, get PD<sub>d</sub>, PD<sub>D</sub>, bushing styles | Based on N<sub>d</sub>, N<sub>D</sub>, belt_width |
| 5b | Validate Bore Compatibility | Table + Validation | From Table 7-5, check bore<sub>min</sub> < d<sub>shaft</sub> < bore<sub>max</sub> (VAL1) | Check both driver and driven |
| 5c | Validate Diameter Constraints | Validation | If D<sub>max</sub> given, check PD<sub>d</sub>, PD<sub>D</sub> ≤ D<sub>max</sub> (VAL2) | Optional check |
| 6 | Select Belt Designation | Table | From Appendix A, select belt based on sprockets and CD | Yellow input: belt_designation, belt_length |
| 7a | Choose Belt Width | Dropdown | Select 30mm or 50mm (for 8mm pitch) | Start with 50mm for conservatism |
| 7b | Find Length Correction Factor | Table | From Table 7-11, look up C<sub>L</sub> based on belt_designation | Yellow input |
| 7c | Find Rated Power | Table | From Table 7-9 (30mm) or 7-10 (50mm), look up P<sub>rated</sub> using N<sub>d</sub> and n | Yellow input, may require interpolation |
| 7d | Calculate Adjusted Power | Equation | P<sub>adjusted</sub> = P<sub>rated</sub> × C<sub>L</sub> (EQ2) | Auto-calculated |
| 7e | Validate Power Adequacy | Validation | Check P<sub>adjusted</sub> >> H<sub>d</sub> (VAL3) | If fails, try 50mm width |
| 8a | Calculate Belt Speed | Equation | V = (π × n × PD<sub>d</sub>)/12 (EQ3) | Auto-calculated |
| 8b | Validate Speed Limit | Validation | Check V < 6500 fpm (VAL4) | Safety constraint |
| 9 | Display Final Specification | Display | Show all selected parameters and validation results | Summary output |

---

## 5. Additional Notes

### Validation Rules
- **Bore Compatibility** (VAL1): bore<sub>min</sub> < d<sub>shaft</sub> < bore<sub>max</sub> for both driver and driven
- **Diameter Limit** (VAL2): PD<sub>d</sub>, PD<sub>D</sub> ≤ D<sub>max</sub> (if specified)
- **Power Adequacy** (VAL3): P<sub>adjusted</sub> should be "well above" H<sub>d</sub> (suggest 20-30% margin)
- **Speed Limit** (VAL4): V < 6500 fpm for belt material safety
- **Velocity Ratio Matching**: VR<sub>actual</sub> should closely match target VR (exact integer teeth)

### Standard Values / Constants
- **Belt Pitch Options**: 3mm, 8mm, 14mm, 20mm (8mm most common)
- **Belt Width Options** (for 8mm pitch): 30mm, 50mm
- **Speed Limit**: 6500 fpm maximum
- **π**: Use 3.14159 or Math.PI

### Dropdown Options

**Belt Pitch:**
- 3 mm
- 8 mm (most common - per author's note)
- 14 mm
- 20 mm

**Belt Width** (for 8mm pitch):
- 30 mm
- 50 mm (recommended starting point - per author's note)

**Service Factor (SF) - Table 7-8** (Hierarchical structure):

**Structure**: [Machine Type] → [Motor Type] → [Service Duration] → SF Value

Example categories:
- **Machine Type**: Light-duty (instrumentation), Medium-duty (conveyors), Heavy-duty (compressors), etc.
- **Motor Type**: AC motor (normal torque), AC motor (high torque), DC motor, Internal combustion
- **Service Duration**: Intermittent (up to normal hours), 8-16 hrs/day, 16-24 hrs/day (continuous)
- **SF Range**: 1.0 to 2.5

*Note*: Full table has 8+ machine categories × 4 motor types × 3 service durations = 96+ combinations. Recommend simplified dropdown or hierarchical selection.

### Future Enhancement Opportunities
- **Appendix A Tables**: Could digitize VR tables for automatic sprocket combination suggestion
- **Table 7-4**: Could create database lookup for sprocket specs based on teeth count and width
- **Table 7-5**: Could automate bushing bore range lookup
- **GT Belt Pitch Chart**: Could digitize graph for automatic pitch selection based on H<sub>d</sub> and n
- **Table 7-11**: Could automate length correction factor lookup
- **Tables 7-9 and 7-10**: Could implement bilinear interpolation for non-standard RPM values
- **Multi-Solution Display**: Show all valid sprocket combinations and let user select best option

### Special Considerations
- **Appendix A**: Not included in main PDF, contains critical VR and belt selection tables - must reference separately
- **Interpolation Required**: Power rating tables (7-9, 7-10) require interpolation for non-standard RPM values
- **Multiple Valid Solutions**: Step 4 may yield several valid sprocket combinations; calculator should facilitate comparison
- **Iterative Width Selection**: If 30mm belt inadequate (P<sub>adjusted</sub> ≤ H<sub>d</sub>), must retry with 50mm
- **Service Factor Complexity**: Table 7-8 is hierarchical; dropdown implementation should guide user through machine type → motor type → service duration
- **Conservative Design**: Author recommends starting with 50mm width and validating down to 30mm if desired

### Calculation Order / Dependencies

```
H_nom, n, VR, CD (user) →
SF (Table 7-8 dropdown) →
Hd = Hnom × SF →
pitch (GT Chart lookup - Graph) →
[Nd, ND] (Appendix A - VR tables) →
VR_actual = ND/Nd (verify match) →
[PDd, PDD, bushing styles] (Table 7-4) →
[bore_min, bore_max] (Table 7-5) →
Validate bore compatibility (VAL1) →
Validate diameter limits (VAL2) →
belt_designation, belt_length (Appendix A) →
belt_width (dropdown: 30mm or 50mm) →
CL (Table 7-11) →
P_rated (Table 7-9 or 7-10) →
P_adjusted = P_rated × CL →
Validate power adequacy (VAL3) →
V = (π × n × PDd)/12 →
Validate speed limit (VAL4) →
Display final specification
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (N/A - no piecewise equations)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against PDF
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator requires extensive table lookups, particularly from Appendix A (not included in main PDF). Manual lookups with info boxes are the primary implementation strategy. Future enhancements could digitize tables for automatic selection.

**Specification Author:** Claude Code
**Date Created:** 2025-10-20
**Last Updated:** 2025-10-20

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
