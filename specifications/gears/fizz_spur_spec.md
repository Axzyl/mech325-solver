# Calculator Specification: Spur Gear Design Guide (Fizz Method)

---

## 1. Metadata

- **Calculator Name:** Spur Gear Design Guide Calculator (Fizz Method)
- **Source PDF:** fizz_spur.pdf
- **PDF Location:** `documents/gears/fizz_spur.pdf`
- **Description:** Complete 40-step spur gear design calculator following the Fizz Guide methodology. Covers basic geometry, stress analysis (bending and contact), life factors, and material selection. Includes automatic calculation of design power, pitch diameters, transmitted loads, bending/contact stresses, and required hardness values for Grade 1 steel.
- **Complexity:** High (40-step process, extensive table/graph lookups, stress calculations)
- **Tabs to Include:**
  - [x] Procedure (Steps 1-40)
  - [x] General Equations (Geometry and force equations)
  - [x] Variables (50+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Contains the sequential 40-step design process with calculations for Steps 1-9 and hardness determination
- **General Equations Tab**: Reference equations from Section 2.1.3 for geometry, pitch, tooth dimensions, forces, and motion
- **Variables Tab**: Complete nomenclature from Section 2.1.2 with all variables organized by category

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **Basic Input Parameters** |
| P | Input Power | Transmitted power | hp | User Input | - | No | Starting value |
| n<sub>P</sub> | Pinion Speed | Input shaft speed | rpm | User Input | - | No | Driving gear |
| K<sub>O</sub> | Overload Factor | Application load adjustment | - | Table | Table 9-1 | Maybe | Default 1.25 |
| P<sub>d</sub> | Diametral Pitch | Teeth per inch | teeth/in | Graph | Step 4 graph | Maybe | Size selection |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Default 18, min 17 |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | User Input | - | No | From velocity ratio |
| F | Face Width | Axial width of tooth | in | User Input | - | No | Nominal: 12/P<sub>d</sub> |
| **Stress Analysis Factors (Manual Lookups)** |
| A<sub>v</sub> | Quality Number | Manufacturing quality | - | Table | Table 9-5 | Maybe | Default 6 |
| K<sub>v</sub> | Dynamic Factor | Speed effect on load | - | Graph | Step 17 graph | Maybe | Function of v<sub>t</sub>, A<sub>v</sub> |
| J<sub>P</sub> | Bending Factor Pinion | Pinion geometry factor | - | Graph | Step 18 graph | Maybe | Function of N<sub>P</sub>, φ |
| J<sub>G</sub> | Bending Factor Gear | Gear geometry factor | - | Graph | Step 18 graph | Maybe | Function of N<sub>G</sub>, φ |
| I | Pitting Geometry Factor | Contact stress geometry | - | Graph | Step 19 graph | Maybe | Function of N<sub>P</sub>, N<sub>G</sub>, φ |
| K<sub>m</sub> | Load Distribution Factor | Misalignment/deflection | - | Calculation | Steps 20-22 | Maybe | Default 1.6 |
| K<sub>s</sub> | Size Factor | Tooth size effect | - | Table | Table 9-2 | Maybe | Default 1.0 |
| K<sub>B</sub> | Rim Thickness Factor | Backup ratio effect | - | Formula/Table | Step 23 | Maybe | Default 1.0 |
| C<sub>P</sub> | Elastic Coefficient | Material property | √psi | Table | Step 24 | Maybe | 2300 for steel |
| **Life and Reliability Factors** |
| SF | Service Factor | Duty cycle multiplier | - | User Input | - | No | Default 1.0 |
| K<sub>R</sub> | Reliability Factor | Failure probability | - | Table | Table 9-11 | Maybe | Default 1.0 |
| Y<sub>N,P</sub> | Bending Cycle Pinion | Bending fatigue factor (pinion) | - | Graph | Fig 9-21 | Maybe | Function of cycles |
| Y<sub>N,G</sub> | Bending Cycle Gear | Bending fatigue factor (gear) | - | Graph | Fig 9-21 | Maybe | Function of cycles |
| Z<sub>N,P</sub> | Pitting Cycle Pinion | Pitting fatigue factor (pinion) | - | Graph | Fig 9-22 | Maybe | Function of cycles |
| Z<sub>N,G</sub> | Pitting Cycle Gear | Pitting fatigue factor (gear) | - | Graph | Fig 9-22 | Maybe | Function of cycles |
| **Auto-Calculated Values** |
| P<sub>des</sub> | Design Power | Adjusted power | hp | Equation | EQ1 | No | P × K<sub>O</sub> |
| D<sub>P</sub> | Pinion Pitch Diameter | Pitch circle diameter | in | Equation | EQ2 | No | N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Gear Pitch Diameter | Pitch circle diameter | in | Equation | EQ3 | No | N<sub>G</sub>/P<sub>d</sub> |
| C | Center Distance | Shaft separation | in | Equation | EQ4 | No | (D<sub>P</sub>+D<sub>G</sub>)/2 |
| v<sub>t</sub> | Pitch Line Velocity | Tangential speed | ft/min | Equation | EQ5 | No | πD<sub>P</sub>n<sub>P</sub>/12 |
| W<sub>t</sub> | Transmitted Load | Tangential force | lbf | Equation | EQ6 | No | 33000P/v<sub>t</sub> |
| σ<sub>tP</sub> | Bending Stress Pinion | Expected bending stress | psi | Equation | EQ7 | No | Complex formula |
| σ<sub>tG</sub> | Bending Stress Gear | Expected bending stress | psi | Equation | EQ8 | No | σ<sub>tP</sub>×J<sub>P</sub>/J<sub>G</sub> |
| σ<sub>c</sub> | Contact Stress | Hertzian contact stress | psi | Equation | EQ9 | No | Complex formula |
| σ<sub>at,P</sub> | Required Bending Strength (Pinion) | Allowable bending stress | psi | Equation | EQ10 | No | σ<sub>tP</sub>×K<sub>R</sub>×SF/Y<sub>N,P</sub> |
| σ<sub>at,G</sub> | Required Bending Strength (Gear) | Allowable bending stress | psi | Equation | EQ11 | No | σ<sub>tG</sub>×K<sub>R</sub>×SF/Y<sub>N,G</sub> |
| σ<sub>ac,P</sub> | Required Contact Strength (Pinion) | Allowable contact stress | psi | Equation | EQ12 | No | σ<sub>c</sub>×K<sub>R</sub>×SF/Z<sub>N,P</sub> |
| σ<sub>ac,G</sub> | Required Contact Strength (Gear) | Allowable contact stress | psi | Equation | EQ13 | No | σ<sub>c</sub>×K<sub>R</sub>×SF/Z<sub>N,G</sub> |
| HB<sub>contact</sub> | Required Hardness (Contact) | Material hardness from contact | HB | Equation | EQ14 | No | (σ<sub>ac,P</sub>/1000-29.10)/0.322 |
| HB<sub>bending</sub> | Required Hardness (Bending) | Material hardness from bending | HB | Equation | EQ15 | No | (σ<sub>at,P</sub>/1000-12.8)/0.0773 |
| **General Equations Tab Variables** |
| p | Circular Pitch | Arc distance between teeth | in | Equation | GEQ1 | No | π/P<sub>d</sub> |
| m | Module | Metric pitch | mm | Equation | GEQ2 | No | 25.4/P<sub>d</sub> |
| m<sub>G</sub> | Gear Ratio | Speed ratio | - | Equation | GEQ3 | No | N<sub>G</sub>/N<sub>P</sub> |
| D<sub>o</sub> | Outside Diameter | Tip diameter | in | Equation | GEQ4 | No | (N+2)/P<sub>d</sub> |
| D<sub>R</sub> | Root Diameter | Base of tooth | in | Equation | GEQ5 | No | D-2b |
| a | Addendum | Radial height above pitch | in | Equation | GEQ6 | No | 1/P<sub>d</sub> |
| b | Dedendum | Radial depth below pitch | in | Equation | GEQ7 | No | Piecewise |
| c | Clearance | Gap at tooth bottom | in | Equation | GEQ8 | No | Piecewise |
| h<sub>f</sub> | Whole Depth | Total tooth height | in | Equation | GEQ9 | No | a+b |
| h<sub>k</sub> | Working Depth | Engaged tooth height | in | Equation | GEQ10 | No | 2a |
| t | Tooth Thickness | Circular thickness | in | Equation | GEQ11 | No | p/2 |
| D<sub>b</sub> | Base Circle Diameter | Involute base | in | Equation | GEQ12 | No | (N<sub>P</sub>/P<sub>d</sub>)cos φ |
| F<sub>t</sub> | Transmitted Force | Same as W<sub>t</sub> | lbf | Equation | GEQ13 | No | W<sub>t</sub> |
| F<sub>r</sub> | Radial Force | Separating force | lbf | Equation | GEQ14 | No | F<sub>t</sub> tan φ |
| T | Torque | Shaft torque | lbf·in | Equation | GEQ15 | No | W<sub>t</sub>×d/2 |
| φ | Pressure Angle | Tooth angle | degrees | User Input | - | No | Typically 20° or 25° |
| n<sub>G</sub> | Gear Speed | Output shaft speed | rpm | Equation | - | No | n<sub>P</sub>×N<sub>P</sub>/N<sub>G</sub> |
| W<sub>r</sub> | Radial Force | Same as F<sub>r</sub> | lbf | Equation | - | No | W<sub>t</sub> tan φ |

---

## 3. Equations

### Procedure Tab Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_{des} = P \times K_O` | Design Power | P, K<sub>O</sub> | P<sub>des</sub> | Step 1 |
| EQ2 | `D_P = \frac{N_P}{P_d}` | Pinion Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | Step 2 |
| EQ3 | `D_G = \frac{N_G}{P_d}` | Gear Pitch Diameter | N<sub>G</sub>, P<sub>d</sub> | D<sub>G</sub> | Step 3 |
| EQ4 | `C = \frac{D_P + D_G}{2}` | Center Distance | D<sub>P</sub>, D<sub>G</sub> | C | Step 4 |
| EQ5 | `v_t = \frac{\pi D_P n_P}{12}` | Pitch Line Velocity | D<sub>P</sub>, n<sub>P</sub> | v<sub>t</sub> | Step 5 |
| EQ6 | `W_t = \frac{33000 \times P}{v_t}` | Transmitted Load | P, v<sub>t</sub> | W<sub>t</sub> | Step 6 |
| EQ7 | `\sigma_{tP} = \frac{W_t P_d}{F J_P} K_O K_s K_m K_B K_v` | Bending Stress Pinion | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>P</sub>, K<sub>O</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub> | σ<sub>tP</sub> | Step 7 |
| EQ8 | `\sigma_{tG} = \sigma_{tP} \times \frac{J_P}{J_G}` | Bending Stress Gear | σ<sub>tP</sub>, J<sub>P</sub>, J<sub>G</sub> | σ<sub>tG</sub> | Step 8 |
| EQ9 | `\sigma_c = C_P \sqrt{\frac{W_t K_O K_s K_m K_v}{F D_P I}}` | Contact Stress | C<sub>P</sub>, W<sub>t</sub>, K<sub>O</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>v</sub>, F, D<sub>P</sub>, I | σ<sub>c</sub> | Step 9 |
| EQ10 | `\sigma_{at,P} = \sigma_{tP} \times \frac{K_R \times SF}{Y_{N,P}}` | Required Bending Strength (Pinion) | σ<sub>tP</sub>, K<sub>R</sub>, SF, Y<sub>N,P</sub> | σ<sub>at,P</sub> | Steps 32-33 |
| EQ11 | `\sigma_{at,G} = \sigma_{tG} \times \frac{K_R \times SF}{Y_{N,G}}` | Required Bending Strength (Gear) | σ<sub>tG</sub>, K<sub>R</sub>, SF, Y<sub>N,G</sub> | σ<sub>at,G</sub> | Steps 32-33 |
| EQ12 | `\sigma_{ac,P} = \sigma_c \times \frac{K_R \times SF}{Z_{N,P}}` | Required Contact Strength (Pinion) | σ<sub>c</sub>, K<sub>R</sub>, SF, Z<sub>N,P</sub> | σ<sub>ac,P</sub> | Steps 34-35 |
| EQ13 | `\sigma_{ac,G} = \sigma_c \times \frac{K_R \times SF}{Z_{N,G}}` | Required Contact Strength (Gear) | σ<sub>c</sub>, K<sub>R</sub>, SF, Z<sub>N,G</sub> | σ<sub>ac,G</sub> | Steps 34-35 |
| EQ14 | `HB_{contact} = \frac{\sigma_{ac,P}/1000 - 29.10}{0.322}` | Required Hardness (Contact) | σ<sub>ac,P</sub> | HB<sub>contact</sub> | Step 37 (Grade 1) |
| EQ15 | `HB_{bending} = \frac{\sigma_{at,P}/1000 - 12.8}{0.0773}` | Required Hardness (Bending) | σ<sub>at,P</sub> | HB<sub>bending</sub> | Step 37 (Grade 1) |

### General Equations Tab Equations

| ID | LaTeX Formula | Description | Notes |
|----|---------------|-------------|-------|
| GEQ1 | `p = \frac{\pi D_P}{N_P} = \frac{\pi D_G}{N_G} = \frac{\pi}{P_d}` | Circular Pitch | Multiple forms |
| GEQ2 | `m = \frac{D_P}{N_P} = \frac{D_G}{N_G} = \frac{1}{P_d}` | Module | Incorrect formula in HTML (should be 25.4/P<sub>d</sub>) |
| GEQ3 | `m_G = \frac{N_G}{N_P}` | Gear Ratio | - |
| GEQ4 | `D_o = \frac{N + 2}{P_d}` | Outside Diameter | N is generic teeth count |
| GEQ5 | `D_R = D - 2b` | Root Diameter | - |
| GEQ6 | `a = \frac{1}{P_d}` | Addendum | Standard full-depth |
| GEQ7 | `b = \begin{cases} \frac{1.25}{P_d} & P_d < 20 \\ \frac{1.20}{P_d} + 0.002 & P_d \geq 20 \end{cases}` | Dedendum | Piecewise: coarse vs fine |
| GEQ8 | `c = \begin{cases} \frac{0.25}{P_d} & P_d < 20 \\ \frac{0.20}{P_d} + 0.002 & P_d \geq 20 \end{cases}` | Clearance | Piecewise: coarse vs fine |
| GEQ9 | `h_f = a + b` | Whole Depth | - |
| GEQ10 | `h_k = 2a` | Working Depth | - |
| GEQ11 | `t = \frac{p}{2} = \frac{\pi}{2P_d}` | Tooth Thickness | Standard |
| GEQ12 | `F_{nominal} = \frac{12}{P_d} \quad \left(\frac{8}{P_d} < F < \frac{16}{P_d}\right)` | Face Width (Nominal) | With range |
| GEQ13 | `C = \frac{D_P + D_G}{2} = \frac{N_P + N_G}{2P_d}` | Center Distance | Multiple forms |
| GEQ14 | `D_b = \frac{N_p}{P_d} \cos \phi` | Base Circle Diameter | φ in degrees |
| GEQ15 | `F_{t,x-y} = W_t` | Transmitted Force | Notation equivalence |
| GEQ16 | `F_{r,x-y} = F_{t,x-y} \tan \phi` | Radial Force | φ in degrees |
| GEQ17 | `T = W_t \frac{d}{2}` | Torque | d is pitch diameter |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Basic Input Parameters** |
| - | Input Power | User Input | Enter P [hp] | Starting value |
| - | Input Speed | User Input | Enter n<sub>P</sub> [rpm] | Pinion speed |
| - | Overload Factor | Table | Select K<sub>O</sub> from Table 9-1 | Default 1.25 |
| - | Diametral Pitch | Graph | Select P<sub>d</sub> from Step 4 graph | Size selection |
| - | Pinion Teeth | User Input | Enter N<sub>P</sub> (typically 18) | ≥17 teeth |
| - | Gear Teeth | User Input | Enter N<sub>G</sub> | From velocity ratio |
| - | Face Width | User Input | Enter F [in] | Nominal: 12/P<sub>d</sub> |
| **Step 1: Design Power** |
| 1 | Calculate Design Power | Equation | P<sub>des</sub> = P × K<sub>O</sub> (EQ1) | Auto-calculated |
| **Steps 2-3: Pitch Diameters** |
| 2 | Calculate Pinion Diameter | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> (EQ2) | Auto-calculated |
| 3 | Calculate Gear Diameter | Equation | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ3) | Auto-calculated |
| **Step 4: Center Distance** |
| 4 | Calculate Center Distance | Equation | C = (D<sub>P</sub>+D<sub>G</sub>)/2 (EQ4) | Auto-calculated |
| **Step 5: Pitch Line Velocity** |
| 5 | Calculate Velocity | Equation | v<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 (EQ5) | [ft/min] |
| **Step 6: Transmitted Load** |
| 6 | Calculate Load | Equation | W<sub>t</sub> = 33000×P/v<sub>t</sub> (EQ6) | [lbf] |
| **Stress Analysis Factors (Manual Lookups)** |
| - | Quality Number | Table | Select A<sub>v</sub> from Table 9-5 | Default 6 |
| 17 | Dynamic Factor | Graph | Lookup K<sub>v</sub> from Step 17 graph | Function of v<sub>t</sub>, A<sub>v</sub> |
| 18 | Bending Factors | Graph | Lookup J<sub>P</sub>, J<sub>G</sub> from Step 18 graph | Function of N, φ |
| 19 | Pitting Factor | Graph | Lookup I from Step 19 graph | Function of N<sub>P</sub>, N<sub>G</sub>, φ |
| 20-22 | Load Distribution | Calculation | Calculate K<sub>m</sub> from Steps 20-22 | Default 1.6 |
| - | Size Factor | Table | Select K<sub>s</sub> from Table 9-2 | Default 1.0 |
| 23 | Rim Thickness | Formula | Calculate K<sub>B</sub> from Step 23 | Default 1.0 |
| 24 | Elastic Coefficient | Table | Select C<sub>P</sub> from Step 24 | 2300 for steel |
| **Steps 7-8: Bending Stress** |
| 7 | Calculate Pinion Bending Stress | Equation | σ<sub>tP</sub> = (W<sub>t</sub>P<sub>d</sub>/(FJ<sub>P</sub>))×K<sub>O</sub>K<sub>s</sub>K<sub>m</sub>K<sub>B</sub>K<sub>v</sub> (EQ7) | [psi] |
| 8 | Calculate Gear Bending Stress | Equation | σ<sub>tG</sub> = σ<sub>tP</sub>×J<sub>P</sub>/J<sub>G</sub> (EQ8) | [psi] |
| **Step 9: Contact Stress** |
| 9 | Calculate Contact Stress | Equation | σ<sub>c</sub> = C<sub>P</sub>×√((W<sub>t</sub>K<sub>O</sub>K<sub>s</sub>K<sub>m</sub>K<sub>v</sub>)/(FD<sub>P</sub>I)) (EQ9) | [psi] |
| **Life/Reliability Factors** |
| - | Service Factor | User Input | Enter SF | Default 1.0 |
| - | Reliability Factor | Table | Select K<sub>R</sub> from Table 9-11 | Default 1.0 |
| 30-31 | Bending Cycle Factors | Graph | Lookup Y<sub>N,P</sub>, Y<sub>N,G</sub> from Fig 9-21 | Function of cycles |
| 30-31 | Pitting Cycle Factors | Graph | Lookup Z<sub>N,P</sub>, Z<sub>N,G</sub> from Fig 9-22 | Function of cycles |
| **Steps 32-33: Required Bending Strengths** |
| 32 | Required Bending Strength (Pinion) | Equation | σ<sub>at,P</sub> = σ<sub>tP</sub>×K<sub>R</sub>×SF/Y<sub>N,P</sub> (EQ10) | [psi] |
| 33 | Required Bending Strength (Gear) | Equation | σ<sub>at,G</sub> = σ<sub>tG</sub>×K<sub>R</sub>×SF/Y<sub>N,G</sub> (EQ11) | [psi] |
| **Steps 34-35: Required Contact Strengths** |
| 34 | Required Contact Strength (Pinion) | Equation | σ<sub>ac,P</sub> = σ<sub>c</sub>×K<sub>R</sub>×SF/Z<sub>N,P</sub> (EQ12) | [psi] |
| 35 | Required Contact Strength (Gear) | Equation | σ<sub>ac,G</sub> = σ<sub>c</sub>×K<sub>R</sub>×SF/Z<sub>N,G</sub> (EQ13) | [psi] |
| **Step 37: Required Hardness (Grade 1 Steel)** |
| 37a | Hardness from Contact | Equation | HB<sub>contact</sub> = (σ<sub>ac,P</sub>/1000-29.10)/0.322 (EQ14) | Use max(HB<sub>contact</sub>, HB<sub>bending</sub>) |
| 37b | Hardness from Bending | Equation | HB<sub>bending</sub> = (σ<sub>at,P</sub>/1000-12.8)/0.0773 (EQ15) | Use max(HB<sub>contact</sub>, HB<sub>bending</sub>) |
| **Steps 10-16, 25-29, 36, 38-40** |
| - | Additional Steps | Manual | Not implemented in calculator | Refer to PDF for complete process |

---

## 5. Additional Notes

### Validation Rules
- **Minimum Teeth**: N<sub>P</sub> ≥ 17 (to avoid undercutting)
- **Integer Values**: N<sub>P</sub>, N<sub>G</sub> must be positive integers
- **Positive Values**: P, n<sub>P</sub>, P<sub>d</sub>, F, all diameters, all factors must be > 0
- **Pressure Angle**: φ typically 20° or 25° (standard values)
- **Velocity Range**: Quality number A<sub>v</sub> affects valid velocity ranges
- **Face Width Range**: 8/P<sub>d</sub> < F < 16/P<sub>d</sub> (recommended)
- **Hardness Selection**: Use larger of HB<sub>contact</sub> and HB<sub>bending</sub>

### Standard Values / Constants
- **Default K<sub>O</sub>**: 1.25 (moderate overload)
- **Default N<sub>P</sub>**: 18 teeth (common starting point)
- **Default A<sub>v</sub>**: 6 (good quality)
- **Default K<sub>m</sub>**: 1.6 (typical load distribution)
- **Default K<sub>s</sub>**: 1.0 (standard size)
- **Default K<sub>B</sub>**: 1.0 (solid gear)
- **Default C<sub>P</sub>**: 2300 √psi (steel-on-steel)
- **Default SF**: 1.0 (standard service)
- **Default K<sub>R</sub>**: 1.0 (99% reliability)
- **Nominal F**: 12/P<sub>d</sub> inches
- **π**: Use 3.14159 or Math.PI
- **Power Conversion**: 1 hp = 33000 ft·lbf/min

### Piecewise Equations
**Dedendum (b):**
- Coarse pitch (P<sub>d</sub> < 20): b = 1.25/P<sub>d</sub>
- Fine pitch (P<sub>d</sub> ≥ 20): b = 1.20/P<sub>d</sub> + 0.002

**Clearance (c):**
- Coarse pitch (P<sub>d</sub> < 20): c = 0.25/P<sub>d</sub>
- Fine pitch (P<sub>d</sub> ≥ 20): c = 0.20/P<sub>d</sub> + 0.002

### Dropdown Options

**None in current implementation** - All lookups are manual table/graph references

**Potential Future Dropdowns:**
- Pressure angle: 14.5°, 20°, 25°
- Quality number (A<sub>v</sub>): 3-11 (Table 9-5)
- Material: Steel, Cast Iron, Bronze, etc.
- Grade: Grade 1, Grade 2, Nitrided (affects hardness formulas)

### Future Enhancement Opportunities
- **Table 9-1**: Automatic K<sub>O</sub> lookup from application type
- **Table 9-5**: Dropdown for quality number A<sub>v</sub>
- **Step 4 Graph**: Interactive P<sub>d</sub> selection based on power and speed
- **Step 17 Graph**: Automatic K<sub>v</sub> calculation from v<sub>t</sub> and A<sub>v</sub>
- **Step 18 Graph**: Automatic J<sub>P</sub>, J<sub>G</sub> lookup from N and φ
- **Step 19 Graph**: Automatic I lookup from N<sub>P</sub>, N<sub>G</sub>, φ
- **Table 9-2**: Automatic K<sub>s</sub> lookup
- **Step 20-22**: Implement K<sub>m</sub> calculation formulas
- **Step 23**: Implement K<sub>B</sub> calculation
- **Table 9-11**: Dropdown for reliability factor
- **Fig 9-21**: Automatic Y<sub>N</sub> lookup from cycle count
- **Fig 9-22**: Automatic Z<sub>N</sub> lookup from cycle count
- **Material Database**: Implement multiple grades with different hardness formulas
- **Comparison Tool**: Show pinion vs gear stress comparison
- **Optimization**: Suggest optimal N<sub>P</sub>, P<sub>d</sub> for minimum size/cost

### Special Considerations
- **40-Step Process**: Calculator implements critical calculation steps (1-9, 32-37), but complete design requires manual steps from PDF
- **Grade 1 Steel Only**: Hardness formulas (EQ14, EQ15) are specific to Grade 1 steel. Other grades have different constants.
- **Pinion Critical**: Use pinion values (σ<sub>at,P</sub>, σ<sub>ac,P</sub>) for hardness calculation as pinion experiences more stress cycles
- **Manual Interpolation**: Many graphs require 2D interpolation or curve reading
- **Stress Units**: All stresses in psi (pounds per square inch)
- **Velocity Units**: Pitch line velocity in ft/min, speed in rpm, diameter in inches
- **Variable Synchronization**: HTML implements extensive variable syncing across tabs using varInstanceMap
- **Session Persistence**: Uses localStorage to save/load calculator state
- **MathJax Rendering**: LaTeX equations rendered with MathJax 3

### Known Issues / Corrections Needed
- **Module Formula (GEQ2)**: HTML shows m = 1/P<sub>d</sub>, but correct metric module is m = 25.4/P<sub>d</sub> mm
- **Missing Steps**: Steps 10-16, 25-29, 36, 38-40 not implemented (geometry factors, material selection, detailed checks)

### Calculation Order / Dependencies

```
User Inputs: P, nP, KO, Pd, NP, NG, F
↓
Step 1: Pdes = P × KO (EQ1)
↓
Steps 2-3: DP = NP/Pd, DG = NG/Pd (EQ2, EQ3)
↓
Step 4: C = (DP+DG)/2 (EQ4)
↓
Step 5: vt = πDP×nP/12 (EQ5)
↓
Step 6: Wt = 33000×P/vt (EQ6)
↓
Manual Lookups: Av, Kv, JP, JG, I, Km, Ks, KB, CP
↓
Step 7: σtP = (Wt×Pd/(F×JP))×KO×Ks×Km×KB×Kv (EQ7)
↓
Step 8: σtG = σtP×JP/JG (EQ8)
↓
Step 9: σc = CP×√((Wt×KO×Ks×Km×Kv)/(F×DP×I)) (EQ9)
↓
Manual Lookups: SF, KR, YNP, YNG, ZNP, ZNG
↓
Steps 32-33: σat,P = σtP×KR×SF/YNP, σat,G = σtG×KR×SF/YNG (EQ10, EQ11)
↓
Steps 34-35: σac,P = σc×KR×SF/ZNP, σac,G = σc×KR×SF/ZNG (EQ12, EQ13)
↓
Step 37: HBcontact = (σac,P/1000-29.10)/0.322 (EQ14)
         HBbending = (σat,P/1000-12.8)/0.0773 (EQ15)
↓
Final: HBrequired = max(HBcontact, HBbending)
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (dedendum, clearance)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML source
- [x] Consistent naming and formatting throughout
- [x] Known issues documented

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This specification documents an **existing** HTML calculator at `C:\data\vscode\m325_solver_v2\calculators\gears\fizz_spur.html`. The calculator is already functional with:
- 3-tab interface (Procedure, General Equations, Variables)
- Automatic calculation for steps 1-9 and hardness determination
- Variable synchronization across tabs using varInstanceMap
- LocalStorage session persistence
- MathJax equation rendering
- Computed value highlighting (green backgrounds)

The calculator requires extensive manual table/graph lookups from the PDF. Future enhancements could digitize these tables for automatic lookup.

**40-Step Fizz Process:** This calculator implements the calculation-intensive portions (Steps 1-9, 32-37). The complete design process includes 40 steps covering:
- Steps 1-9: Basic calculations (implemented)
- Steps 10-16: Geometry factors and tooth dimensions (manual)
- Step 17-24: Stress factors (manual lookups)
- Steps 25-29: Material properties (manual)
- Steps 30-31: Cycle factors (manual lookups)
- Steps 32-35: Required strengths (implemented)
- Steps 36-37: Material selection (hardness implemented)
- Steps 38-40: Final validation and documentation (manual)

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template structure
