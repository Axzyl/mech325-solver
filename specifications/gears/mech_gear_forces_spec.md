# Calculator Specification: Gear Forces Calculator

---

## 1. Metadata

- **Calculator Name:** Gear Forces Calculator
- **Source PDF:** gear_forces.pdf
- **PDF Location:** `documents/gears/gear_forces.pdf`
- **Description:** Calculates forces acting on spur, helical, and bevel gears based on Mott's gear force formulas. Determines tangential, radial, axial, and normal forces given power, speed, and geometric parameters.
- **Complexity:** Low-Medium (straightforward trigonometric calculations)
- **Tabs to Include:**
  - [x] Spur Gears
  - [x] Helical Gears
  - [x] Bevel Gears

**Rationale for Tab Structure:**
- **Spur Gears Tab**: Contains 4 force equations (T, W<sub>t</sub>, W<sub>r</sub>, W<sub>n</sub>)
- **Helical Gears Tab**: Adds pitch line speed and axial force (5 equations)
- **Bevel Gears Tab**: Uses mean radii and cone angles (6 equations)
- Each gear type has distinct force characteristics, justifying separate tabs

---

## 2. Variables

### Spur Gears Variables

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P | Power | Transmitted power | hp | User Input | - | No | Primary input |
| n | Speed | Rotational speed | rpm | User Input | - | No | Shaft speed |
| D | Pitch Diameter | Pitch circle diameter | in | User Input | - | No | Gear geometry |
| φ | Pressure Angle | Pressure angle | deg | User Input | - | No | Typically 20° or 25° |
| **Auto-Calculated** |
| T | Torque | Transmitted torque | lb·in | Equation | EQ-S1 | No | T = 63000P/n |
| W<sub>t</sub> | Tangential Force | Force along pitch line | lbf | Equation | EQ-S2 | No | Primary force component |
| W<sub>r</sub> | Radial Force | Force toward center | lbf | Equation | EQ-S3 | No | Separating force |
| W<sub>n</sub> | Normal Force | Total tooth force | lbf | Equation | EQ-S4 | No | Resultant force |

### Helical Gears Variables

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P | Power | Transmitted power | hp | User Input | - | No | Primary input |
| n | Speed | Rotational speed | rpm | User Input | - | No | Shaft speed |
| D | Pitch Diameter | Pitch circle diameter | in | User Input | - | No | Gear geometry |
| φ<sub>t</sub> | Transverse Pressure Angle | Pressure angle in transverse plane | deg | User Input | - | No | Related to normal pressure angle |
| ψ | Helix Angle | Helix angle | deg | User Input | - | No | Typically 15°-45° |
| **Auto-Calculated** |
| T | Torque | Transmitted torque | lb·in | Equation | EQ-H1 | No | T = 63000P/n |
| v<sub>t</sub> | Pitch Line Speed | Tangential velocity | ft/min | Equation | EQ-H2 | No | For reference |
| W<sub>t</sub> | Tangential Force | Force along pitch line | lbf | Equation | EQ-H3 | No | Primary force component |
| W<sub>r</sub> | Radial Force | Force toward center | lbf | Equation | EQ-H4 | No | Separating force |
| W<sub>x</sub> | Axial Force | Thrust force along axis | lbf | Equation | EQ-H5 | No | Requires thrust bearing |

### Bevel Gears Variables

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P | Power | Transmitted power | hp | User Input | - | No | Primary input |
| n<sub>P</sub> | Pinion Speed | Pinion rotational speed | rpm | User Input | - | No | Faster shaft |
| d | Pinion Pitch Diameter | Pinion pitch diameter at large end | in | User Input | - | No | Smaller gear |
| D | Gear Pitch Diameter | Gear pitch diameter at large end | in | User Input | - | No | Larger gear |
| F | Face Width | Axial face width | in | User Input | - | No | Typically ≤ cone distance/3 |
| φ | Pressure Angle | Pressure angle | deg | User Input | - | No | Typically 20° |
| γ | Pinion Cone Angle | Pinion pitch cone angle | deg | User Input | - | No | From geometry |
| Γ | Gear Cone Angle | Gear pitch cone angle | deg | User Input | - | No | γ + Γ = 90° for 90° shafts |
| **Auto-Calculated** |
| r<sub>m</sub> | Pinion Mean Radius | Mean radius of pinion | in | Equation | EQ-B1 | No | At middle of face width |
| R<sub>m</sub> | Gear Mean Radius | Mean radius of gear | in | Equation | EQ-B2 | No | At middle of face width |
| W<sub>t</sub> | Tangential Force | Force along pitch line | lbf | Equation | EQ-B3 | No | At mean radius |
| W<sub>r</sub> | Radial Force | Force toward center | lbf | Equation | EQ-B4 | No | Separating force |
| W<sub>x</sub> | Axial Force | Thrust force along axis | lbf | Equation | EQ-B5 | No | Axial thrust component |

---

## 3. Equations

### Spur Gears Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ-S1 | `T = \frac{63000P}{n} = \frac{W_t D}{2}` | Torque | P, n | T | Also relates W<sub>t</sub> and D |
| EQ-S2 | `W_t = \frac{126000P}{nD}` | Tangential Force | P, n, D | W<sub>t</sub> | Primary force component |
| EQ-S3 | `W_r = W_t \tan(\phi)` | Radial Force | W<sub>t</sub>, φ | W<sub>r</sub> | Separating force |
| EQ-S4 | `W_n = \frac{W_t}{\cos(\phi)} = \sqrt{W_t^2 + W_r^2}` | Normal Force | W<sub>t</sub>, φ (or W<sub>r</sub>) | W<sub>n</sub> | Two equivalent formulas |

### Helical Gears Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ-H1 | `T = \frac{63000P}{n} = \frac{W_t D}{2}` | Torque | P, n | T | Same as spur gears |
| EQ-H2 | `v_t = \frac{\pi D n}{12}` | Pitch Line Speed | D, n | v<sub>t</sub> | For reference/validation |
| EQ-H3 | `W_t = \frac{126000P}{nD}` | Tangential Force | P, n, D | W<sub>t</sub> | Same as spur gears |
| EQ-H4 | `W_r = W_t \tan(\phi_t)` | Radial Force | W<sub>t</sub>, φ<sub>t</sub> | W<sub>r</sub> | Uses transverse pressure angle |
| EQ-H5 | `W_x = W_t \tan(\psi)` | Axial Force | W<sub>t</sub>, ψ | W<sub>x</sub> | Thrust along shaft axis |

### Bevel Gears Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ-B1 | `r_m = \frac{d}{2} - \frac{F}{2} \sin(\gamma)` | Pinion Mean Radius | d, F, γ | r<sub>m</sub> | At middle of face |
| EQ-B2 | `R_m = \frac{D}{2} - \frac{F}{2} \sin(\Gamma)` | Gear Mean Radius | D, F, Γ | R<sub>m</sub> | At middle of face |
| EQ-B3 | `W_t = \frac{63000P}{r_{m,p} n_p}` | Tangential Force | P, r<sub>m</sub>, n<sub>P</sub> | W<sub>t</sub> | Recommended formula |
| EQ-B4 | `W_r = W_t \tan(\phi) \cos(\Gamma)` | Radial Force | W<sub>t</sub>, φ, Γ | W<sub>r</sub> | Alternative: cos(γ) |
| EQ-B5 | `W_x = W_t \tan(\phi) \sin(\Gamma)` | Axial Force | W<sub>t</sub>, φ, Γ | W<sub>x</sub> | Alternative: sin(γ) |

---

## 4. Procedure Steps

### Spur Gears Procedure

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Input Power | User Input | Enter P [hp] | Transmitted power |
| 2 | Input Speed | User Input | Enter n [rpm] | Rotational speed |
| 3 | Input Pitch Diameter | User Input | Enter D [in] | Gear geometry |
| 4 | Input Pressure Angle | User Input | Enter φ [deg] | Typically 20° or 25° |
| 5 | Calculate Torque | Equation | T = 63000P/n (EQ-S1) | Auto-calculated |
| 6 | Calculate Tangential Force | Equation | W<sub>t</sub> = 126000P/(nD) (EQ-S2) | Auto-calculated |
| 7 | Calculate Radial Force | Equation | W<sub>r</sub> = W<sub>t</sub> tan(φ) (EQ-S3) | Auto-calculated |
| 8 | Calculate Normal Force | Equation | W<sub>n</sub> = √(W<sub>t</sub>² + W<sub>r</sub>²) (EQ-S4) | Auto-calculated |

### Helical Gears Procedure

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Input Power | User Input | Enter P [hp] | Transmitted power |
| 2 | Input Speed | User Input | Enter n [rpm] | Rotational speed |
| 3 | Input Pitch Diameter | User Input | Enter D [in] | Gear geometry |
| 4 | Input Transverse Pressure Angle | User Input | Enter φ<sub>t</sub> [deg] | In transverse plane |
| 5 | Input Helix Angle | User Input | Enter ψ [deg] | Typically 15°-45° |
| 6 | Calculate Torque | Equation | T = 63000P/n (EQ-H1) | Auto-calculated |
| 7 | Calculate Pitch Line Speed | Equation | v<sub>t</sub> = πDn/12 (EQ-H2) | Auto-calculated |
| 8 | Calculate Tangential Force | Equation | W<sub>t</sub> = 126000P/(nD) (EQ-H3) | Auto-calculated |
| 9 | Calculate Radial Force | Equation | W<sub>r</sub> = W<sub>t</sub> tan(φ<sub>t</sub>) (EQ-H4) | Auto-calculated |
| 10 | Calculate Axial Force | Equation | W<sub>x</sub> = W<sub>t</sub> tan(ψ) (EQ-H5) | Auto-calculated |

### Bevel Gears Procedure

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Input Power | User Input | Enter P [hp] | Transmitted power |
| 2 | Input Pinion Speed | User Input | Enter n<sub>P</sub> [rpm] | Faster shaft |
| 3 | Input Pinion Pitch Diameter | User Input | Enter d [in] | Smaller gear |
| 4 | Input Gear Pitch Diameter | User Input | Enter D [in] | Larger gear |
| 5 | Input Face Width | User Input | Enter F [in] | Axial dimension |
| 6 | Input Pressure Angle | User Input | Enter φ [deg] | Typically 20° |
| 7 | Input Pinion Cone Angle | User Input | Enter γ [deg] | From geometry calculations |
| 8 | Input Gear Cone Angle | User Input | Enter Γ [deg] | γ + Γ = 90° for right-angle drive |
| 9 | Calculate Pinion Mean Radius | Equation | r<sub>m</sub> = d/2 - (F/2)sin(γ) (EQ-B1) | Auto-calculated |
| 10 | Calculate Gear Mean Radius | Equation | R<sub>m</sub> = D/2 - (F/2)sin(Γ) (EQ-B2) | Auto-calculated |
| 11 | Calculate Tangential Force | Equation | W<sub>t</sub> = 63000P/(r<sub>m</sub>n<sub>P</sub>) (EQ-B3) | Auto-calculated |
| 12 | Calculate Radial Force | Equation | W<sub>r</sub> = W<sub>t</sub> tan(φ) cos(Γ) (EQ-B4) | Auto-calculated |
| 13 | Calculate Axial Force | Equation | W<sub>x</sub> = W<sub>t</sub> tan(φ) sin(Γ) (EQ-B5) | Auto-calculated |

---

## 5. Additional Notes

### Validation Rules
- **Positive Values**: P, n (or n<sub>P</sub>), D, d, F must all be > 0
- **Angle Ranges**:
  - Pressure angle φ: typically 14.5°, 20°, or 25°
  - Transverse pressure angle φ<sub>t</sub>: typically 15°-25°
  - Helix angle ψ: typically 15°-45°
  - Cone angles γ, Γ: 0° < γ, Γ < 90°, often γ + Γ = 90° for perpendicular shafts
- **Non-zero Speed**: Speed values must be > 0 to avoid division by zero
- **Angle Units**: All angles input in degrees, converted to radians for trigonometric functions
- **Face Width**: For bevel gears, F typically ≤ cone distance/3

### Standard Values / Constants
- **Common Pressure Angles (Spur/Helical)**: 14.5°, 20°, 25°
- **Typical Helix Angle (Helical)**: 15°-45° (often 30°)
- **Right-Angle Bevel Gears**: γ + Γ = 90°
- **π**: Use Math.PI for calculations

### Dropdown Options

**Note**: Current implementation has no dropdowns. All values are user inputs.

**Future Enhancement - Standard Pressure Angles:**
- 14.5° (older standard)
- 20° (most common)
- 25° (high-strength applications)

### Future Enhancement Opportunities
- **Pressure Angle Dropdown**: Pre-populate with standard values (14.5°, 20°, 25°)
- **Force Direction Diagrams**: Visual representation of force components
- **Unit Conversion**: Support for metric units (kW, mm, N)
- **Bearing Load Calculator**: Calculate bearing loads from force components
- **Shaft Angle Validation**: For bevel gears, validate γ + Γ equals shaft angle
- **Alternative Formulas**: PDF shows multiple formulas for bevel W<sub>t</sub>, could offer selection
- **Force Vector Visualization**: 3D representation of force components

### Special Considerations

**Spur Gears:**
- Normal force W<sub>n</sub> can be calculated two ways (shown in EQ-S4)
- Forces act in plane of rotation only (no axial component)
- Radial force causes bearing separating loads

**Helical Gears:**
- Axial force W<sub>x</sub> requires thrust bearing
- Hand of helix determines axial force direction
- Transverse pressure angle φ<sub>t</sub> differs from normal pressure angle φ<sub>n</sub>
- Relationship: tan(φ<sub>n</sub>) = tan(φ<sub>t</sub>) cos(ψ)

**Bevel Gears:**
- Forces calculated at mean radius, not pitch radius
- PDF shows multiple formulas for W<sub>t</sub> (implemented recommended version)
- Radial force can use either cos(γ) or cos(Γ) (equivalent for 90° shaft angle)
- Axial force can use either sin(γ) or sin(Γ) (equivalent for 90° shaft angle)
- Both radial and axial components exist
- Mean radius accounts for conical geometry

**Angle Conversions:**
- All angles input in degrees
- JavaScript: radians = degrees × (Math.PI / 180)
- Trigonometric functions (sin, cos, tan, asin) use radians

**Precision:**
- Forces displayed to 4 decimal places
- Sufficient for engineering calculations

### Calculation Order / Dependencies

**Spur Gears:**
```
P, n, D, φ (user inputs) →
T = 63000P/n →
W_t = 126000P/(nD) →
W_r = W_t × tan(φ) →
W_n = √(W_t² + W_r²)
```

**Helical Gears:**
```
P, n, D, φ_t, ψ (user inputs) →
T = 63000P/n →
v_t = πDn/12 →
W_t = 126000P/(nD) →
W_r = W_t × tan(φ_t) →
W_x = W_t × tan(ψ)
```

**Bevel Gears:**
```
P, n_P, d, D, F, φ, γ, Γ (user inputs) →
r_m = d/2 - (F/2)sin(γ) →
R_m = D/2 - (F/2)sin(Γ) →
W_t = 63000P/(r_m × n_P) →
W_r = W_t × tan(φ) × cos(Γ) →
W_x = W_t × tan(φ) × sin(Γ)
```

### Implementation Features

**Current Implementation:**
- Three separate tabs for gear types
- Real-time calculation on input change
- Green background indicates computed values
- User can override any computed value
- Session save/load via LocalStorage
- Reset all functionality
- Input synchronization for duplicate fields (spur tab only)
- MathJax LaTeX rendering for equations

**Input Tracking:**
- `userValues` Set tracks which fields are user-entered
- Computed values marked with 'computed' class (green background)
- User can manually override any computed value
- Manual override prevents auto-recalculation of that field

**Field Synchronization (Spur Only):**
- Main input fields: P, n, D, φ, W<sub>t</sub>
- Duplicated in equation blocks with suffixes: _eq1, _eq2
- `syncInput()` function keeps duplicates synchronized
- Prevents circular updates using userValues tracking

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF/HTML listed and categorized
- [x] Source references noted for all variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented for all three gear types
- [x] Piecewise equations explained (N/A - no piecewise equations)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This calculator is already fully implemented in HTML. This specification serves as documentation for the existing implementation. The calculator has low-medium complexity with straightforward trigonometric calculations. No tables or graphs required - all inputs are user-provided or computed.

**Key Implementation Strengths:**
- Clean separation of three gear types via tabs
- Real-time calculation with visual feedback
- User override capability for all computed values
- Session persistence
- No external dependencies beyond MathJax

**Potential Improvements:**
- Add pressure angle dropdown with standard values
- Add force diagram visualization
- Extend field synchronization to helical and bevel tabs
- Add unit conversion (metric/imperial)
- Validate bevel gear shaft angle (γ + Γ)

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0 (Based on fizz_chains_spec.md)
Last Updated: 2025-10-22
