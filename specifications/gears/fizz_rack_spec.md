# Calculator Specification: Rack and Pinion Design (Fizz Guide)

---

## 1. Metadata

- **Calculator Name:** Rack and Pinion Calculator (Fizz Guide)
- **Source PDF:** fizz_rack.pdf
- **PDF Location:** `documents/gears/fizz_rack.pdf`
- **HTML File:** `calculators/gears/fizz_rack.html`
- **Description:** Calculates rack and pinion kinematics including pitch diameter, back-to-centerline distance, rack velocity, travel time, and number of revolutions using the Fizz Guide 6-step methodology.
- **Complexity:** Low (straightforward sequential calculations, one manual lookup)
- **Tabs to Include:**
  - [x] Single Calculator View (6 sequential steps)
  - [ ] General Equations (not needed - equations embedded in procedure)
  - [ ] Variables (not needed - only 10 variables)

**Rationale for Tab Structure:**
- **Single View**: Simple 6-step linear process with only 10 variables - no benefit from separate tabs
- **No Equations Tab**: All 5 equations are straightforward and shown inline with steps
- **No Variables Tab**: Small variable count, all variables appear in sequential order

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P<sub>D</sub> | Diametral Pitch | Tooth size metric | teeth/in | User Input | - | No | Standard tooth sizing |
| N<sub>P</sub> | Number of Teeth on Pinion | Pinion teeth count | teeth | User Input | - | No | Integer value |
| n<sub>P</sub> | Pinion Speed | Rotational speed of pinion | rpm | User Input | - | No | Driving speed |
| s<sub>rack</sub> | Rack Travel Distance | Linear travel distance | ft | User Input | - | No | Total travel required |
| **Manual Lookups** |
| B | Pitch to Back Distance | Distance from pitch line to back of rack | in | Table | Table 8-10 in PDF | Maybe | Based on P<sub>D</sub> |
| **Auto-Calculated** |
| D<sub>p</sub> | Pitch Diameter | Diameter of pinion at pitch circle | in | Equation | EQ1 | No | D<sub>p</sub> = N<sub>P</sub>/P<sub>D</sub> |
| B-C | Back to Centerline | Distance from rack back to pinion center | in | Equation | EQ2 | No | B-C = B + D<sub>p</sub>/2 |
| V<sub>rack</sub> | Velocity of Rack | Linear speed of rack | ft/min | Equation | EQ3 | No | π/6 × (D<sub>P</sub> × n<sub>P</sub>)/2 |
| t | Time to Move Distance | Time to travel s<sub>rack</sub> | s | Equation | EQ4 | No | 60 × s<sub>rack</sub>/V<sub>rack</sub> |
| θ<sub>P</sub> | Number of Revolutions | Pinion revolutions for travel | rev | Equation | EQ5 | No | (6/π) × 2s<sub>rack</sub>/D<sub>P</sub> |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `D_p = \frac{N_P}{P_D}` | Pitch Diameter | N<sub>P</sub>, P<sub>D</sub> | D<sub>p</sub> | Standard gear formula |
| EQ2 | `B\text{-}C = B + \frac{D_p}{2}` | Back to Centerline | B, D<sub>p</sub> | B-C | Mounting dimension |
| EQ3 | `V_{rack} = \frac{\pi}{6} \times \frac{D_P \times n_P}{2}` | Velocity of Rack | D<sub>p</sub>, n<sub>P</sub> | V<sub>rack</sub> | Units: ft/min, includes unit conversion |
| EQ4 | `t = 60 \times \frac{s_{rack}}{V_{rack}}` | Time to Move Distance | s<sub>rack</sub>, V<sub>rack</sub> | t | Converts minutes to seconds |
| EQ5 | `\theta_P = \frac{6}{\pi} \times \frac{2 \times s_{rack}}{D_P}` | Number of Revolutions | s<sub>rack</sub>, D<sub>P</sub> | θ<sub>P</sub> | Total revolutions for travel |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 0 | Given Information | User Input | Enter P<sub>D</sub> [teeth/in], N<sub>P</sub> [teeth], n<sub>P</sub> [rpm], s<sub>rack</sub> [ft] | Initial parameters |
| 1 | Calculate Pitch Diameter | Equation | D<sub>p</sub> = N<sub>P</sub>/P<sub>D</sub> (EQ1) | Auto-calculated |
| 2 | Lookup Pitch to Back Distance | Table | Find B from Table 8-10 based on P<sub>D</sub> | Manual yellow input |
| 3 | Calculate Back to Centerline | Equation | B-C = B + D<sub>p</sub>/2 (EQ2) | Mounting dimension |
| 4 | Calculate Rack Velocity | Equation | V<sub>rack</sub> = (π/6) × (D<sub>P</sub> × n<sub>P</sub>)/2 (EQ3) | Linear speed in ft/min |
| 5 | Calculate Time to Move Distance | Equation | t = 60 × s<sub>rack</sub>/V<sub>rack</sub> (EQ4) | Time in seconds |
| 6 | Calculate Number of Revolutions | Equation | θ<sub>P</sub> = (6/π) × 2s<sub>rack</sub>/D<sub>P</sub> (EQ5) | Total revolutions |

---

## 5. Additional Notes

### Validation Rules
- **Integer Values**: N<sub>P</sub> must be an integer (positive)
- **Positive Values**: P<sub>D</sub>, n<sub>P</sub>, s<sub>rack</sub>, B must be > 0
- **Division by Zero**: V<sub>rack</sub> must be > 0 for time calculation

### Standard Values / Constants
- **π**: Use 3.14159 or Math.PI
- **Unit Conversions**:
  - Diameter in inches, speeds in rpm/ft/min
  - Time conversion: 60 seconds per minute

### Dropdown Options
None - this calculator does not use dropdowns.

### Future Enhancement Opportunities
- **Table 8-10**: Implement dropdown for P<sub>D</sub> with automatic B lookup
- **Rack Length Calculator**: Add total rack length required based on travel distance
- **Force Analysis**: Extend to include tangential and normal forces
- **Material Selection**: Add rack and pinion material recommendations

### Special Considerations
- **Table 8-10 Lookup**: The only manual lookup in this calculator. B values are specific to standard diametral pitches.
- **Unit Consistency**: Careful attention needed for inch/foot conversions in velocity and distance calculations
- **Formula Simplification**: EQ3 and EQ5 contain π/6 and 6/π factors - these come from unit conversions (inches to feet) and circumference relationships

### Calculation Order / Dependencies

```
User Inputs: PD, NP, nP, srack →
Step 1: Dp = NP / PD →
Step 2: B (manual lookup from Table 8-10) →
Step 3: BC = B + Dp/2 →
Step 4: Vrack = (π/6) × (Dp × nP)/2 →
Step 5: t = 60 × srack / Vrack →
Step 6: thetaP = (6/π) × 2×srack / Dp
```

**No circular dependencies** - pure sequential calculation

### Implementation Details

**Variable Synchronization:**
The HTML implementation uses a global sync system with instance mapping:
- `varInstanceMap` tracks all instances of each variable across equation blocks
- `syncAllInstances()` propagates values when any instance is updated
- Example: `Dp` appears in equations 1, 3, 4, 6 → all instances sync

**Color Coding:**
- White background: User inputs
- Yellow background: Manual table lookups (B from Table 8-10)
- Green background: Auto-calculated values

**Session Persistence:**
- Uses LocalStorage key: `rack_fizz_session`
- Saves all input values and user/computed states
- Restores on page load

**Computed Field Logic:**
All equations check `!userValues.has(id)` before auto-calculating to prevent overwriting user edits.

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (N/A - none present)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This calculator is already implemented in `calculators/gears/fizz_rack.html`. This specification serves as documentation and reference for future maintenance or modifications. The calculator has low complexity with only one manual lookup (Table 8-10).

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0 (Based on fizz_chains_spec.md)
Last Updated: 2025-10-22

---

## 8. Table 8-10 Reference (Pitch to Back Distance)

For future interactive implementation, Table 8-10 values (example - verify with actual PDF):

| P<sub>D</sub> (teeth/in) | B (in) |
|--------------------------|--------|
| 2 | TBD |
| 4 | TBD |
| 6 | TBD |
| 8 | TBD |
| 10 | TBD |
| 12 | TBD |
| 16 | TBD |
| 20 | TBD |

**Note:** Actual values must be extracted from Table 8-10 in fizz_rack.pdf for accurate implementation.
