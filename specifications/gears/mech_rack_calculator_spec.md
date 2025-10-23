# Calculator Specification: Rack and Pinion Calculator

---

## 1. Metadata

- **Calculator Name:** Rack and Pinion Calculator
- **Source PDF:** rack.pdf
- **PDF Location:** `documents/gears/rack.pdf`
- **Description:** Calculates kinematics and geometry for rack and pinion systems, including pitch diameter, rack velocity, centerline distances, travel time, and number of revolutions.
- **Complexity:** Low-Medium (straightforward equations, minimal lookups)
- **Tabs to Include:**
  - [x] Procedure (6-step process)
  - [x] General Equations (3 general equations)
  - [x] Variables (13 variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Clear 6-step sequential procedure for rack and pinion design
- **General Equations Tab**: Contains 3 general equations for quick reference calculations
- **Variables Tab**: Central management of 13 variables for easy access and synchronization

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P<sub>d</sub> | Diametral Pitch | Pitch of pinion gear | teeth/in | User Input | - | No | Starting value |
| N<sub>P</sub> | Number of Teeth | Number of teeth on pinion | teeth | User Input | - | No | Positive integer |
| n<sub>P</sub> | Pinion Speed | Rotational speed of pinion | rpm | User Input | - | No | Rotation rate |
| s<sub>rack</sub> | Rack Travel Distance | Linear distance rack travels | ft | User Input | - | No | Optional input |
| B | Pitch to Back Distance | Distance from pitch line to back of rack | in | Manual Lookup | Table 8-10 | Maybe | From table |
| ρ | Revolutions | Number of pinion revolutions | rev | User Input | - | No | For general equation 3 |
| **Auto-Calculated (Procedure Steps)** |
| D<sub>P</sub> | Pitch Diameter | Pitch diameter of pinion | in | Equation | Step 1 | No | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> |
| B-C | Back to Centerline | Distance from back of rack to pinion centerline | in | Equation | Step 3 | No | B-C = B + D<sub>P</sub>/2 |
| V<sub>rack</sub> | Rack Velocity | Linear velocity of rack | ft/min | Equation | Step 4 | No | V<sub>rack</sub> = (π/6)(D<sub>P</sub>n<sub>P</sub>/2) |
| t | Time | Time to move rack distance | seconds | Equation | Step 5 | No | t = 60(s<sub>rack</sub>/V<sub>rack</sub>) |
| θ<sub>P</sub> | Number of Revolutions | Pinion revolutions for rack travel | rev | Equation | Step 6 | No | θ<sub>P</sub> = (6/π)(2s<sub>rack</sub>/D<sub>P</sub>) |
| **Auto-Calculated (General Equations)** |
| v<sub>t</sub> | Pitch Line Speed | Tangential velocity at pitch circle | ft/min | Equation | General EQ1 | No | v<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/2 |
| S<sub>rack</sub> | Rack Distance Traveled | Distance rack travels in ρ revolutions | ft | Equation | General EQ3 | No | S<sub>rack</sub> = πD<sub>P</sub>ρ/2 |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Procedure Equations** |
| Step 1 | `D_P = \frac{N_P}{P_d}` | Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | - |
| Step 3 | `B\text{-}C = B + \frac{D_P}{2}` | Back to Centerline Distance | B, D<sub>P</sub> | B-C | - |
| Step 4 | `V_{rack} = \frac{\pi}{6} \left(\frac{D_P n_P}{2}\right)` | Rack Velocity | D<sub>P</sub>, n<sub>P</sub> | V<sub>rack</sub> | Simplified from general eq |
| Step 5 | `t = 60 \left(\frac{s_{rack}}{V_{rack}}\right)` | Time to Move Distance | s<sub>rack</sub>, V<sub>rack</sub> | t | Converts minutes to seconds |
| Step 6 | `\theta_P = \frac{6}{\pi} \left(\frac{2 s_{rack}}{D_P}\right)` | Number of Revolutions | s<sub>rack</sub>, D<sub>P</sub> | θ<sub>P</sub> | - |
| **General Equations** |
| Gen EQ1 | `v_t = \frac{\pi D_P n_P}{2}` | Pitch Line Speed | D<sub>P</sub>, n<sub>P</sub> | v<sub>t</sub> | Tangential velocity |
| Gen EQ2 | `V_{rack} = \frac{\pi D_P n_P}{12}` | Speed of Rack | D<sub>P</sub>, n<sub>P</sub> | V<sub>rack</sub> | Alternative formulation |
| Gen EQ3 | `S_{rack} = \frac{\pi D_P \rho}{2}` | Distance Rack Travels | D<sub>P</sub>, ρ | S<sub>rack</sub> | For given revolutions |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| Given | Input Parameters | User Input | Enter P<sub>d</sub> [teeth/in], N<sub>P</sub> [teeth], n<sub>P</sub> [rpm], s<sub>rack</sub> [ft] (optional) | Initial values |
| 1 | Calculate Pitch Diameter | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> | Auto-calculated |
| 2 | Find Distance from Pitch Line to Back | Table Lookup | Look up B from Table 8-10 based on P<sub>d</sub> | Manual yellow input |
| 3 | Calculate Distance from Back to Centerline | Equation | B-C = B + D<sub>P</sub>/2 | Auto-calculated |
| 4 | Calculate Velocity of Rack | Equation | V<sub>rack</sub> = (π/6)(D<sub>P</sub>n<sub>P</sub>/2) | Auto-calculated |
| 5 | Calculate Time to Move Rack Distance | Equation | t = 60(s<sub>rack</sub>/V<sub>rack</sub>) | Auto-calculated (requires s<sub>rack</sub>) |
| 6 | Calculate Number of Revolutions | Equation | θ<sub>P</sub> = (6/π)(2s<sub>rack</sub>/D<sub>P</sub>) | Auto-calculated (requires s<sub>rack</sub>) |

---

## 5. Additional Notes

### Validation Rules
- **Positive Teeth**: N<sub>P</sub> must be a positive integer
- **Positive Pitch**: P<sub>d</sub> must be > 0
- **Positive Speed**: n<sub>P</sub> must be > 0
- **Optional Inputs**: s<sub>rack</sub> is optional; steps 5 and 6 only calculate if provided
- **Table Range**: Table 8-10 provides B values for P<sub>d</sub> = 4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 48, 64

### Standard Values / Constants
- **π**: Use 3.14159 or Math.PI
- **Recommended P<sub>d</sub> values**: Standard values from Table 8-10 (4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 48, 64)

### Table 8-10: Example Rack Specifications

| Diametral Pitch | Pitch Line to Back (B) (in) | Overall Thickness (in) | Face Width (in) | Nominal Length (ft) |
|-----------------|----------------------------|----------------------|----------------|-------------------|
| 64 | 0.109 | 0.125 | 0.125 | 2 |
| 48 | 0.104 | 0.125 | 0.125 | 2 |
| 32 | 0.156 | 0.187 | 0.187 | 4 |
| 24 | 0.208 | 0.250 | 0.25 | 4 |
| 20 | 0.450 | 0.500 | 0.5 | 6 |
| 16 | 0.688 | 0.750 | 0.75 | 6 |
| 12 | 0.917 | 1.000 | 1 | 6 |
| 10 | 1.150 | 1.250 | 1.25 | 6 |
| 8 | 1.375 | 1.500 | 1.5 | 6 |
| 6 | 1.333 | 1.500 | 2 | 6 |
| 5 | 1.300 | 1.500 | 2.5 | 6 |
| 4 | 1.750 | 2.000 | 3.5 | 6 |

### Future Enhancement Opportunities
- **Table 8-10 Dropdown**: Implement P<sub>d</sub> dropdown with automatic B value lookup
- **Unit Conversion**: Add metric units (mm, m/min) alongside imperial
- **Rack Length Calculation**: Add ability to calculate required rack length for given travel
- **Face Width Validation**: Cross-reference face width requirements from table
- **Multiple Rack Segments**: Calculate for racks made of multiple sections

### Special Considerations
- **Unit Consistency**: Equations mix inches (D<sub>P</sub>, B) and feet (s<sub>rack</sub>, V<sub>rack</sub>). Conversion factors built into equations.
- **Step Dependencies**: Steps 5 and 6 both require s<sub>rack</sub> to be provided. If not provided, these outputs remain empty.
- **General vs Procedure**: General equations provide alternative formulations and additional calculations not in main procedure.
- **Synchronization**: Calculator uses extensive variable instance mapping to sync values across all three tabs.

### Calculation Order / Dependencies

```
User Inputs:
├─ Pd, NP, np (required)
└─ srack (optional for steps 5, 6)

Step 1:
Dp = NP / Pd →

Step 2:
B (manual lookup from Table 8-10 using Pd) →

Step 3:
BC = B + Dp/2 →

Step 4:
Vrack = (π/6)(Dp × np / 2) →

Step 5 (if srack provided):
t = 60 × (srack / Vrack) →

Step 6 (if srack provided):
θP = (6/π) × (2 × srack / Dp)

General Equations (independent):
├─ vt = π × Dp × np / 2
├─ Vrack_gen = π × Dp × np / 12 (alternative formula)
└─ Srack_gen = π × Dp × ρ / 2 (requires ρ input)
```

---

## 6. Implementation Details

### Variable Instance Mapping

The calculator implements bidirectional synchronization across tabs using the following instance maps:

```javascript
varInstanceMap = {
    'PD': ['PD', 'var_PD'],
    'NP': ['NP', 'var_NP'],
    'np': ['np', 'var_np', 'np_gen1', 'np_gen2', 'np_eq4'],
    'Dp': ['Dp', 'var_Dp', 'Dp_gen1', 'Dp_gen2', 'Dp_gen3', 'Dp_eq3', 'Dp_eq4', 'Dp_eq6'],
    'Vrack': ['Vrack', 'var_Vrack', 'Vrack_gen', 'Vrack_eq5'],
    'srack': ['srack', 'var_srack', 'srack_eq5', 'srack_eq6'],
    'vt_gen': ['vt_gen', 'var_vt_gen'],
    'Srack_gen': ['Srack_gen', 'var_Srack_gen'],
    'rho': ['rho', 'var_rho'],
    'B': ['B', 'var_B', 'B_eq3'],
    'BC': ['BC', 'var_BC'],
    't': ['t', 'var_t'],
    'thetaP': ['thetaP', 'var_thetaP']
};
```

### Input Classification System

The calculator uses a three-tier input system:

1. **White background** - User inputs (manual entry)
   - P<sub>d</sub>, N<sub>P</sub>, n<sub>P</sub>, s<sub>rack</sub>, ρ

2. **Yellow highlight** - Manual lookups from tables
   - B (from Table 8-10)

3. **Green background** - Auto-calculated values
   - D<sub>P</sub>, B-C, V<sub>rack</sub>, t, θ<sub>P</sub>, v<sub>t</sub>, S<sub>rack</sub>

### Session Persistence

The calculator implements LocalStorage-based session management:

```javascript
saveSession() {
    state = {
        userValues: Array.from(userValues),
        values: {all input values}
    };
    localStorage.setItem('rack_state', JSON.stringify(state));
}

loadSession() {
    // Restores all values and user/computed state on page load
}
```

### Summary Panel

The calculator displays a summary of key results:
- Pitch Diameter: D<sub>P</sub> [in]
- Back to Centerline: B-C [in]
- Rack Velocity: V<sub>rack</sub> [ft/min]
- Time: t [s]
- Revolutions: θ<sub>P</sub> [rev]

---

## 7. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (N/A - none present)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout
- [x] Implementation details documented

---

## 8. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This specification documents an **existing implementation**. The calculator has low-medium complexity with straightforward equations and minimal table lookups. Table 8-10 is currently displayed in HTML for manual entry, but could be enhanced with automatic lookup functionality.

**Key Features:**
- Three-tab interface (Procedure, General Equations, Variables)
- Bidirectional variable synchronization across all tabs
- LocalStorage session persistence
- Optional inputs (s<sub>rack</sub>, ρ) for extended calculations
- MathJax LaTeX rendering
- Real-time calculation on input

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template structure
