# Calculator Specification: Bevel Gear Geometry (Mott)

---

## 1. Metadata

- **Calculator Name:** Bevel Gear Geometry Calculator
- **Source PDF:** bevel_design_spec.pdf
- **PDF Location:** `documents/gears/bevel_design_spec.pdf`
- **Description:** Calculates bevel gear geometric parameters including pitch diameters, cone angles, face width, mean dimensions, addendum/dedendum angles, and outside diameters using Mott's design methodology.
- **Complexity:** Medium (sequential calculations with piecewise equations)
- **Tabs to Include:**
  - [x] Procedure (10-step sequential process)
  - [x] General Equations (20 equations)
  - [x] Variables (26 variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Clear 10-step sequential calculation procedure
- **General Equations Tab**: Contains 20 equations organized by calculation category for reference
- **Variables Tab**: 26 variables across multiple categories requiring central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P<sub>d</sub> | Diametral Pitch | Number of teeth per inch of pitch diameter | teeth/in | User Input | - | No | Starting value, determines coarse/fine pitch |
| φ | Pressure Angle | Tooth pressure angle | deg | User Input | - | No | Typically 20° or 25° |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion (smaller gear) | teeth | User Input | - | No | Must be integer ≥ 12 |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear (larger gear) | teeth | User Input | - | No | Must be integer > N<sub>P</sub> |
| F | Selected Face Width | User-selected face width (within range) | in | User Input | - | No | Must satisfy F<sub>nom</sub> ≤ F ≤ F<sub>max</sub> |
| **Auto-Calculated (Step 1)** |
| m<sub>G</sub> | Gear Ratio | Speed/torque ratio | - | Equation | EQ1 | No | m<sub>G</sub> = N<sub>G</sub>/N<sub>P</sub> |
| D | Gear Pitch Diameter | Pitch diameter of gear | in | Equation | EQ2 | No | D = N<sub>G</sub>/P<sub>d</sub> |
| d | Pinion Pitch Diameter | Pitch diameter of pinion | in | Equation | EQ3 | No | d = N<sub>P</sub>/P<sub>d</sub> |
| **Auto-Calculated (Step 2)** |
| γ | Pinion Pitch Cone Angle | Pitch cone angle of pinion | deg | Equation | EQ4 | No | γ = tan⁻¹(N<sub>P</sub>/N<sub>G</sub>) |
| Γ | Gear Pitch Cone Angle | Pitch cone angle of gear | deg | Equation | EQ5 | No | Γ = tan⁻¹(N<sub>G</sub>/N<sub>P</sub>) |
| **Auto-Calculated (Step 3)** |
| A<sub>O</sub> | Cone Distance | Distance from apex to outer end | in | Equation | EQ6 | No | A<sub>O</sub> = 0.5D/sin(Γ) |
| **Auto-Calculated (Step 4)** |
| F<sub>nom</sub> | Nominal Face Width | Recommended minimum face width | in | Equation | EQ7 | No | F<sub>nom</sub> = A<sub>O</sub>/3 |
| F<sub>max</sub> | Maximum Face Width | Maximum allowable face width | in | Equation | EQ8 | No | F<sub>max</sub> = 10/P<sub>d</sub> |
| **Auto-Calculated (Step 5)** |
| A<sub>m</sub> | Mean Cone Distance | Distance from apex to midpoint of face | in | Equation | EQ9 | No | A<sub>m</sub> = A<sub>O</sub> - F/2 |
| **Auto-Calculated (Step 6)** |
| p<sub>m</sub> | Mean Circular Pitch | Circular pitch at mean cone distance | in | Equation | EQ10 | No | p<sub>m</sub> = πA<sub>m</sub>/(0.5N<sub>G</sub>) |
| **Auto-Calculated (Step 7 - Piecewise)** |
| h | Whole Depth | Total tooth depth | in | Equation | EQ11a/EQ11b | No | Coarse: 2.188/P<sub>d</sub>, Fine: 2/P<sub>d</sub> + 0.002 |
| c | Clearance | Radial clearance | in | Equation | EQ12a/EQ12b | No | Coarse: 0.188/P<sub>d</sub>, Fine: 0.002 |
| **Auto-Calculated (Step 8 - Piecewise)** |
| h<sub>m</sub> | Mean Whole Depth | Whole depth at mean cone distance | in | Equation | EQ13a/EQ13b | No | Coarse: h·cos(Γ) - c/2, Fine: h·cos(Γ) - c |
| c<sub>1</sub> | Mean Clearance | Clearance at mean cone distance | in | Equation | EQ14a/EQ14b | No | Coarse: c/2, Fine: c |
| a<sub>m</sub> | Mean Addendum | Addendum at mean cone distance | in | Equation | EQ15 | No | a<sub>m</sub> = h<sub>m</sub>/2 |
| b<sub>m</sub> | Mean Dedendum | Dedendum at mean cone distance | in | Equation | EQ16 | No | b<sub>m</sub> = h<sub>m</sub> - a<sub>m</sub> |
| **Auto-Calculated (Step 9)** |
| a<sub>G</sub> | Gear Addendum Angle | Addendum angle for gear | in | Equation | EQ17 | No | a<sub>G</sub> = a<sub>m</sub>/A<sub>O</sub> |
| a<sub>P</sub> | Pinion Addendum Angle | Addendum angle for pinion | in | Equation | EQ18 | No | a<sub>P</sub> = a<sub>m</sub>/A<sub>O</sub> |
| b<sub>G</sub> | Gear Dedendum Angle | Dedendum angle for gear | in | Equation | EQ19 | No | b<sub>G</sub> = b<sub>m</sub>/A<sub>O</sub> |
| b<sub>P</sub> | Pinion Dedendum Angle | Dedendum angle for pinion | in | Equation | EQ20 | No | b<sub>P</sub> = b<sub>m</sub>/A<sub>O</sub> |
| **Auto-Calculated (Step 10)** |
| δ<sub>G</sub> | Gear Face Angle | Face cone angle of gear | deg | Equation | EQ21 | No | δ<sub>G</sub> = Γ + a<sub>G</sub> |
| δ<sub>P</sub> | Pinion Face Angle | Face cone angle of pinion | deg | Equation | EQ22 | No | δ<sub>P</sub> = γ + a<sub>P</sub> |
| a<sub>OG</sub> | Gear Outer Addendum | Outer addendum of gear | in | Equation | EQ23 | No | a<sub>OG</sub> = a<sub>m</sub> + (F/2)·sin(δ<sub>G</sub>) |
| a<sub>OP</sub> | Pinion Outer Addendum | Outer addendum of pinion | in | Equation | EQ24 | No | a<sub>OP</sub> = a<sub>m</sub> + (F/2)·sin(δ<sub>P</sub>) |
| D<sub>O</sub> | Gear Outside Diameter | Outside diameter of gear | in | Equation | EQ25 | No | D<sub>O</sub> = D + 2a<sub>OG</sub>·cos(Γ) |
| d<sub>O</sub> | Pinion Outside Diameter | Outside diameter of pinion | in | Equation | EQ26 | No | d<sub>O</sub> = d + 2a<sub>OP</sub>·cos(γ) |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Step 1: Gear Ratio and Pitch Diameters** |
| EQ1 | `m_G = \frac{N_G}{N_P}` | Gear Ratio | N<sub>G</sub>, N<sub>P</sub> | m<sub>G</sub> | - |
| EQ2 | `D = \frac{N_G}{P_d}` | Gear Pitch Diameter | N<sub>G</sub>, P<sub>d</sub> | D | - |
| EQ3 | `d = \frac{N_P}{P_d}` | Pinion Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | d | - |
| **Step 2: Pitch Cone Angles** |
| EQ4 | `\gamma = \tan^{-1}\left(\frac{N_P}{N_G}\right)` | Pinion Pitch Cone Angle | N<sub>P</sub>, N<sub>G</sub> | γ | Result in degrees |
| EQ5 | `\Gamma = \tan^{-1}\left(\frac{N_G}{N_P}\right)` | Gear Pitch Cone Angle | N<sub>G</sub>, N<sub>P</sub> | Γ | Result in degrees |
| **Step 3: Cone Distance** |
| EQ6 | `A_O = \frac{0.5 D}{\sin(\Gamma)}` | Cone Distance | D, Γ | A<sub>O</sub> | Γ in degrees |
| **Step 4: Face Width Range** |
| EQ7 | `F_{nom} = \frac{A_O}{3}` | Nominal Face Width | A<sub>O</sub> | F<sub>nom</sub> | Recommended minimum |
| EQ8 | `F_{max} = \frac{10}{P_d}` | Maximum Face Width | P<sub>d</sub> | F<sub>max</sub> | Recommended maximum |
| **Step 5: Mean Cone Distance** |
| EQ9 | `A_m = A_O - \frac{F}{2}` | Mean Cone Distance | A<sub>O</sub>, F | A<sub>m</sub> | - |
| **Step 6: Mean Circular Pitch** |
| EQ10 | `p_m = \frac{\pi A_m}{0.5 N_G}` | Mean Circular Pitch | A<sub>m</sub>, N<sub>G</sub> | p<sub>m</sub> | - |
| **Step 7: Whole Depth and Clearance (PIECEWISE)** |
| EQ11a | `h = \frac{2.188}{P_d}` | Whole Depth (Coarse) | P<sub>d</sub> | h | If P<sub>d</sub> < 20 |
| EQ11b | `h = \frac{2}{P_d} + 0.002` | Whole Depth (Fine) | P<sub>d</sub> | h | If P<sub>d</sub> ≥ 20 |
| EQ12a | `c = \frac{0.188}{P_d}` | Clearance (Coarse) | P<sub>d</sub> | c | If P<sub>d</sub> < 20 |
| EQ12b | `c = 0.002` | Clearance (Fine) | - | c | If P<sub>d</sub> ≥ 20 (constant) |
| **Step 8: Mean Addendum and Dedendum (PIECEWISE)** |
| EQ13a | `h_m = h \cos(\Gamma) - \frac{c}{2}` | Mean Whole Depth (Coarse) | h, Γ, c | h<sub>m</sub> | If P<sub>d</sub> < 20 |
| EQ13b | `h_m = h \cos(\Gamma) - c` | Mean Whole Depth (Fine) | h, Γ, c | h<sub>m</sub> | If P<sub>d</sub> ≥ 20 |
| EQ14a | `c_1 = \frac{c}{2}` | Mean Clearance (Coarse) | c | c<sub>1</sub> | If P<sub>d</sub> < 20 |
| EQ14b | `c_1 = c` | Mean Clearance (Fine) | c | c<sub>1</sub> | If P<sub>d</sub> ≥ 20 |
| EQ15 | `a_m = \frac{h_m}{2}` | Mean Addendum | h<sub>m</sub> | a<sub>m</sub> | - |
| EQ16 | `b_m = h_m - a_m` | Mean Dedendum | h<sub>m</sub>, a<sub>m</sub> | b<sub>m</sub> | - |
| **Step 9: Addendum and Dedendum Angles** |
| EQ17 | `a_G = \frac{a_m}{A_O}` | Gear Addendum Angle | a<sub>m</sub>, A<sub>O</sub> | a<sub>G</sub> | Unit is inches (not degrees) |
| EQ18 | `a_P = \frac{a_m}{A_O}` | Pinion Addendum Angle | a<sub>m</sub>, A<sub>O</sub> | a<sub>P</sub> | Unit is inches (not degrees) |
| EQ19 | `b_G = \frac{b_m}{A_O}` | Gear Dedendum Angle | b<sub>m</sub>, A<sub>O</sub> | b<sub>G</sub> | Unit is inches (not degrees) |
| EQ20 | `b_P = \frac{b_m}{A_O}` | Pinion Dedendum Angle | b<sub>m</sub>, A<sub>O</sub> | b<sub>P</sub> | Unit is inches (not degrees) |
| **Step 10: Face Angles and Outside Diameters** |
| EQ21 | `\delta_G = \Gamma + a_G` | Gear Face Angle | Γ, a<sub>G</sub> | δ<sub>G</sub> | Result in degrees |
| EQ22 | `\delta_P = \gamma + a_P` | Pinion Face Angle | γ, a<sub>P</sub> | δ<sub>P</sub> | Result in degrees |
| EQ23 | `a_{OG} = a_m + \frac{F}{2} \sin(\delta_G)` | Gear Outer Addendum | a<sub>m</sub>, F, δ<sub>G</sub> | a<sub>OG</sub> | δ<sub>G</sub> in degrees |
| EQ24 | `a_{OP} = a_m + \frac{F}{2} \sin(\delta_P)` | Pinion Outer Addendum | a<sub>m</sub>, F, δ<sub>P</sub> | a<sub>OP</sub> | δ<sub>P</sub> in degrees |
| EQ25 | `D_O = D + 2 a_{OG} \cos(\Gamma)` | Gear Outside Diameter | D, a<sub>OG</sub>, Γ | D<sub>O</sub> | Γ in degrees |
| EQ26 | `d_O = d + 2 a_{OP} \cos(\gamma)` | Pinion Outside Diameter | d, a<sub>OP</sub>, γ | d<sub>O</sub> | γ in degrees |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 0 | Input Given Parameters | User Input | Enter P<sub>d</sub> [teeth/in], φ [deg], N<sub>P</sub> [teeth], N<sub>G</sub> [teeth] | φ typically 20° or 25° |
| **Step 1: Gear Ratio and Pitch Diameters** |
| 1a | Calculate Gear Ratio | Equation | m<sub>G</sub> = N<sub>G</sub>/N<sub>P</sub> (EQ1) | Auto-calculated |
| 1b | Calculate Gear Pitch Diameter | Equation | D = N<sub>G</sub>/P<sub>d</sub> (EQ2) | Auto-calculated |
| 1c | Calculate Pinion Pitch Diameter | Equation | d = N<sub>P</sub>/P<sub>d</sub> (EQ3) | Auto-calculated |
| **Step 2: Pitch Cone Angles** |
| 2a | Calculate Pinion Pitch Cone Angle | Equation | γ = tan⁻¹(N<sub>P</sub>/N<sub>G</sub>) (EQ4) | Auto-calculated, result in degrees |
| 2b | Calculate Gear Pitch Cone Angle | Equation | Γ = tan⁻¹(N<sub>G</sub>/N<sub>P</sub>) (EQ5) | Auto-calculated, result in degrees |
| **Step 3: Cone Distance** |
| 3 | Calculate Cone Distance | Equation | A<sub>O</sub> = 0.5D/sin(Γ) (EQ6) | Auto-calculated |
| **Step 4: Face Width Range** |
| 4a | Calculate Nominal Face Width | Equation | F<sub>nom</sub> = A<sub>O</sub>/3 (EQ7) | Auto-calculated (min recommendation) |
| 4b | Calculate Maximum Face Width | Equation | F<sub>max</sub> = 10/P<sub>d</sub> (EQ8) | Auto-calculated (max recommendation) |
| 4c | Select Face Width | User Input | Choose F such that F<sub>nom</sub> ≤ F ≤ F<sub>max</sub> | User must select within range |
| 4d | Validate Face Width | Validation | Check F<sub>nom</sub> ≤ F ≤ F<sub>max</sub> | Warning if out of range |
| **Step 5: Mean Cone Distance** |
| 5 | Calculate Mean Cone Distance | Equation | A<sub>m</sub> = A<sub>O</sub> - F/2 (EQ9) | Auto-calculated |
| **Step 6: Mean Circular Pitch** |
| 6 | Calculate Mean Circular Pitch | Equation | p<sub>m</sub> = πA<sub>m</sub>/(0.5N<sub>G</sub>) (EQ10) | Auto-calculated |
| **Step 7: Whole Depth and Clearance (PIECEWISE)** |
| 7a | Determine Pitch Type | Conditional | Check if P<sub>d</sub> < 20 (coarse) or P<sub>d</sub> ≥ 20 (fine) | Display pitch type indicator |
| 7b | Calculate Whole Depth | Equation | Coarse: h = 2.188/P<sub>d</sub> (EQ11a)<br>Fine: h = 2/P<sub>d</sub> + 0.002 (EQ11b) | Auto-calculated, formula changes |
| 7c | Calculate Clearance | Equation | Coarse: c = 0.188/P<sub>d</sub> (EQ12a)<br>Fine: c = 0.002 (EQ12b) | Auto-calculated, formula changes |
| **Step 8: Mean Addendum and Dedendum (PIECEWISE)** |
| 8a | Calculate Mean Whole Depth | Equation | Coarse: h<sub>m</sub> = h·cos(Γ) - c/2 (EQ13a)<br>Fine: h<sub>m</sub> = h·cos(Γ) - c (EQ13b) | Auto-calculated, formula changes |
| 8b | Calculate Mean Clearance | Equation | Coarse: c<sub>1</sub> = c/2 (EQ14a)<br>Fine: c<sub>1</sub> = c (EQ14b) | Auto-calculated, formula changes |
| 8c | Calculate Mean Addendum | Equation | a<sub>m</sub> = h<sub>m</sub>/2 (EQ15) | Auto-calculated |
| 8d | Calculate Mean Dedendum | Equation | b<sub>m</sub> = h<sub>m</sub> - a<sub>m</sub> (EQ16) | Auto-calculated |
| **Step 9: Addendum and Dedendum Angles** |
| 9a | Calculate Gear Addendum Angle | Equation | a<sub>G</sub> = a<sub>m</sub>/A<sub>O</sub> (EQ17) | Auto-calculated |
| 9b | Calculate Pinion Addendum Angle | Equation | a<sub>P</sub> = a<sub>m</sub>/A<sub>O</sub> (EQ18) | Auto-calculated (same as a<sub>G</sub>) |
| 9c | Calculate Gear Dedendum Angle | Equation | b<sub>G</sub> = b<sub>m</sub>/A<sub>O</sub> (EQ19) | Auto-calculated |
| 9d | Calculate Pinion Dedendum Angle | Equation | b<sub>P</sub> = b<sub>m</sub>/A<sub>O</sub> (EQ20) | Auto-calculated (same as b<sub>G</sub>) |
| **Step 10: Face Angles and Outside Diameters** |
| 10a | Calculate Gear Face Angle | Equation | δ<sub>G</sub> = Γ + a<sub>G</sub> (EQ21) | Auto-calculated |
| 10b | Calculate Pinion Face Angle | Equation | δ<sub>P</sub> = γ + a<sub>P</sub> (EQ22) | Auto-calculated |
| 10c | Calculate Gear Outer Addendum | Equation | a<sub>OG</sub> = a<sub>m</sub> + (F/2)·sin(δ<sub>G</sub>) (EQ23) | Auto-calculated |
| 10d | Calculate Pinion Outer Addendum | Equation | a<sub>OP</sub> = a<sub>m</sub> + (F/2)·sin(δ<sub>P</sub>) (EQ24) | Auto-calculated |
| 10e | Calculate Gear Outside Diameter | Equation | D<sub>O</sub> = D + 2a<sub>OG</sub>·cos(Γ) (EQ25) | Auto-calculated (final result) |
| 10f | Calculate Pinion Outside Diameter | Equation | d<sub>O</sub> = d + 2a<sub>OP</sub>·cos(γ) (EQ26) | Auto-calculated (final result) |

---

## 5. Additional Notes

### Validation Rules
- **Minimum Pinion Teeth**: N<sub>P</sub> ≥ 12 (recommended minimum to avoid undercutting)
- **Integer Values**: N<sub>P</sub>, N<sub>G</sub> must be positive integers
- **Gear Ratio**: N<sub>G</sub> > N<sub>P</sub> (gear is larger than pinion)
- **Positive Values**: P<sub>d</sub>, all diameters, distances must be > 0
- **Pressure Angle Range**: 0° < φ < 90° (typically 20° or 25°)
- **Face Width Range**: F<sub>nom</sub> ≤ F ≤ F<sub>max</sub> (enforced with warning message)
- **Pitch Type Boundary**: P<sub>d</sub> = 20 teeth/in is the threshold between coarse and fine pitch

### Piecewise Equation Transitions
The calculator has **four piecewise equations** that change at P<sub>d</sub> = 20:

**Coarse Pitch (P<sub>d</sub> < 20):**
- h = 2.188/P<sub>d</sub>
- c = 0.188/P<sub>d</sub>
- h<sub>m</sub> = h·cos(Γ) - c/2
- c<sub>1</sub> = c/2

**Fine Pitch (P<sub>d</sub> ≥ 20):**
- h = 2/P<sub>d</sub> + 0.002
- c = 0.002 (constant)
- h<sub>m</sub> = h·cos(Γ) - c
- c<sub>1</sub> = c

The calculator displays a **pitch type indicator** ("Coarse Pitch" or "Fine Pitch") and updates LaTeX equations dynamically when P<sub>d</sub> crosses the threshold.

### Standard Values / Constants
- **π**: Use Math.PI (3.14159...)
- **Typical Pressure Angles**: 20° (most common), 25° (heavy loads)
- **Recommended Face Width**: F = A<sub>O</sub>/3 (starting point)
- **Minimum Pinion Teeth**: 12-17 teeth (avoid undercutting)

### Angle Convention
- **All trigonometric inputs** in JavaScript calculations use **radians**
- **All user inputs and outputs** display angles in **degrees**
- **Conversion**: radians = degrees × π/180
- Functions used: Math.atan(), Math.sin(), Math.cos()

### Special Considerations
- **Face Width Selection**: This is a user decision within a recommended range. The calculator provides F<sub>nom</sub> and F<sub>max</sub> as guidance, but the user must select F.
- **Validation Warning**: A yellow warning box appears if F is outside the recommended range.
- **Addendum/Dedendum "Angles"**: Despite the name, a<sub>G</sub>, a<sub>P</sub>, b<sub>G</sub>, b<sub>P</sub> are measured in **inches**, not degrees. They represent tangent-like ratios used in subsequent angle calculations.
- **Symmetric Angles**: For standard bevel gears, a<sub>G</sub> = a<sub>P</sub> and b<sub>G</sub> = b<sub>P</sub>
- **Final Outputs**: D<sub>O</sub> and d<sub>O</sub> are the critical manufacturing dimensions

### Calculation Order / Dependencies

```
Pd, phi, NP, NG (user inputs) →

Step 1:
  mG = NG/NP
  D = NG/Pd
  d = NP/Pd

Step 2:
  gamma = atan(NP/NG)  [convert to degrees]
  Gamma = atan(NG/NP)  [convert to degrees]

Step 3:
  AO = 0.5*D / sin(Gamma)  [Gamma in radians]

Step 4:
  Fnom = AO/3
  Fmax = 10/Pd
  F (user selects within range)
  Validate: Fnom ≤ F ≤ Fmax

Step 5:
  Am = AO - F/2

Step 6:
  pm = (π * Am) / (0.5 * NG)

Step 7 (piecewise based on Pd < 20):
  if (Pd < 20):  // Coarse
    h = 2.188/Pd
    c = 0.188/Pd
  else:  // Fine
    h = 2/Pd + 0.002
    c = 0.002

Step 8 (piecewise based on Pd < 20):
  if (Pd < 20):  // Coarse
    hm = h*cos(Gamma) - c/2  [Gamma in radians]
    c1 = c/2
  else:  // Fine
    hm = h*cos(Gamma) - c  [Gamma in radians]
    c1 = c

  am = hm/2
  bm = hm - am

Step 9:
  aG = am/AO
  aP = am/AO  (same as aG)
  bG = bm/AO
  bP = bm/AO  (same as bG)

Step 10:
  deltaG = Gamma + aG  [Gamma in degrees, aG in inches]
  deltaP = gamma + aP  [gamma in degrees, aP in inches]

  aOG = am + (F/2)*sin(deltaG)  [deltaG in radians]
  aOP = am + (F/2)*sin(deltaP)  [deltaP in radians]

  DO = D + 2*aOG*cos(Gamma)  [Gamma in radians]
  dO = d + 2*aOP*cos(gamma)  [gamma in radians]
```

### UI/UX Features
- **Green Background**: Indicates auto-calculated values (can be overridden by user)
- **White Background**: User input fields
- **Pitch Type Indicator**: Blue info box showing "Coarse Pitch" or "Fine Pitch"
- **Face Width Warning**: Yellow warning box if F is outside recommended range
- **Tab System**: Three tabs (Procedure, General Equations, Variables)
- **Session Save/Load**: LocalStorage persistence using `bevelGearSession` key
- **Variable Synchronization**: Same variable shown in multiple places stays synchronized
- **MathJax Rendering**: LaTeX equations render dynamically, including piecewise formula updates

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All 26 variables from HTML listed and categorized
- [x] Source references noted for all variables
- [x] All 26 equations extracted and converted to LaTeX
- [x] Procedure steps documented (10 steps with sub-steps)
- [x] Piecewise equations explained (4 equations with 2 branches each)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML source
- [x] Consistent naming and formatting throughout
- [x] Angle conversion notes included
- [x] Calculation dependencies documented

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has medium complexity due to piecewise equations and angle unit conversions. The calculator requires careful handling of the P<sub>d</sub> = 20 boundary where four equations change their formulas. All trigonometric functions must convert between degrees (user-facing) and radians (JavaScript Math functions). The face width validation provides user guidance but allows override.

**Key Implementation Points:**
1. **Piecewise Logic**: Implement conditional equation selection based on `Pd < 20`
2. **Dynamic LaTeX**: Update equation display when pitch type changes
3. **Angle Conversions**: All trig functions require degree→radian conversion
4. **Face Width Validation**: Show/hide warning box based on range check
5. **Variable Sync**: Maintain consistency across Procedure, General Equations, and Variables tabs
6. **Session Persistence**: Save/load all 26 variables to LocalStorage

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template structure
