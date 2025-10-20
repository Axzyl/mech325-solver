# Calculator Specification: Flat Belt Drive Design (Shigley Method)

---

## 1. Metadata

- **Calculator Name:** Flat Belt Drive Design Calculator (Shigley Method)
- **Source PDF:** Mech_325_Bible_Mech-flat_belts.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Mech-flat_belts.pdf`
- **Description:** Calculates flat belt drive parameters including belt geometry, tensions, forces, transmitted power, and safety factors following the Shigley methodology.
- **Complexity:** High
- **Tabs to Include:**
  - [x] Procedure (4-step process)
  - [x] General Equations (17+ equations)
  - [x] Variables (30+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 4-step numbered procedure with multiple sub-calculations
- **General Equations Tab**: Contains 17+ equations that benefit from having a reference section
- **Variables Tab**: 30+ variables require central management and overview

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| C | Center Distance | Center to center distance between pulleys | in | User Input | - | No | Starting value |
| d | Driving Pulley Diameter | Diameter of driving (input) pulley | in | User Input | - | No | Smaller pulley |
| D | Driven Pulley Diameter | Diameter of driven (output) pulley | in | User Input | - | No | Larger pulley |
| n | Rotational Speed | Speed of driving pulley | rpm | User Input | - | No | - |
| H<sub>nom</sub> | Nominal Horsepower | Input power requirement | hp | User Input | - | No | Starting value |
| b | Belt Width | Width of the belt | in | User Input / Table | Table 17-2 | Maybe | From material table or given |
| t | Belt Thickness | Thickness of the belt | in | User Input / Table | Table 17-2 | Maybe | From material table or given |
| Belt Material | Belt Material Type | Material selection | - | Dropdown | Table 17-2 | No | Leather, Polyamide, Urethane |
| w | Weight per Foot | Weight of one foot of belt | lbf/ft | User Input / Equation | Table 17-2 or EQ3 | Maybe | From table or calculated |
| f | Coefficient of Friction | Static friction coefficient | - | User Input / Table | Table 17-2 | Maybe | From material table |
| γ | Weight Density | Specific weight of belt material | lbf/in³ | User Input / Table | Table 17-2 | Maybe | For calculating w |
| VR | Velocity Ratio | Speed ratio between pulleys | - | Equation | EQ1 | No | VR = D/d |
| V | Belt Speed | Linear speed of belt | ft/min | Equation | EQ2 | No | V = πdn/12 |
| φ<sub>d</sub> | Driving Wrap Angle | Contact angle on driving pulley | rad | Equation | EQ4 | No | Open belt configuration |
| φ<sub>D</sub> | Driven Wrap Angle | Contact angle on driven pulley | rad | Equation | EQ5 | No | Open belt configuration |
| L | Belt Length | Total length of belt | in | Equation | EQ6 | No | For open belt |
| K<sub>s</sub> | Service Factor | Load type adjustment factor | - | User Input | - | No | Usually given in problem |
| n<sub>d</sub> | Design Factor | Safety/design factor | - | User Input | - | No | Usually given in problem |
| H<sub>d</sub> | Design Horsepower | Adjusted power for design | hp | Equation | EQ7 | No | H<sub>d</sub> = H<sub>nom</sub>K<sub>s</sub>n<sub>d</sub> |
| T | Required Torque | Torque at driving pulley | lbf·in | Equation | EQ8 | No | T = 63025H<sub>d</sub>/n |
| F<sub>c</sub> | Centrifugal Force | Centrifugal tension in belt | lbf | Equation | EQ9 | No | Due to rotation |
| F<sub>a</sub> | Allowed Tension | Manufacturer's allowed tension per inch width | lbf/in | Table | Table 17-2 | Maybe | From material table |
| C<sub>p</sub> | Pulley Correction Factor | Small pulley diameter correction | - | Table / User Input | Table 17-4 | Maybe | From table or 1.0 for urethane |
| C<sub>v</sub> | Velocity Correction Factor | Speed correction factor | - | Graph / User Input | Figure (Cv graph) | Maybe | From graph or 1.0 for urethane/polyamide |
| F<sub>1a</sub> | Allowable Tight Side Tension | Maximum tension on tight side | lbf | Equation | EQ10 | No | F<sub>1a</sub> = bF<sub>a</sub>C<sub>p</sub>C<sub>v</sub> |
| F<sub>1a</sub>-F<sub>2</sub> | Tension Difference | Difference between tight and slack | lbf | Equation | EQ11 | No | = 2T/d |
| F<sub>2</sub> | Slack Side Tension | Tension on slack side of belt | lbf | Equation | EQ12 | No | F<sub>2</sub> = F<sub>1a</sub> - 2T/d |
| F<sub>i</sub> | Initial Tension | Static tension before operation | lbf | Equation | EQ13 | No | F<sub>i</sub> = (F<sub>1a</sub>+F<sub>2</sub>)/2 - F<sub>c</sub> |
| H<sub>a</sub> | Transmitted Power | Actual transmitted horsepower | hp | Equation | EQ14 | No | Should equal H<sub>d</sub> |
| n<sub>fs</sub> | Safety Factor | Final safety factor | - | Equation | EQ15 | No | n<sub>fs</sub> = H<sub>a</sub>/(H<sub>nom</sub>K<sub>s</sub>) |
| f' | Developed Friction | Actual friction coefficient developed | - | Equation | EQ16 | No | Must be < f |
| dip | Belt Dip | Sag of belt at midspan | in | Equation | EQ17 | No | Belt deflection |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `VR = \frac{D}{d}` | Velocity Ratio | D, d | VR | Speed ratio |
| EQ2 | `V = \frac{\pi d n}{12}` | Belt Speed | d, n | V | Linear speed in ft/min |
| EQ3 | `w = 12\gamma b t` | Weight per Foot | γ, b, t | w | If not from table |
| EQ4 | `\phi_d = \pi - 2\sin^{-1}\left(\frac{D-d}{2C}\right)` | Driving Wrap Angle | D, d, C | φ<sub>d</sub> | Open belt, result in radians |
| EQ5 | `\phi_D = \pi + 2\sin^{-1}\left(\frac{D-d}{2C}\right)` | Driven Wrap Angle | D, d, C | φ<sub>D</sub> | Open belt, result in radians |
| EQ6 | `L = \sqrt{4C^2 - (D-d)^2} + \frac{1}{2}(D\phi_D + d\phi_d)` | Belt Length | C, D, d, φ<sub>D</sub>, φ<sub>d</sub> | L | Open belt configuration |
| EQ7 | `H_d = H_{nom} K_s n_d` | Design Horsepower | H<sub>nom</sub>, K<sub>s</sub>, n<sub>d</sub> | H<sub>d</sub> | Adjusted power |
| EQ8 | `T = \frac{63025 H_d}{n}` | Required Torque | H<sub>d</sub>, n | T | Torque at driving pulley |
| EQ9 | `F_c = \frac{w}{g}\left(\frac{V}{60}\right)^2` | Centrifugal Force | w, V | F<sub>c</sub> | g = 32.2 ft/s² |
| EQ10 | `F_{1a} = b F_a C_p C_v` | Allowable Tight Side Tension | b, F<sub>a</sub>, C<sub>p</sub>, C<sub>v</sub> | F<sub>1a</sub> | Maximum tension |
| EQ11 | `F_{1a} - F_2 = \frac{2T}{d}` | Tension Difference | T, d | F<sub>1a</sub>-F<sub>2</sub> | From torque |
| EQ12 | `F_2 = F_{1a} - \frac{2T}{d}` | Slack Side Tension | F<sub>1a</sub>, T, d | F<sub>2</sub> | Lower tension side |
| EQ13 | `F_i = \frac{F_{1a} + F_2}{2} - F_c` | Initial Tension | F<sub>1a</sub>, F<sub>2</sub>, F<sub>c</sub> | F<sub>i</sub> | Static pre-tension |
| EQ14 | `H_a = \frac{(F_{1a} - F_2)V}{33000}` | Transmitted Power | F<sub>1a</sub>, F<sub>2</sub>, V | H<sub>a</sub> | Should equal H<sub>d</sub> |
| EQ15 | `n_{fs} = \frac{H_a}{H_{nom}K_s}` | Safety Factor | H<sub>a</sub>, H<sub>nom</sub>, K<sub>s</sub> | n<sub>fs</sub> | Final safety check |
| EQ16 | `f' = \frac{1}{\phi_d}\ln\left(\frac{F_{1a} - F_c}{F_2 - F_c}\right)` | Developed Friction | φ<sub>d</sub>, F<sub>1a</sub>, F<sub>c</sub>, F<sub>2</sub> | f' | Must be < f |
| EQ17 | `dip = \frac{12(C/12)^2 w}{8F_i}` | Belt Dip | C, w, F<sub>i</sub> | dip | Belt sag |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1a | Input Basic Parameters | User Input | Enter C [in], d [in], D [in], n [rpm], H<sub>nom</sub> [hp] | Primary inputs |
| 1b | Input/Select Belt Properties | User Input / Table | Enter or select: b [in], t [in], Belt Material, w [lbf/ft], f | From Table 17-2 or given |
| 1c | Input Design Factors | User Input | Enter K<sub>s</sub>, n<sub>d</sub> | Usually given in problem |
| 2a | Calculate Velocity Ratio | Equation | VR = D/d (EQ1) | Speed ratio |
| 2b | Calculate Belt Speed | Equation | V = πdn/12 (EQ2) | Linear speed |
| 2c | Calculate Weight per Foot | Equation | w = 12γbt (EQ3) if not from table | Optional if w is given |
| 2d | Calculate Wrap Angles | Equation | φ<sub>d</sub> = π - 2sin⁻¹((D-d)/(2C)) (EQ4), φ<sub>D</sub> = π + 2sin⁻¹((D-d)/(2C)) (EQ5) | Open belt |
| 2e | Calculate Belt Length | Equation | L = √(4C² - (D-d)²) + ½(Dφ<sub>D</sub> + dφ<sub>d</sub>) (EQ6) | Total length |
| 2f | Calculate Design Horsepower | Equation | H<sub>d</sub> = H<sub>nom</sub>K<sub>s</sub>n<sub>d</sub> (EQ7) | Adjusted power |
| 3a | Calculate Torque | Equation | T = 63025H<sub>d</sub>/n (EQ8) | At driving pulley |
| 3b | Calculate Centrifugal Force | Equation | F<sub>c</sub> = (w/g)(V/60)² (EQ9), g = 32.2 ft/s² | Centrifugal tension |
| 3c | Lookup/Input Correction Factors | Table / User Input | F<sub>a</sub> from Table 17-2, C<sub>p</sub> from Table 17-4, C<sub>v</sub> from graph or = 1.0 | Material properties |
| 3d | Calculate Allowable Tight Side Tension | Equation | F<sub>1a</sub> = bF<sub>a</sub>C<sub>p</sub>C<sub>v</sub> (EQ10) | Maximum tension |
| 3e | Calculate Tension Difference | Equation | F<sub>1a</sub> - F<sub>2</sub> = 2T/d (EQ11) | From torque |
| 3f | Calculate Slack Side Tension | Equation | F<sub>2</sub> = F<sub>1a</sub> - 2T/d (EQ12) | Lower tension |
| 3g | Calculate Initial Tension | Equation | F<sub>i</sub> = (F<sub>1a</sub>+F<sub>2</sub>)/2 - F<sub>c</sub> (EQ13) | Pre-tension needed |
| 4a | Calculate Transmitted Power | Equation | H<sub>a</sub> = (F<sub>1a</sub>-F<sub>2</sub>)V/33000 (EQ14) | Should equal H<sub>d</sub> |
| 4b | Calculate Safety Factor | Equation | n<sub>fs</sub> = H<sub>a</sub>/(H<sub>nom</sub>K<sub>s</sub>) (EQ15) | Final check |
| 4c | Check Friction Development | Equation | f' = (1/φ<sub>d</sub>)ln((F<sub>1a</sub>-F<sub>c</sub>)/(F<sub>2</sub>-F<sub>c</sub>)) (EQ16) | Must have f' < f |
| 4d | Calculate Belt Dip | Equation | dip = 12(C/12)²w/(8F<sub>i</sub>) (EQ17) | Belt sag |

---

## 5. Additional Notes

### Validation Rules
- **VR** should typically be between 1 and 6
- **φ<sub>d</sub>** (driving wrap angle) should be > 150° (2.618 rad) for good power transmission
- **f' < f** is critical - if not satisfied, design must be changed
- **H<sub>a</sub>** should approximately equal **H<sub>d</sub>**
- **n<sub>fs</sub>** should be > 1.0 (typically 1.1-1.3)

### Standard Values / Constants
- **g** = 32.2 ft/s² (gravitational acceleration)
- **Cp** = 1.0 for urethane belts
- **Cv** = 1.0 for urethane and polyamide belts
- **Belt configurations**: Open belt (most common) or crossed belt (equations differ)

### Dropdown Options

**Belt Material:**
- Leather (1 ply, 2 ply)
- Polyamide (F-0, F-1, F-2, A-2, A-3, A-4, A-5)
- Urethane (various thicknesses)
- Round (special case)

### Future Enhancement Opportunities
- **Table 17-2** could be made interactive for automatic material property selection
- **Table 17-4** (Cp values) could be made interactive based on material and pulley diameter
- **Cv graph** could be digitized for automatic lookup
- **Automatic belt selection** based on H<sub>d</sub>, V, and pulley sizes

### Special Considerations
- **Crossed belt configuration**: Different equations for wrap angles and belt length
  - φ = π + 2sin⁻¹((D+d)/(2C)) for both pulleys
  - L = √(4C² - (D+d)²) + ½(D+d)φ
- **Material selection**: Urethane is simpler (Cp=1, Cv=1), Leather requires more lookups
- **Units**: Careful with unit conversions (inches, feet, ft/min, etc.)
- **Iteration**: If f' ≥ f, must increase wrap angle (larger C or smaller pulleys) or change material
- **Rounding**: Use appropriate precision (typically 2-4 decimal places)

### Calculation Order / Dependencies

```
[User Inputs: C, d, D, n, Hnom, b, t, material, w, f, Ks, nd] →
VR = D/d →
V = πdn/12 →
w = 12γbt (if needed) →
φd = π - 2sin⁻¹((D-d)/(2C)) →
φD = π + 2sin⁻¹((D-d)/(2C)) →
L = √(4C² - (D-d)²) + ½(DφD + dφd) →
Hd = Hnom·Ks·nd →
T = 63025Hd/n →
Fc = (w/g)(V/60)² →
[Lookup: Fa, Cp, Cv from tables] →
F1a = b·Fa·Cp·Cv →
F1a - F2 = 2T/d →
F2 = F1a - 2T/d →
Fi = (F1a + F2)/2 - Fc →
Ha = (F1a - F2)V/33000 →
nfs = Ha/(Hnom·Ks) →
f' = (1/φd)ln((F1a - Fc)/(F2 - Fc)) →
dip = 12(C/12)²w/(8Fi) →
Check: f' < f
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

**Specification Author:** Claude Code
**Date Created:** 2025-10-20
**Last Updated:** 2025-10-20

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
