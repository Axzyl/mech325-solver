# Calculator Specification: Boundary-Lubricated Bearings (Bushings)

---

## 1. Metadata

- **Calculator Name:** Boundary-Lubricated Bearings (Bushings) Selection and Verification
- **Source PDF:** fizz_blb.pdf
- **PDF Location:** `documents/bearings/fizz_blb.pdf`
- **Description:** Complete bushing selection calculator for boundary-lubricated bearings. Calculates minimum bearing length based on thermal considerations, assists with bushing selection from standard sizes, and verifies design against service range limits (characteristic pressure, nominal pressure, velocity, PV product, and linear wear). Primarily focused on Oiles 500 SP type bushings.
- **Complexity:** Medium
- **Tabs to Include:**
  - [x] Procedure (Clear 11-step selection and verification process)
  - [x] General Equations (8 equations for sizing and verification)
  - [x] Variables (Essential for ~25 variables)

**Rationale for Tab Structure:**
The PDF contains a straightforward 11-step procedure for bushing selection and verification, 8 equations, and requires 4 table lookups. Three tabs provide clear organization: Procedure guides the workflow, General Equations allows quick reference, and Variables manages the ~25 variables including multiple table lookups.

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Geometry - Basic** | | | | | | | |
| D<sub>b</sub> | Bearing Diameter | Outer diameter of journal/shaft space | in | User Input | Page 1, diagram | No | Same as OD of journal |
| D<sub>j</sub> | Journal Diameter | Diameter of rotating shaft | in | User Input | Page 1, diagram | No | Shaft diameter |
| C<sub>r</sub> | Radial Clearance | Radial gap between surfaces | in | Equation | EQ1 | No | Cr = (Db - Dj) / 2 |
| C<sub>d</sub> | Diametral Clearance | Diametrical gap | in | Equation | EQ2 | No | Cd = Db - Dj = 2Cr |
| **Bushing Dimensions** | | | | | | | |
| ID | Inner Diameter | Inside diameter of bushing | in | Table | Table 12-12 | Maybe | Must be > Dj (shaft diameter) |
| OD | Outer Diameter | Outside diameter of bushing | in | Table | Table 12-12 | Maybe | From standard sizes |
| D | Bearing Diameter (alias) | Same as ID | in | Equation | = ID | No | Used in equations |
| L | Bearing Length | Axial length of bushing | in | Table/Equation | EQ-L, Table 12-12 | Maybe | Must meet L ≥ Lmin |
| L<sub>min</sub> | Minimum Length | Calculated minimum length | in | Equation | EQ-L | No | From thermal analysis |
| **Loading and Speed** | | | | | | | |
| F | Radial Load | Radial force on bearing | lbf | User Input | Page 2 | No | Total radial load |
| N | Angular Speed | Rotational speed | rpm | User Input | Page 2 | No | Bearing speed |
| n<sub>d</sub> | Design Factor | Safety/design factor | - | User Input | Page 2 | No | Typically 1.0-2.0 |
| **Temperature** | | | | | | | |
| T<sub>f</sub> | Lubricant Temperature | Max operating temperature | °F | User Input | Page 2 | No | Max temperature limit |
| T<sub>∞</sub> | Ambient Temperature | Surrounding air temperature | °F | User Input | Page 2 | No | Default 70°F if not given |
| **Material Properties** | | | | | | | |
| material | Bushing Material | Type of bearing material | - | Dropdown | Table 12-9 | No | Oiles 800, 500, Polyactal, etc. |
| bearing_type | Bearing Type | Classification for friction | - | Dropdown | Table 12-10 | No | Placetic, Composite, Met |
| K | Wear Factor | Material wear coefficient | in³·min/(lbf·ft·h) | Table | Table 12-9 | Maybe | Based on material |
| PV<sub>limit</sub> | Limiting PV | Maximum PV product allowed | psi·ft/min | Table | Table 12-9 | Maybe | Based on material |
| f<sub>s</sub> | Coefficient of Friction | Friction coefficient | - | Table | Table 12-10 | Maybe | Based on bearing_type |
| **Constants** | | | | | | | |
| ℏC<sub>R</sub> | Heat Capacity Constant | Thermal constant | Btu/(lbf·°F) | User Input | Page 2 | No | Default 2.7 unless given |
| J | Mechanical Equivalent | Conversion constant | ft·lbf/Btu | User Input | Page 2 | No | Default 778 unless given |
| **Calculated Performance** | | | | | | | |
| P<sub>max</sub> | Characteristic Pressure | Maximum bearing pressure | psi | Equation | EQ-PMAX | No | Must be < 3560 psi (Oiles 500 SP) |
| P | Nominal Pressure | Average bearing pressure | psi | Equation | EQ-P | No | - |
| V | Velocity | Surface velocity | ft/min | Equation | EQ-V | No | Must be < Vmax |
| PV | PV Product | Pressure-velocity product | psi·ft/min | Equation | EQ-PV | No | Must be < PVlimit |
| V<sub>max</sub> | Maximum Velocity | Velocity limit | ft/min | Table/Constant | Table 12-11 | No | 100 ft/min for Oiles 500 SP |
| **Wear Analysis** | | | | | | | |
| t | Operating Time | Duration of operation | hours | User Input | Page 4, Step 10 | No | Optional, for wear calc |
| w | Linear Wear | Radial wear depth | in | Equation | EQ-W | No | Optional verification |
| w<sub>max</sub> | Maximum Wear | Allowable wear | in | User Input | Page 4, Step 10 | No | Optional, if given in problem |
| **Validation Checks** | | | | | | | |
| ratio_LD | Length-to-Diameter Ratio | L/D ratio | - | Equation | EQ-RATIO | No | Must be 0.5 ≤ L/D ≤ 2.0 |

**Notes:**
- Default values: T∞ = 70°F, ℏCR = 2.7, J = 778
- Table 12-12 (Available Bushing Sizes) is large with many ID/OD/Length combinations
- Limits for Oiles 500 SP: Pmax < 3560 psi, Vmax < 100 ft/min, PV < 46,700 psi·ft/min, T < 300°F
- nd in the formulas appears to be the design factor (not the speed N)

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Geometry** | | | | | |
| EQ1 | `C_r = \frac{D_b - D_j}{2}` | Radial clearance | Db, Dj | Cr | - |
| EQ2 | `C_d = D_b - D_j = 2C_r` | Diametral clearance | Db, Dj (or Cr) | Cd | - |
| **Sizing** | | | | | |
| EQ-L | `L \geq \frac{720 f_s n_d F N}{J \hbar C_R (T_f - T_\infty)}` | Minimum bearing length | fs, nd, F, N, J, ℏCR, Tf, T∞ | Lmin | Thermal constraint |
| EQ-RATIO | `\text{ratio}_{LD} = \frac{L}{D}` | Length-to-diameter ratio | L, D | ratio_LD | Must be 0.5 ≤ ratio ≤ 2.0 |
| **Performance Verification** | | | | | |
| EQ-PMAX | `P_{max} = \frac{4}{\pi} \frac{n_d F}{DL}` | Characteristic pressure | nd, F, D, L | Pmax | Must be < 3560 psi for Oiles 500 SP |
| EQ-P | `P = \frac{n_d F}{DL}` | Nominal pressure | nd, F, D, L | P | - |
| EQ-V | `V = \frac{\pi DN}{12}` | Surface velocity | D, N | V | Must be < 100 ft/min for Oiles 500 SP |
| EQ-PV | `PV = P \times V` | PV product | P, V | PV | Must be < 46,700 psi·ft/min for Oiles 500 SP |
| **Wear** | | | | | |
| EQ-W | `w = \frac{K n_d F N t}{3L}` | Linear wear | K, nd, F, N, t, L | w | Optional, if wmax given |

**Notes:**
- EQ-L uses default values J = 778 ft·lbf/Btu and ℏCR = 2.7 Btu/(lbf·°F) unless specified
- EQ-PMAX has factor of 4/π compared to EQ-P
- All pressure/velocity limits are for Oiles 500 SP material; other materials have different limits from Table 12-9

---

## 4. Procedure Steps

### 4.1.2 Design Selection

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Initial Inputs** | | | | |
| 0-1 | Input design requirements | User Input | Enter F, N, Tf (or use default T∞ = 70°F) | Starting parameters |
| 0-2 | Input design factor | User Input | Enter nd (typically 1.0-2.0) | Safety margin |
| 0-3 | Input shaft diameter if given | User Input | Enter Dj if specified | Optional constraint |
| **Step 1** | **Get material properties from tables** | | | |
| 1-1 | Select bushing material | Dropdown | Choose from Table 12-9 | Oiles 800, 500, Polyactal, etc. |
| 1-2 | Get wear factor K | Table Lookup | Table 12-9 based on material | Yellow input |
| 1-3 | Get limiting PV | Table Lookup | Table 12-9 based on material | Yellow input |
| 1-4 | Select bearing type for friction | Dropdown | Choose from Table 12-10 | Placetic, Composite, Met |
| 1-5 | Get coefficient of friction fs | Table Lookup | Table 12-10 based on bearing_type | Yellow input |
| **Step 2** | **Calculate minimum bearing length** | | | |
| 2-1 | Set constants if not given | User Input/Default | J = 778, ℏCR = 2.7, T∞ = 70°F | Use defaults unless specified |
| 2-2 | Calculate Lmin | Equation | Use EQ-L: Lmin = 720 fs nd F N / [J ℏCR (Tf - T∞)] | - |
| **Step 3** | **Select bushing from standard sizes** | | | |
| 3-1 | Choose bushing from table | Table Lookup | Table 12-12: select ID, OD, L | Yellow input, multiple options |
| 3-2 | Ensure L ≥ Lmin | Validation | Check selected L against Lmin | If fails, choose larger L |
| 3-3 | If shaft diameter given, check ID > Dj | Validation | Ensure ID > Dj | If fails, choose larger ID |
| 3-4 | Set D = ID | Equation | D is same as inner diameter | For calculations |
| **Step 4** | **Check L/D ratio** | | | |
| 4-1 | Calculate L/D ratio | Equation | ratio_LD = L / D | - |
| 4-2 | Verify 0.5 ≤ L/D ≤ 2.0 | Validation | Check if ratio in range | If fails, choose different bushing |
| **Step 5** | **Introduction to verification** | | | |
| 5-0 | Note verification for Oiles 500 SP | Information | Following checks assume Oiles 500 SP | Other materials use Table 12-9 limits |
| **Step 6** | **Calculate and check characteristic pressure** | | | |
| 6-1 | Calculate Pmax | Equation | Use EQ-PMAX: Pmax = (4/π) nd F / (DL) | - |
| 6-2 | Verify Pmax < 3560 psi | Validation | Check against Oiles 500 SP limit | Warning if exceeds |
| **Step 7** | **Calculate nominal pressure** | | | |
| 7-1 | Calculate P | Equation | Use EQ-P: P = nd F / (DL) | - |
| **Step 8** | **Calculate and check velocity** | | | |
| 8-1 | Calculate V | Equation | Use EQ-V: V = π D N / 12 | - |
| 8-2 | Verify V < 100 ft/min | Validation | Check against Vmax for Oiles 500 SP | Warning if exceeds |
| **Step 9** | **Calculate and check PV product** | | | |
| 9-1 | Calculate PV | Equation | Use EQ-PV: PV = P × V | - |
| 9-2 | Verify PV < 46,700 psi·ft/min | Validation | Check against limit for Oiles 500 SP | Warning if exceeds |
| **Step 10** | **Calculate wear if required** | | | |
| 10-1 | Check if wear analysis needed | Conditional | Is wmax given in problem? | Optional step |
| 10-2 | If yes, input operating time t | User Input | Enter t in hours | Only if wear check required |
| 10-3 | Calculate linear wear w | Equation | Use EQ-W: w = K nd F N t / (3L) | - |
| 10-4 | Verify w < wmax | Validation | Check against specified limit | Warning if exceeds |
| **Step 11** | **Final verification** | | | |
| 11-1 | Check all validations passed | Summary | All limits satisfied? | Green if pass, red if fail |
| 11-2 | Display selected bushing | Output | Show ID, OD, L, material | - |
| 11-3 | Display calculated values | Output | Show Pmax, P, V, PV, w (if applicable) | - |
| 11-4 | Display success message | Output | "Acceptable bushing selected!" | If all checks pass |

**Notes:**
- Steps 1-4 are sizing/selection
- Steps 5-10 are verification
- Step 10 is optional (only if maximum wear is specified)
- If any validation fails, user must go back to Step 3 and select different bushing
- Default values: T∞ = 70°F, J = 778, ℏCR = 2.7

---

## 5. Additional Notes

### Piecewise Equations

None - all equations are straightforward. However, verification limits depend on material selection:
- **Oiles 500 SP** (default): Pmax < 3560 psi, V < 100 ft/min, PV < 46,700 psi·ft/min
- Other materials have different limits from Table 12-9 (e.g., Oiles 800: PV = 18,000 psi·ft/min)

### Validation Rules

**Minimum Length:**
- L ≥ Lmin (from thermal calculation)

**L/D Ratio:**
- 0.5 ≤ L/D ≤ 2.0 (critical design constraint)

**Inner Diameter:**
- ID > Dj (if shaft diameter is given)

**Service Range Limits (Oiles 500 SP):**
- Pmax < 3560 psi
- V < 100 ft/min
- PV < 46,700 psi·ft/min
- T < 300°F (temperature limit)

**Wear (if applicable):**
- w < wmax (if maximum wear is specified)

**Positive Values:**
- All dimensions (ID, OD, L, D) > 0
- All loads and speeds (F, N) > 0
- Tf > T∞ (lubricant temp > ambient)

### Standard Values / Constants

**Default Constants:**
- J = 778 ft·lbf/Btu (mechanical equivalent of heat)
- ℏCR = 2.7 Btu/(lbf·°F) (heat capacity constant)
- T∞ = 70°F (ambient temperature)

**Typical Design Values:**
- nd = 1.0 to 2.0 (design factor)
- L/D ratio typically around 1.0

**Oiles 500 SP Properties (from Table 12-11):**
- Tensile strength > 110,000 psi
- Elongation > 12%
- Compressive strength = 49,770 psi
- Brinell hardness > 210 HB
- Coefficient of thermal expansion > 1.6 × 10⁻⁵ °C⁻¹
- Specific gravity = 8.2

### Dropdown Options

**material (Bushing Material) from Table 12-9:**
- Oiles 800 - K = 3×10⁻¹⁰, PV = 18,000
- Oiles 500 - K = 0.6×10⁻¹⁰, PV = 46,700
- Polyactal copolymer - K = 50×10⁻¹⁰, PV = 5,000
- Polyactal homopolymer - K = 60×10⁻¹⁰, PV = 3,000
- 66 nylon - K = 200×10⁻¹⁰, PV = 2,000
- 66 nylon + 15% PTFE - K = 13×10⁻¹⁰, PV = 7,000
- + 15% PTFE + 30% glass - K = 16×10⁻¹⁰, PV = 10,000
- + 2.5% MoS₂ - K = 200×10⁻¹⁰, PV = 2,000
- 6 nylon - K = 200×10⁻¹⁰, PV = 2,000
- Polycarbonate + 15% PTFE - K = 75×10⁻¹⁰, PV = 7,000
- Sintered bronze - K = 102×10⁻¹⁰, PV = 8,500
- Phenol + 25% glass fiber - K = 8×10⁻¹⁰, PV = 11,500

**bearing_type (for friction coefficient) from Table 12-10:**
- Placetic:
  - Oiles 80 - fs = 0.05
- Composite:
  - Drymet ST - fs = 0.03
  - Toughmet - fs = 0.05
- Met:
  - Cermet M - fs = 0.05
  - Oiles 2000 - fs = 0.03
  - Oiles 300 - fs = 0.03
  - Oiles 500SP - fs = 0.03

### Future Enhancement Opportunities

**Interactive Table Lookups:**
- K and PVlimit from Table 12-9 (could auto-fill based on material selection)
- fs from Table 12-10 (could auto-fill based on bearing_type)
- ID, OD, L from Table 12-12 (could provide filtered dropdown based on Lmin and ID > Dj constraints)

**Bushing Selection Assistant:**
- Auto-filter Table 12-12 to show only bushings meeting:
  - L ≥ Lmin
  - ID > Dj (if applicable)
  - 0.5 ≤ L/D ≤ 2.0
- Highlight recommended selections

**Material Comparison:**
- Calculate performance for multiple materials side-by-side
- Compare PV products, wear rates
- Show which materials meet all requirements

**Visualization:**
- Cross-section diagram of selected bushing
- Show clearances Cr and Cd
- Display bearing geometry with dimensions

### Special Considerations

**Table 12-12 Complexity:**
- Very large table with ID from 1/2" to 6", OD from 3/4" to 6", L from 1/2" to 5"
- Not all ID/OD/L combinations are available (indicated by bullets in table)
- May need to implement as searchable table or filtered dropdown

**Material-Specific Limits:**
- Calculator shows Oiles 500 SP limits by default
- For other materials, limits should come from Table 12-9
- Consider adding material-specific validation

**Unit Consistency:**
- All equations use U.S. Customary units (inches, lbf, psi, ft/min, °F)
- No metric conversion provided in PDF
- K units: in³·min/(lbf·ft·h) - complex unit

**Informal Language:**
- PDF title includes "Other Shit That Spins" - use professional terminology
- "Random factors" should be "material properties"

**Temperature Consideration:**
- If Tf not given, may need to estimate or use conservative value
- Table 12-11 shows T < 300°F limit for Oiles 500 SP

### Calculation Order / Dependencies

```
User Inputs: F, N, Tf, nd, (optional: Dj, T∞, J, ℏCR) →

material selection →
  Table 12-9 → K, PVlimit
  Table 12-10 → fs →

Lmin = 720 fs nd F N / [J ℏCR (Tf - T∞)] →

Table 12-12 → Select ID, OD, L (where L ≥ Lmin, ID > Dj if given) →
D = ID →

ratio_LD = L / D →
Check: 0.5 ≤ ratio_LD ≤ 2.0 →

Pmax = (4/π) nd F / (DL) →
  Check: Pmax < 3560 psi (Oiles 500 SP)

P = nd F / (DL) →

V = π D N / 12 →
  Check: V < 100 ft/min (Oiles 500 SP)

PV = P × V →
  Check: PV < 46,700 psi·ft/min (Oiles 500 SP)

If wear check needed:
  t (input) →
  w = K nd F N t / (3L) →
  Check: w < wmax

All checks pass → Success
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized (~25 variables)
- [x] Source references noted for all table/graph variables (4 tables)
- [x] All equations extracted and converted to LaTeX (9 equations)
- [x] Procedure steps documented (11-step procedure)
- [x] Piecewise equations explained (N/A - no piecewise equations)
- [x] Validation rules documented (L/D ratio, service range limits)
- [x] Future enhancements noted (interactive tables, selection assistant, visualization)
- [x] Tab structure decision justified (3 tabs for procedure + equations + variables)
- [x] Specification reviewed against PDF (pages 1-4 covered)
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [ ] Ready for Implementation

**Specification Author:** Claude Code
**Date Created:** 2025-01-09
**Last Updated:** 2025-01-09

**Implementation Notes for HTML Calculator:**

**Complexity Assessment:**
- This is a **Medium complexity** calculator
- Straightforward procedure with clear validation checks
- Main challenge is Table 12-12 (large bushing sizes table)

**Table Implementation:**
1. **Table 12-9** (Wear Factors): 12 rows, 3 columns - yellow dropdown for material, auto-fill K and PVlimit
2. **Table 12-10** (Friction Coefficients): 7 rows, 3 columns - yellow dropdown for bearing_type, auto-fill fs
3. **Table 12-11** (Oiles 500 SP Service Range): Reference table for limits - display as info box
4. **Table 12-12** (Available Bushing Sizes): Very large matrix table - consider:
   - Searchable/filterable table
   - Dropdown with filters (ID range, L ≥ Lmin)
   - Auto-highlight feasible options
   - Show only combinations with bullet (available)

**Validation Indicators:**
- Use color coding for validation results:
  - Green checkmark: All validations pass
  - Yellow warning: Close to limit
  - Red X: Exceeds limit
- Show which specific limit was violated

**Recommended Calculator Structure:**
1. **Procedure Tab:**
   - Step-by-step workflow (11 steps)
   - Clear separation of sizing (steps 1-4) vs. verification (steps 5-11)
   - Real-time validation indicators
   - Success/failure summary at end

2. **General Equations Tab:**
   - Grouped by purpose (Geometry, Sizing, Performance, Wear)
   - All equations editable for exploration
   - Show which limits apply

3. **Variables Tab:**
   - Organized by Input / Table Lookup / Calculated
   - Highlight required vs. optional
   - Show defaults (T∞, J, ℏCR)

**Special Features:**
- **Auto-filter bushing table** based on:
  - L ≥ Lmin
  - ID > Dj (if shaft diameter given)
  - Highlight options meeting L/D ratio requirement

- **Multi-material comparison** mode:
  - Test multiple materials side-by-side
  - Show which pass all requirements

- **Interactive diagram**:
  - Show bushing cross-section with labeled dimensions
  - Indicate clearances

**Testing Priorities:**
- Verify Lmin calculation with thermal equation
- Test L/D ratio validation
- Verify all service range checks (Pmax, V, PV)
- Test wear calculation (optional step)
- Validate table lookups (K, fs values)

**User Experience Notes:**
- Step 10 (wear) is optional - make clear when it's needed
- Default values should be pre-filled but editable
- Table 12-12 selection is critical - needs good UI

---

## Template Version

Template Version: 1.0
Specification Version: 1.0
