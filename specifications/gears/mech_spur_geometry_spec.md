# Calculator Specification: Spur Gear Geometry (Mott's Design)

---

## 1. Metadata

- **Calculator Name:** Spur Gear Spec Calculator
- **Source PDF:** spur_design_spec.pdf
- **PDF Location:** `documents/gears/spur_design_spec.pdf`
- **Description:** Calculates spur gear geometric specifications using Mott's design methodology. Includes pitch diameters, tooth dimensions, various diameters (outside, root, base), tooth thickness, center distance, and wrap angles. Handles both coarse pitch (Pd < 20) and fine pitch (Pd >= 20) specifications with automatic piecewise equation selection.
- **Complexity:** Medium (piecewise equations, bidirectional sync)
- **Tabs to Include:**
  - [x] Procedure (9-step process)
  - [x] General Equations (17 equations)
  - [x] Variables (19 variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Clear 9-step sequential procedure for calculating spur gear specifications
- **General Equations Tab**: Contains general reference equations organized by category (pitch relationships, tooth dimensions, diameters)
- **Variables Tab**: 19 variables with bidirectional editing capability for flexible calculations

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs (Primary)** |
| P<sub>d</sub> | Diametral Pitch | Number of teeth per inch of pitch diameter | teeth/in | User Input | - | No | Primary input, determines coarse/fine pitch |
| φ | Pressure Angle | Tooth pressure angle | deg | User Input | - | No | Standard values: 14.5°, 20°, 25° |
| N<sub>P</sub> | Number of Teeth (Pinion) | Tooth count on pinion | teeth | User Input | - | No | Must be positive integer |
| N<sub>G</sub> | Number of Teeth (Gear) | Tooth count on gear | teeth | User Input | - | No | Must be positive integer |
| m | Module | Metric pitch measurement | mm | User Input | - | No | Alternative to P<sub>d</sub>: m = 25.4/P<sub>d</sub> |
| **Auto-Calculated (Step 1: Pitch Diameters)** |
| D<sub>P</sub> | Pitch Diameter (Pinion) | Pitch circle diameter of pinion | in | Equation | EQ1 | No | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Pitch Diameter (Gear) | Pitch circle diameter of gear | in | Equation | EQ2 | No | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> |
| **Auto-Calculated (Step 2: Circular Pitch)** |
| p | Circular Pitch | Arc distance between adjacent teeth | in | Equation | EQ3 | No | p = π/P<sub>d</sub> |
| **Auto-Calculated (Step 3: Tooth Dimensions - Piecewise)** |
| a | Addendum | Height of tooth above pitch circle | in | Equation | EQ4 | No | a = 1.00/P<sub>d</sub> |
| b | Dedendum | Depth of tooth below pitch circle | in | Equation | EQ5 (Piecewise) | No | Coarse: 1.25/P<sub>d</sub>, Fine: 1.2/P<sub>d</sub> + 0.002 |
| c | Clearance | Gap between addendum and dedendum | in | Equation | EQ6 (Piecewise) | No | Coarse: 0.25/P<sub>d</sub>, Fine: 0.2/P<sub>d</sub> + 0.002 |
| **Auto-Calculated (Step 4: Outside Diameters)** |
| D<sub>oP</sub> | Outside Diameter (Pinion) | Tip diameter of pinion | in | Equation | EQ7 | No | D<sub>oP</sub> = (N<sub>P</sub> + 2)/P<sub>d</sub> |
| D<sub>oG</sub> | Outside Diameter (Gear) | Tip diameter of gear | in | Equation | EQ8 | No | D<sub>oG</sub> = (N<sub>G</sub> + 2)/P<sub>d</sub> |
| **Auto-Calculated (Step 5: Root Diameters)** |
| D<sub>RP</sub> | Root Diameter (Pinion) | Root circle diameter of pinion | in | Equation | EQ9 | No | D<sub>RP</sub> = D<sub>P</sub> - 2b |
| D<sub>RG</sub> | Root Diameter (Gear) | Root circle diameter of gear | in | Equation | EQ10 | No | D<sub>RG</sub> = D<sub>G</sub> - 2b |
| **Auto-Calculated (Step 6: Depths)** |
| h<sub>t</sub> | Whole Depth | Total tooth depth | in | Equation | EQ11 | No | h<sub>t</sub> = a + b |
| h<sub>k</sub> | Working Depth | Depth of tooth engagement | in | Equation | EQ12 | No | h<sub>k</sub> = 2a |
| **Auto-Calculated (Step 7: Tooth Thickness)** |
| t | Tooth Thickness | Arc thickness at pitch circle | in | Equation | EQ13 | No | t = π/(2P<sub>d</sub>) |
| **Auto-Calculated (Step 8: Center Distance)** |
| C | Center Distance | Distance between gear centers | in | Equation | EQ14 | No | C = (N<sub>G</sub> + N<sub>P</sub>)/(2P<sub>d</sub>) |
| **Auto-Calculated (Step 9: Base Circle Diameters)** |
| D<sub>bP</sub> | Base Diameter (Pinion) | Base circle diameter of pinion | in | Equation | EQ15 | No | D<sub>bP</sub> = D<sub>P</sub> cos(φ) |
| D<sub>bG</sub> | Base Diameter (Gear) | Base circle diameter of gear | in | Equation | EQ16 | No | D<sub>bG</sub> = D<sub>G</sub> cos(φ) |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Pitch Relationships** |
| EQ1 | `D_P = \frac{N_P}{P_d}` | Pitch Diameter (Pinion) | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | - |
| EQ2 | `D_G = \frac{N_G}{P_d}` | Pitch Diameter (Gear) | N<sub>G</sub>, P<sub>d</sub> | D<sub>G</sub> | - |
| EQ3 | `p = \frac{\pi}{P_d}` | Circular Pitch | P<sub>d</sub> | p | - |
| **Module Conversion** |
| EQ0 | `m = \frac{25.4}{P_d}` | Module from Diametral Pitch | P<sub>d</sub> | m | Metric conversion |
| **Tooth Dimensions (Addendum - Universal)** |
| EQ4 | `a = \frac{1.00}{P_d}` | Addendum | P<sub>d</sub> | a | Same for coarse and fine pitch |
| **Tooth Dimensions (Dedendum - Piecewise)** |
| EQ5a | `b = \frac{1.25}{P_d}` | Dedendum (Coarse Pitch) | P<sub>d</sub> | b | When P<sub>d</sub> < 20 |
| EQ5b | `b = \frac{1.2}{P_d} + 0.002` | Dedendum (Fine Pitch) | P<sub>d</sub> | b | When P<sub>d</sub> >= 20 |
| **Tooth Dimensions (Clearance - Piecewise)** |
| EQ6a | `c = \frac{0.25}{P_d}` | Clearance (Coarse Pitch) | P<sub>d</sub> | c | When P<sub>d</sub> < 20 |
| EQ6b | `c = \frac{0.2}{P_d} + 0.002` | Clearance (Fine Pitch) | P<sub>d</sub> | c | When P<sub>d</sub> >= 20 |
| **Outside Diameters** |
| EQ7 | `D_{oP} = \frac{N_P + 2}{P_d}` | Outside Diameter (Pinion) | N<sub>P</sub>, P<sub>d</sub> | D<sub>oP</sub> | Equivalent to D<sub>P</sub> + 2a |
| EQ8 | `D_{oG} = \frac{N_G + 2}{P_d}` | Outside Diameter (Gear) | N<sub>G</sub>, P<sub>d</sub> | D<sub>oG</sub> | Equivalent to D<sub>G</sub> + 2a |
| **Root Diameters** |
| EQ9 | `D_{RP} = D_P - 2b` | Root Diameter (Pinion) | D<sub>P</sub>, b | D<sub>RP</sub> | - |
| EQ10 | `D_{RG} = D_G - 2b` | Root Diameter (Gear) | D<sub>G</sub>, b | D<sub>RG</sub> | - |
| **Tooth Depths** |
| EQ11 | `h_t = a + b` | Whole Depth | a, b | h<sub>t</sub> | - |
| EQ12 | `h_k = 2a` | Working Depth | a | h<sub>k</sub> | - |
| **Tooth Thickness** |
| EQ13 | `t = \frac{\pi}{2P_d}` | Tooth Thickness | P<sub>d</sub> | t | Arc thickness at pitch circle |
| **Center Distance** |
| EQ14 | `C = \frac{N_G + N_P}{2P_d}` | Center Distance | N<sub>G</sub>, N<sub>P</sub>, P<sub>d</sub> | C | Distance between gear centers |
| **Base Circle Diameters** |
| EQ15 | `D_{bP} = D_P \cos(\phi)` | Base Diameter (Pinion) | D<sub>P</sub>, φ | D<sub>bP</sub> | φ in degrees |
| EQ16 | `D_{bG} = D_G \cos(\phi)` | Base Diameter (Gear) | D<sub>G</sub>, φ | D<sub>bG</sub> | φ in degrees |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Given Information** |
| - | Enter Primary Inputs | User Input | P<sub>d</sub> [teeth/in], φ [deg], N<sub>P</sub> [teeth], N<sub>G</sub> [teeth] | All editable throughout |
| - | Optional: Enter Module | User Input | m [mm] converts to P<sub>d</sub> via m = 25.4/P<sub>d</sub> | Bidirectional sync |
| **Step 1** |
| 1a | Calculate Pitch Diameter (Pinion) | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> (EQ1) | Auto-calculated, green background |
| 1b | Calculate Pitch Diameter (Gear) | Equation | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ2) | Auto-calculated, green background |
| **Step 2** |
| 2 | Calculate Circular Pitch | Equation | p = π/P<sub>d</sub> (EQ3) | Auto-calculated |
| **Step 3** |
| 3a | Determine Pitch Type | Auto-Detection | If P<sub>d</sub> < 20: Coarse; If P<sub>d</sub> >= 20: Fine | Displays indicator |
| 3b | Calculate Addendum | Equation | a = 1.00/P<sub>d</sub> (EQ4) | Universal formula |
| 3c | Calculate Dedendum | Equation (Piecewise) | Coarse: b = 1.25/P<sub>d</sub> (EQ5a); Fine: b = 1.2/P<sub>d</sub> + 0.002 (EQ5b) | Formula updates dynamically |
| 3d | Calculate Clearance | Equation (Piecewise) | Coarse: c = 0.25/P<sub>d</sub> (EQ6a); Fine: c = 0.2/P<sub>d</sub> + 0.002 (EQ6b) | Formula updates dynamically |
| **Step 4** |
| 4a | Calculate Outside Diameter (Pinion) | Equation | D<sub>oP</sub> = (N<sub>P</sub> + 2)/P<sub>d</sub> (EQ7) | Auto-calculated |
| 4b | Calculate Outside Diameter (Gear) | Equation | D<sub>oG</sub> = (N<sub>G</sub> + 2)/P<sub>d</sub> (EQ8) | Auto-calculated |
| **Step 5** |
| 5a | Calculate Root Diameter (Pinion) | Equation | D<sub>RP</sub> = D<sub>P</sub> - 2b (EQ9) | Auto-calculated |
| 5b | Calculate Root Diameter (Gear) | Equation | D<sub>RG</sub> = D<sub>G</sub> - 2b (EQ10) | Auto-calculated |
| **Step 6** |
| 6a | Calculate Whole Depth | Equation | h<sub>t</sub> = a + b (EQ11) | Auto-calculated |
| 6b | Calculate Working Depth | Equation | h<sub>k</sub> = 2a (EQ12) | Auto-calculated |
| **Step 7** |
| 7 | Calculate Tooth Thickness | Equation | t = π/(2P<sub>d</sub>) (EQ13) | Auto-calculated |
| **Step 8** |
| 8 | Calculate Center Distance | Equation | C = (N<sub>G</sub> + N<sub>P</sub>)/(2P<sub>d</sub>) (EQ14) | Auto-calculated |
| **Step 9** |
| 9a | Calculate Base Diameter (Pinion) | Equation | D<sub>bP</sub> = D<sub>P</sub> cos(φ) (EQ15) | Auto-calculated, φ in degrees |
| 9b | Calculate Base Diameter (Gear) | Equation | D<sub>bG</sub> = D<sub>G</sub> cos(φ) (EQ16) | Auto-calculated, φ in degrees |
| **Completion** |
| - | Display Success Message | Visual Feedback | "All calculations complete!" | Green success box |

---

## 5. Additional Notes

### Validation Rules
- **Positive Integers**: N<sub>P</sub>, N<sub>G</sub> must be positive integers (step = 1)
- **Positive Values**: P<sub>d</sub>, m, φ must be greater than 0
- **Angle Range**: φ typically 0° < φ < 90° (practical range: 14.5°, 20°, 25°)
- **Module Bidirectional**: m and P<sub>d</sub> automatically sync: m = 25.4/P<sub>d</sub>
- **Pitch Type**: Automatically determined by P<sub>d</sub> < 20 (coarse) or P<sub>d</sub> >= 20 (fine)

### Standard Values / Constants
- **π**: Math.PI (3.14159...)
- **Standard Pressure Angles**: 14.5°, 20°, 25° (20° most common)
- **Standard Diametral Pitches (Coarse)**: 2, 2.5, 3, 4, 5, 6, 8, 10, 12, 16
- **Standard Diametral Pitches (Fine)**: 20, 24, 32, 40, 48, 64, 80, 96, 120, 150, 180
- **Module Conversion Factor**: 25.4 (mm/in)

### Piecewise Equations Details

**Pitch Type Determination:**
- **Coarse Pitch**: P<sub>d</sub> < 20 teeth/in
- **Fine Pitch**: P<sub>d</sub> >= 20 teeth/in

**Affected Equations:**

| Variable | Coarse Pitch (P<sub>d</sub> < 20) | Fine Pitch (P<sub>d</sub> >= 20) |
|----------|----------------------------------|----------------------------------|
| **Dedendum (b)** | b = 1.25/P<sub>d</sub> | b = 1.2/P<sub>d</sub> + 0.002 |
| **Clearance (c)** | c = 0.25/P<sub>d</sub> | c = 0.2/P<sub>d</sub> + 0.002 |
| **Addendum (a)** | a = 1.00/P<sub>d</sub> | a = 1.00/P<sub>d</sub> (same) |

**UI Behavior:**
- Pitch type indicator displays above Step 3 equations with blue background
- LaTeX formulas update dynamically in equation blocks
- Text labels update to show correct formula
- All dependent calculations (D<sub>RP</sub>, D<sub>RG</sub>, h<sub>t</sub>) recalculate automatically

### Bidirectional Sync System

**Variable Instance Mapping:**
- Each variable (e.g., P<sub>d</sub>) appears multiple times across equations
- All instances sync bidirectionally when any instance is edited
- User can edit ANY field (given inputs or calculated values) to override
- `userValues` Set tracks which values are user-entered vs computed
- Computed values show green background (`computed` class)

**Example: P<sub>d</sub> appears in:**
- Given Information section (id: `Pd`)
- Variables tab (id: `var_Pd`)
- Step 1a equation (id: `Pd_eq1`)
- Step 1b equation (id: `Pd_eq2`)
- Step 2 equation (id: `Pd_eq3`)
- Steps 3, 4, 7, 8 equations (ids: `Pd_eq4` through `Pd_eq14`)

**Sync Function:** `syncAllInstances(changedId)` updates all instances when any is edited

### Session Persistence

**LocalStorage:**
- All input values saved to browser LocalStorage
- `userValues` Set saved to track input source
- Auto-loads on page refresh
- Save button: Manual save with confirmation
- Reset button: Clears all values and storage with confirmation

**Storage Key:** `'spurGearSession'`

### Tab System

**Three Tabs:**
1. **Procedure** (default): Step-by-step calculation workflow
2. **General Equations**: Reference section with categorized equations
3. **Variables**: Master variable table with editable inputs

**Tab Features:**
- MathJax re-renders when switching to equation tabs
- All tabs share same data model (bidirectional sync)
- Active tab highlights with blue underline

### Future Enhancement Opportunities
- **Gear Ratio**: Add VR = N<sub>G</sub>/N<sub>P</sub> calculation
- **Speed Calculations**: Add n<sub>2</sub> = n<sub>1</sub>/VR for output speed
- **Face Width**: Add recommended face width guidelines (typically 3p to 5p)
- **Material Selection**: Add strength-based material recommendations
- **Tooth Form**: Add involute profile visualization
- **Interference Check**: Add undercut detection (minimum teeth for pressure angle)
- **Metric Units**: Add full metric mode (module-based calculations)
- **Preset Library**: Standard gear pairs (e.g., 20 deg, 8 pitch, 18/72 teeth)
- **Export**: PDF report generation with gear drawing

### Special Considerations
- **Piecewise Threshold**: Crossing P<sub>d</sub> = 20 triggers formula change and visual update
- **Angle Units**: φ input in degrees, converted to radians for cosine calculations
- **Precision**: All outputs display 4 decimal places (`.toFixed(4)`)
- **Computed Class**: Visual indicator (green background) for auto-calculated values
- **Override Capability**: User can edit any calculated value to override, marking it as user input
- **MathJax Rendering**: LaTeX equations render asynchronously, typesetPromise used for updates

### Calculation Order / Dependencies

```
User Inputs: Pd, phi, Np, Ng (or m converts to Pd)
↓
[Step 1] DP = Np/Pd, DG = Ng/Pd
↓
[Step 2] p = π/Pd
↓
[Step 3] Determine: Pd < 20 (coarse) or Pd >= 20 (fine)
         a = 1.00/Pd
         Coarse: b = 1.25/Pd, c = 0.25/Pd
         Fine: b = 1.2/Pd + 0.002, c = 0.2/Pd + 0.002
↓
[Step 4] DoP = (Np + 2)/Pd, DoG = (Ng + 2)/Pd
↓
[Step 5] DRP = DP - 2b, DRG = DG - 2b (depends on Step 1 and Step 3)
↓
[Step 6] ht = a + b, hk = 2a (depends on Step 3)
↓
[Step 7] t = π/(2Pd)
↓
[Step 8] C = (Ng + Np)/(2Pd)
↓
[Step 9] DbP = DP × cos(phi), DbG = DG × cos(phi) (depends on Step 1)
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (9 steps)
- [x] Piecewise equations explained (dedendum and clearance)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML file
- [x] Consistent naming and formatting throughout
- [x] Bidirectional sync system documented
- [x] Session persistence documented
- [x] Calculation dependencies mapped

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has medium complexity with sophisticated features already implemented in the HTML:
- Piecewise equation handling with dynamic formula updates
- Bidirectional variable synchronization across all instances
- Session persistence via LocalStorage
- Three-tab interface with MathJax rendering
- User override capability for all calculated values
- Visual indicators for computed values (green background)

The specification is based on a fully functional HTML implementation at `C:\data\vscode\m325_solver_v2\calculators\gears\mech_spur_geometry.html`.

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template
