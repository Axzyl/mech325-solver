# Calculator Specification: Helical Gear Geometry

---

## 1. Metadata

- **Calculator Name:** Helical Gear Geometry Calculator
- **Source PDF:** helical_design_spec.pdf
- **PDF Location:** `documents/gears/helical_design_spec.pdf`
- **Description:** Calculates helical gear geometry parameters including pitch relationships (transverse and normal), pressure angles, circular pitches, pitch diameter, and axial pitch metrics. Supports bidirectional conversion between transverse and normal diametral pitch and pressure angles.
- **Complexity:** Medium (bidirectional equations, angle conversions)
- **Tabs to Include:**
  - [x] Procedure (6-step sequential process)
  - [x] General Equations (7 primary equations)
  - [x] Variables (11 variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Clear 6-step calculation sequence for helical gear design
- **General Equations Tab**: Contains all helical gear equations for reference
- **Variables Tab**: 11 variables with different source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **Primary Inputs** |
| N | Number of Teeth | Number of teeth on gear | teeth | User Input | - | No | Integer value |
| F | Face Width | Width of gear face | in | User Input | - | No | Must be > 0 |
| ψ | Helix Angle | Angle of helical teeth | deg | User Input | - | No | 0° < ψ < 90° |
| **Bidirectional Pitch Inputs (Provide ONE)** |
| P<sub>d</sub> | Transverse Diametral Pitch | Diametral pitch in transverse plane | teeth/in | User Input | - | No | Provide ONE: Pd or Pnd |
| P<sub>nd</sub> | Normal Diametral Pitch | Diametral pitch in normal plane | teeth/in | User Input | - | No | Provide ONE: Pd or Pnd |
| **Bidirectional Pressure Angle Inputs (Provide ONE)** |
| φ<sub>t</sub> | Transverse Pressure Angle | Pressure angle in transverse plane | deg | User Input | - | No | Provide ONE: φt or φn |
| φ<sub>n</sub> | Normal Pressure Angle | Pressure angle in normal plane | deg | User Input | - | No | Provide ONE: φt or φn |
| **Auto-Calculated Outputs** |
| p<sub>t</sub> | Transverse Circular Pitch | Circular pitch in transverse plane | in | Equation | EQ2 | No | - |
| p<sub>n</sub> | Normal Circular Pitch | Circular pitch in normal plane | in | Equation | EQ3 | No | - |
| p<sub>x</sub> | Axial Pitch | Pitch measured along axis | in | Equation | EQ4 | No | - |
| D | Pitch Diameter | Pitch circle diameter | in | Equation | EQ5 | No | - |
| N<sub>ax</sub> | Number of Axial Pitches | Number of axial pitches in face width | - | Equation | EQ7 | No | Dimensionless |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1a | `P_{nd} = \frac{P_d}{\cos(\psi)}` | Normal Pitch from Transverse | P<sub>d</sub>, ψ | P<sub>nd</sub> | Bidirectional |
| EQ1b | `P_d = P_{nd} \cos(\psi)` | Transverse Pitch from Normal | P<sub>nd</sub>, ψ | P<sub>d</sub> | Bidirectional |
| EQ2 | `p_t = \frac{\pi}{P_d}` | Transverse Circular Pitch | P<sub>d</sub> | p<sub>t</sub> | - |
| EQ3 | `p_n = p_t \cos(\psi)` | Normal Circular Pitch | p<sub>t</sub>, ψ | p<sub>n</sub> | - |
| EQ4 | `p_x = \frac{p_t}{\tan(\psi)}` | Axial Pitch | p<sub>t</sub>, ψ | p<sub>x</sub> | - |
| EQ5 | `D = \frac{N}{P_d}` | Pitch Diameter | N, P<sub>d</sub> | D | - |
| EQ6a | `\phi_n = \tan^{-1}(\tan(\phi_t) \cos(\psi))` | Normal Pressure Angle from Transverse | φ<sub>t</sub>, ψ | φ<sub>n</sub> | Bidirectional |
| EQ6b | `\phi_t = \tan^{-1}\left(\frac{\tan(\phi_n)}{\cos(\psi)}\right)` | Transverse Pressure Angle from Normal | φ<sub>n</sub>, ψ | φ<sub>t</sub> | Bidirectional |
| EQ7 | `N_{ax} = \frac{F}{p_x}` | Number of Axial Pitches | F, p<sub>x</sub> | N<sub>ax</sub> | - |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Given Information** |
| - | Enter Primary Inputs | User Input | N [teeth], F [in], ψ [deg] | Always required |
| - | Enter ONE Diametral Pitch | User Input | P<sub>d</sub> [teeth/in] OR P<sub>nd</sub> [teeth/in] | Mutually exclusive |
| - | Enter ONE Pressure Angle | User Input | φ<sub>t</sub> [deg] OR φ<sub>n</sub> [deg] | Mutually exclusive |
| **Step 1** | Convert Between Normal and Transverse Diametral Pitch | Equation | Use EQ1a or EQ1b based on user input | Bidirectional calculation |
| **Step 2** | Calculate Circular Pitches | Equation | p<sub>t</sub> = π/P<sub>d</sub> (EQ2) | Auto-calculated |
| - | Calculate Normal Circular Pitch | Equation | p<sub>n</sub> = p<sub>t</sub> cos(ψ) (EQ3) | Auto-calculated |
| **Step 3** | Calculate Axial Pitch | Equation | p<sub>x</sub> = p<sub>t</sub>/tan(ψ) (EQ4) | Auto-calculated |
| **Step 4** | Calculate Pitch Diameter | Equation | D = N/P<sub>d</sub> (EQ5) | Auto-calculated |
| **Step 5** | Convert Between Pressure Angles | Equation | Use EQ6a or EQ6b based on user input | Bidirectional calculation |
| **Step 6** | Calculate Number of Axial Pitches | Equation | N<sub>ax</sub> = F/p<sub>x</sub> (EQ7) | Auto-calculated |

---

## 5. Additional Notes

### Validation Rules
- **Positive Values**: N > 0 (integer), F > 0, P<sub>d</sub> > 0, P<sub>nd</sub> > 0
- **Angle Ranges**:
  - 0° < ψ < 90° (helix angle)
  - 0° < φ<sub>t</sub> < 90° (transverse pressure angle)
  - 0° < φ<sub>n</sub> < 90° (normal pressure angle)
- **Integer Values**: N must be a positive integer
- **Bidirectional Logic**:
  - User must provide EITHER P<sub>d</sub> OR P<sub>nd</sub> (not both)
  - User must provide EITHER φ<sub>t</sub> OR φ<sub>n</sub> (not both)
  - The unprovided value is calculated from the provided one

### Standard Values / Constants
- **π**: Use Math.PI (3.14159...)
- **Common Helix Angles**: 15°, 20°, 23°, 30°, 45°
- **Common Pressure Angles**:
  - Normal: 14.5°, 20°, 25°
  - Transverse: Depends on helix angle and normal pressure angle

### Bidirectional Calculation Logic

**Diametral Pitch:**
- **User provides P<sub>d</sub>**: Calculate P<sub>nd</sub> = P<sub>d</sub>/cos(ψ)
- **User provides P<sub>nd</sub>**: Calculate P<sub>d</sub> = P<sub>nd</sub> × cos(ψ)
- **Direction Indicator**: Show which value is being calculated (visual feedback)

**Pressure Angle:**
- **User provides φ<sub>t</sub>**: Calculate φ<sub>n</sub> = arctan(tan(φ<sub>t</sub>) × cos(ψ))
- **User provides φ<sub>n</sub>**: Calculate φ<sub>t</sub> = arctan(tan(φ<sub>n</sub>)/cos(ψ))
- **Direction Indicator**: Show which value is being calculated (visual feedback)

### Angle Conversion Notes
- All angles are entered and displayed in degrees
- Internal calculations use radians
- Conversion: radians = degrees × π/180
- Inverse conversion: degrees = radians × 180/π

### Variable Instance Synchronization

The calculator implements a sophisticated sync system where each variable can appear in multiple locations:
- Given Information section
- Individual equation blocks (Step 1-6)
- General Equations tab
- Variables tab

**Sync Map (from JavaScript):**
```
N: ['N', 'var_N', 'N_eq4', 'N_gen']
F: ['F', 'var_F', 'F_eq6', 'F_gen']
psi: ['psi', 'var_psi', 'psi_eq1', 'psi_eq2', 'psi_eq3', 'psi_eq5', 'psi_gen', 'psi_gen2', 'psi_gen3', 'psi_gen4']
Pd: ['Pd', 'var_Pd', 'Pd_eq1', 'Pd_eq2', 'Pd_eq4', 'Pd_gen', 'Pd_gen2', 'Pd_gen3']
Pnd: ['Pnd', 'var_Pnd', 'Pnd_eq1', 'Pnd_gen']
phi_t: ['phi_t', 'var_phi_t', 'phi_t_eq5', 'phi_t_gen']
phi_n: ['phi_n', 'var_phi_n', 'phi_n_eq5', 'phi_n_gen']
pt: ['pt', 'var_pt', 'pt_eq2', 'pt_eq3', 'pt_gen', 'pt_gen2', 'pt_gen3']
pn: ['pn', 'var_pn', 'pn_gen']
px: ['px', 'var_px', 'px_eq6', 'px_gen', 'px_gen2']
D: ['D', 'var_D', 'D_gen']
Nax: ['Nax', 'var_Nax', 'Nax_gen']
```

All instances of the same variable are synchronized bidirectionally - editing any instance updates all others.

### User vs Computed Value Tracking

**State Variables:**
- `userValues`: Set containing variable IDs that user has directly edited
- `userProvidedPd`: Tracks which pitch was provided (true = Pd, false = Pnd, null = neither)
- `userProvidedPhi`: Tracks which pressure angle was provided (true = φt, false = φn, null = neither)

**Computed Value Styling:**
- Computed values have green background (`background: #e8f5e9`)
- Computed values have green border (`border-color: #4caf50`)
- User can override any computed value by editing it
- Once overridden, value is marked as user-provided and won't be auto-updated

### Future Enhancement Opportunities
- **Preset Helix Angles**: Dropdown for common values (15°, 20°, 23°, 30°, 45°)
- **Preset Pressure Angles**: Dropdown for standard values (14.5°, 20°, 25°)
- **Virtual Number of Teeth**: Add calculation for equivalent spur gear
- **Tooth Geometry**: Extend to calculate addendum, dedendum, whole depth
- **Contact Ratio**: Calculate contact ratio in transverse and axial planes
- **Strength Calculations**: Link to stress analysis calculators

### Special Considerations
- **Numerical Precision**: All calculations display 4 decimal places
- **Angle Convention**: All angles in degrees for user interface, radians for calculations
- **Division by Zero**: Prevent ψ = 0° (pure spur gear) to avoid tan(0) = 0 in EQ4
- **Small Helix Angles**: For ψ < 5°, warn user that gear approaches spur geometry
- **Large Helix Angles**: For ψ > 45°, warn about increased axial forces
- **Session Persistence**: Uses LocalStorage to save/load all values and state

### Calculation Order / Dependencies

```
[User Inputs]
N, F, ψ (always required)
Pd OR Pnd (one required) →
phi_t OR phi_n (one required) →

[Step 1: Pitch Conversion]
IF Pd provided:
  Pnd = Pd / cos(ψ)
ELSE IF Pnd provided:
  Pd = Pnd × cos(ψ)
→

[Step 2: Circular Pitches]
pt = π / Pd →
pn = pt × cos(ψ) →

[Step 3: Axial Pitch]
px = pt / tan(ψ) →

[Step 4: Pitch Diameter]
D = N / Pd →

[Step 5: Pressure Angle Conversion]
IF phi_t provided:
  phi_n = arctan(tan(phi_t) × cos(ψ))
ELSE IF phi_n provided:
  phi_t = arctan(tan(phi_n) / cos(ψ))
→

[Step 6: Axial Metrics]
Nax = F / px
```

### Session Storage Schema

**LocalStorage Key:** `helicalGearSession`

**Saved Data Structure:**
```javascript
{
  "N": "value",
  "F": "value",
  "psi": "value",
  "Pd": "value",
  "Pnd": "value",
  "phi_t": "value",
  "phi_n": "value",
  "pt": "value",
  "pn": "value",
  "px": "value",
  "D": "value",
  "Nax": "value",
  // ... all other input IDs ...
  "_userValues": ["N", "F", "psi", ...],  // Array of user-edited IDs
  "_userProvidedPd": true/false/null,      // Which pitch was provided
  "_userProvidedPhi": true/false/null      // Which angle was provided
}
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Bidirectional equations explained
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout
- [x] Sync system documented
- [x] State management documented

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This specification documents an already-implemented calculator. The HTML file at `calculators/gears/mech_helical_geometry.html` is fully functional and includes:
- Bidirectional pitch and pressure angle conversion
- Real-time calculation as user types
- Global variable synchronization across all tabs
- Session persistence via LocalStorage
- Visual feedback for computed vs user-entered values
- Direction indicators for bidirectional calculations

**Complexity Assessment:** Medium
- **Moderate**: Bidirectional equation logic requires careful state management
- **Moderate**: Multiple angle conversions between degrees and radians
- **Low**: No table lookups or iterative calculations required
- **Moderate**: Global synchronization system with 12 variables across 40+ input instances

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md
Last Updated: 2025-10-22
