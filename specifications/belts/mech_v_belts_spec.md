# Calculator Specification: V-Belt Design (Mott Method)

---

## 1. Metadata

- **Calculator Name:** V-Belt Design Calculator (Mott Method)
- **Source PDF:** Mech_325_Bible_Mech-v_belts.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Mech-v_belts.pdf`
- **Description:** Calculates V-belt drive parameters including design power, belt type selection, sheave diameters, belt length, wrap angles, correction factors, and required number of belts following Mott's 7-step methodology.
- **Complexity:** High (iterative procedures, extensive graph lookups, conditional logic)
- **Tabs to Include:**
  - [x] Procedure (7-step process)
  - [x] General Equations (15+ equations)
  - [x] Variables (30+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 7-step sequential procedure with decision points
- **General Equations Tab**: Contains 15+ equations that benefit from reference section
- **Variables Tab**: 30+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| H<sub>nom</sub> | Nominal Power | Input power requirement | hp | User Input | - | No | Starting value |
| n | Input Speed | Driving sheave speed | rpm | User Input | - | No | Speed of faster shaft |
| VR | Velocity Ratio | Speed ratio | - | User Input | - | No | VR = n<sub>driving</sub>/n<sub>driven</sub> |
| n<sub>driven</sub> | Output Speed (optional) | Driven sheave speed | rpm | User Input | - | No | Alternative to VR |
| CD<sub>trial</sub> | Trial Center Distance | Initial guess for center distance | in | User Input | - | No | D < CD < 3(D+d) |
| **Manual Lookups** |
| K<sub>s</sub> | Service Factor | Load type adjustment | - | Table | Table 7-1 | No | Based on machine type and hours |
| belt_type | Belt Type | Belt designation | - | Graph | Belt Type Chart | No | 3V, 5V, or 8V |
| d | Driving Sheave Diameter | Standard sheave diameter | in | Table | Tables 7-15/16/17 | Maybe | Selected from standard sizes |
| D | Driven Sheave Diameter | Standard sheave diameter | in | Table | Tables 7-15/16/17 | Maybe | Selected from standard sizes |
| P<sub>1</sub> | Power Rating at VR=1 | Base rated power (3V/8V) | hp | Graph | Figures 7-14/7-16 | Maybe | From solid curves |
| P<sub>2</sub> | Power Rating at VR=3.38 | Max rated power (3V/8V) | hp | Graph | Figures 7-14/7-16 | Maybe | From dashed curves |
| H<sub>s</sub> | Base Power (5V only) | Base rated power (5V) | hp | Graph | Figure 7-15 | Maybe | From solid curves |
| H<sub>added</sub> | Added Power (5V only) | VR-based addition (5V) | hp | Graph | Figure 7-17 | Maybe | Based on VR |
| L<sub>p</sub> | Standard Belt Length | Selected belt pitch length | in | Table | Table 7-2 | Maybe | Closest to calculated |
| **Auto-Calculated** |
| H<sub>d</sub> | Design Power | Adjusted power for design | hp | Equation | EQ1 | No | H<sub>d</sub> = H<sub>nom</sub> × K<sub>s</sub> |
| V<sub>assumed</sub> | Assumed Belt Speed | Initial belt speed | fpm | Constant | - | No | 4000 fpm (assumed) |
| d<sub>calc</sub> | Calculated Driving Diameter | Initial calculated diameter | in | Equation | EQ2 | No | d = 12V/(πn) |
| D<sub>calc</sub> | Calculated Driven Diameter | Initial calculated diameter | in | Equation | EQ3 | No | D = VR × d |
| VR<sub>final</sub> | Final Velocity Ratio | Recalculated from standard sizes | - | Equation | EQ4 | No | VR = D/d |
| n<sub>out</sub> | Final Output Speed | Actual output speed | rpm | Equation | EQ5 | No | n<sub>out</sub> = n/VR<sub>final</sub> |
| V<sub>final</sub> | Final Belt Speed | Actual belt speed | fpm | Equation | EQ6 | No | V = (πnd)/12 |
| H<sub>r</sub> | Rated Power per Belt | Interpolated/summed power | hp | Equation | EQ7 (3V/8V) or EQ8 (5V) | No | Depends on belt type |
| L<sub>p,calc</sub> | Calculated Belt Length | Initial pitch length | in | Equation | EQ9 | No | Complex formula |
| B | Intermediate Variable | Helper for CD calculation | in | Equation | EQ10 | No | B = 4L<sub>p</sub> - 6.28(D+d) |
| CD | Final Center Distance | Recalculated center distance | in | Equation | EQ11 | No | From standard length |
| θ<sub>d</sub> | Driving Wrap Angle | Contact angle on small sheave | rad/deg | Equation | EQ12 | No | π - 2sin⁻¹((D-d)/(2CD)) |
| θ<sub>D</sub> | Driven Wrap Angle | Contact angle on large sheave | rad/deg | Equation | EQ13 | No | π + 2sin⁻¹((D-d)/(2CD)) |
| C<sub>θ</sub> | Wrap Angle Correction | Correction for wrap angle | - | Equation / Graph | EQ14 | Maybe | Can be interpolated |
| C<sub>L</sub> | Length Correction | Correction for belt length | - | Equation / Graph | EQ15 | Maybe | Can be interpolated |
| H<sub>c</sub> | Corrected Rated Power | Power with corrections applied | hp | Equation | EQ16 | No | H<sub>c</sub> = H<sub>r</sub> × C<sub>θ</sub> × C<sub>L</sub> |
| N<sub>belts,calc</sub> | Calculated Belts | Raw calculation | - | Equation | EQ17 | No | H<sub>d</sub>/H<sub>c</sub> |
| N<sub>belts</sub> | Minimum Belts Required | Final result (rounded up) | - | Equation | EQ18 | No | ⌈H<sub>d</sub>/H<sub>c</sub>⌉ |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `H_d = H_{nom} \times K_s` | Design Power | H<sub>nom</sub>, K<sub>s</sub> | H<sub>d</sub> | - |
| EQ2 | `d = \frac{12V}{\pi n}` | Calculated Driving Diameter | V (4000 fpm), n | d<sub>calc</sub> | Initial calculation |
| EQ3 | `D = VR \times d` | Calculated Driven Diameter | VR, d | D<sub>calc</sub> | Initial calculation |
| EQ4 | `VR_{final} = \frac{D}{d}` | Final Velocity Ratio | D (standard), d (standard) | VR<sub>final</sub> | After standard selection |
| EQ5 | `n_{out} = \frac{n}{VR_{final}}` | Final Output Speed | n, VR<sub>final</sub> | n<sub>out</sub> | Actual speed |
| EQ6 | `V_{final} = \frac{\pi n d}{12}` | Final Belt Speed | n, d (standard) | V<sub>final</sub> | Check |V-4000| ≤ 200 |
| EQ7 | `H_r = P_2 + \frac{VR - 3.38}{3.38 - 1}(P_2 - P_1)` | Rated Power (3V/8V) | P<sub>1</sub>, P<sub>2</sub>, VR | H<sub>r</sub> | If VR > 3.38, use 3.38 |
| EQ8 | `H_r = H_s + H_{added}` | Rated Power (5V) | H<sub>s</sub>, H<sub>added</sub> | H<sub>r</sub> | Additive method |
| EQ9 | `L_p = 2CD + 1.57(D+d) + \frac{(D-d)^2}{4CD}` | Calculated Belt Length | CD<sub>trial</sub>, D, d | L<sub>p,calc</sub> | Pitch length |
| EQ10 | `B = 4L_p - 6.28(D+d)` | Intermediate Variable | L<sub>p</sub>, D, d | B | Helper for CD |
| EQ11 | `CD = \frac{B + \sqrt{B^2 - 32(D-d)^2}}{16}` | Final Center Distance | B, D, d | CD | From standard length |
| EQ12 | `\theta_d = \pi - 2\sin^{-1}\left(\frac{D-d}{2CD}\right)` | Driving Wrap Angle | D, d, CD | θ<sub>d</sub> | Result in radians |
| EQ13 | `\theta_D = \pi + 2\sin^{-1}\left(\frac{D-d}{2CD}\right)` | Driven Wrap Angle | D, d, CD | θ<sub>D</sub> | Result in radians |
| EQ14 | `C_\theta = f(\theta_d)` | Wrap Angle Correction | θ<sub>d</sub> | C<sub>θ</sub> | From graph or interpolation |
| EQ15 | `C_L = f(L_p, \text{belt type})` | Length Correction | L<sub>p</sub>, belt_type | C<sub>L</sub> | From graph or interpolation |
| EQ16 | `H_c = H_r \times C_\theta \times C_L` | Corrected Rated Power | H<sub>r</sub>, C<sub>θ</sub>, C<sub>L</sub> | H<sub>c</sub> | Final power rating |
| EQ17 | `N_{belts,calc} = \frac{H_d}{H_c}` | Calculated Number of Belts | H<sub>d</sub>, H<sub>c</sub> | N<sub>belts,calc</sub> | Raw value |
| EQ18 | `N_{belts} = \lceil N_{belts,calc} \rceil` | Minimum Belts (Round Up) | N<sub>belts,calc</sub> | N<sub>belts</sub> | Always round up |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Initial Setup | User Input | Enter H<sub>nom</sub> [hp], n [rpm], VR or n<sub>driven</sub> [rpm] | Starting values |
| 2a | Service Factor | Table | Look up K<sub>s</sub> from Table 7-1 | Based on machine type, driver, hours |
| 2b | Design Power | Equation | H<sub>d</sub> = H<sub>nom</sub> × K<sub>s</sub> (EQ1) | Auto-calculated |
| 2c | Belt Type Selection | Graph | Use Belt Type Chart with H<sub>d</sub> and n | Yellow input: 3V, 5V, or 8V |
| 3a | Calculate Initial Diameter | Equation | d<sub>calc</sub> = 12V/(πn), V=4000 fpm (EQ2) | Auto-calculated |
| 3b | Select Standard Driving Diameter | Table | Choose d from Tables 7-15/16/17 | Yellow input: standard size |
| 3c | Calculate Driven Diameter | Equation | D<sub>calc</sub> = VR × d (EQ3) | Auto-calculated |
| 3d | Select Standard Driven Diameter | Table | Choose D from Tables 7-15/16/17 | Yellow input: standard size |
| 3e | Recalculate Final Parameters | Equation | VR<sub>final</sub> = D/d (EQ4), n<sub>out</sub> = n/VR (EQ5), V = πnd/12 (EQ6) | Auto-calculated |
| 3f | Validate Belt Speed | Validation | Check |V-4000| ≤ 200 fpm | If fails, retry with different d/D |
| 4a | Read Power Ratings (3V/8V) | Graph | Look up P<sub>1</sub>, P<sub>2</sub> from Figures 7-14/7-16 | Yellow input (conditional) |
| 4b | Calculate Rated Power (3V/8V) | Equation | H<sub>r</sub> via interpolation (EQ7) | Auto-calculated (conditional) |
| 4c | Read Power Ratings (5V) | Graph | Look up H<sub>s</sub> from Figure 7-15, H<sub>added</sub> from Figure 7-17 | Yellow input (conditional) |
| 4d | Calculate Rated Power (5V) | Equation | H<sub>r</sub> = H<sub>s</sub> + H<sub>added</sub> (EQ8) | Auto-calculated (conditional) |
| 5a | Choose Trial Center Distance | User Input | Enter CD<sub>trial</sub>, check D < CD < 3(D+d) | Guidance provided |
| 5b | Calculate Belt Length | Equation | L<sub>p,calc</sub> using EQ9 | Auto-calculated |
| 5c | Select Standard Belt Length | Table | Choose L<sub>p</sub> from Table 7-2 (closest) | Yellow input or dropdown |
| 5d | Recalculate Center Distance | Equation | B = 4L<sub>p</sub> - 6.28(D+d) (EQ10), CD from EQ11 | Auto-calculated |
| 6a | Calculate Wrap Angles | Equation | θ<sub>d</sub> (EQ12), θ<sub>D</sub> (EQ13) | Auto-calculated, show in degrees |
| 6b | Find Wrap Angle Correction | Graph / Equation | C<sub>θ</sub> from graph or interpolation (EQ14) | Yellow or auto |
| 6c | Find Length Correction | Graph / Equation | C<sub>L</sub> from graph or interpolation (EQ15) | Yellow or auto |
| 6d | Calculate Corrected Power | Equation | H<sub>c</sub> = H<sub>r</sub> × C<sub>θ</sub> × C<sub>L</sub> (EQ16) | Auto-calculated |
| 7a | Calculate Number of Belts | Equation | N<sub>belts,calc</sub> = H<sub>d</sub>/H<sub>c</sub> (EQ17) | Auto-calculated |
| 7b | Round Up Minimum Belts | Equation | N<sub>belts</sub> = ⌈N<sub>belts,calc</sub>⌉ (EQ18) | Final result |

---

## 5. Additional Notes

### Validation Rules
- **Positive Values**: H<sub>nom</sub>, n, VR must be > 0
- **Service Factor Range**: 1.0 ≤ K<sub>s</sub> ≤ 2.0
- **Belt Speed Tolerance**: |V<sub>final</sub> - 4000| ≤ 200 fpm (5%)
- **Center Distance Range**: D < CD<sub>trial</sub> < 3(D+d)
- **Minimum Wrap Angle**: θ<sub>d</sub> should typically be > 120° (2.094 rad)
- **VR Capping** (for 3V/8V): If VR > 3.38, use VR = 3.38 for interpolation
- **Number of Belts**: Always round UP to next integer

### Standard Values / Constants
- **Assumed Belt Speed**: V = 4000 fpm (for initial diameter calculation)
- **Belt Speed Tolerance**: ±5% (±200 fpm from 4000)
- **π**: Use 3.14159 or Math.PI
- **Belt Types**: 3V, 5V, 8V (discrete options)

### Dropdown Options

**Belt Type:**
- 3V (or 3VX)
- 5V (or 5VX)
- 8V (or 8VX)

**Service Factor (K<sub>s</sub>) - Table 7-1** (Hierarchical structure):

Example categories:
- **Driven Machine Type**: Smooth loading, Light shock, Moderate shock, Heavy shock, Choke-able
- **Driver Type**: AC motor (normal), AC motor (high torque), DC motor, IC engine
- **Operating Hours**: <6h/day, 6-15h/day, >15h/day
- **K<sub>s</sub> Range**: 1.0 to 2.0

*Note*: Full table has ~50+ combinations. Recommend simplified dropdown or hierarchical selection.

**Standard Belt Lengths - Table 7-2** (by belt type):

- **3V only**: 25, 26.5, 28, 30, 31.5, 33.5, 35.5, 37.5, 40, 42.5, 45, 47.5, 165 in
- **3V and 5V**: 50, 53, 56, 60, 63, 67, 71, 75, 80, 85, 90, 95 in
- **3V, 5V, 8V**: 100, 106, 112, 118, 125, 132, 140 in
- **5V and 8V**: 150, 160, 170, 180, 190, 200, 212, 224, 236, 250, 265, 280, 300, 315, 335, 355 in
- **8V only**: 375, 400, 425, 450, 475, 500 in

### Future Enhancement Opportunities
- **Belt Type Chart**: Could digitize for automatic belt type selection
- **Power Rating Graphs**: Could digitize Figures 7-14, 7-15, 7-16, 7-17 for automatic P<sub>1</sub>, P<sub>2</sub>, H<sub>s</sub>, H<sub>added</sub> lookup
- **Standard Sheave Sizes**: Could implement automatic "closest match" highlighting
- **Correction Factor Interpolation**: Implement linear interpolation for C<sub>θ</sub> and C<sub>L</sub> based on graph data
- **Belt Length Auto-Select**: Automatically select closest standard length from Table 7-2
- **Iterative Refinement Helper**: Suggest adjustments to d/D when belt speed validation fails

### Special Considerations
- **Conditional Logic**: Different rated power calculation for 3V/8V (interpolation) vs 5V (additive)
- **Iterative Step 3**: May require multiple trials to achieve belt speed within tolerance
- **Angle Units**: Calculations use radians; display in degrees for user clarity
- **Standard Size Selection**: Critical that user selects from standard sizes, not arbitrary values
- **Graph Digitization**: Power rating graphs are key challenge for automation
- **Correction Factors**: Can be automated via interpolation if graph data is digitized

### Calculation Order / Dependencies

```
H_nom, n, VR (user) →
K_s (Table 7-1) →
Hd = Hnom × K_s →
belt_type (Belt Type Chart - Graph) →
d_calc = 12×4000/(π×n) →
d (standard - Table 7-15/16/17) →
D_calc = VR × d →
D (standard - Table 7-15/16/17) →
VR_final = D/d →
n_out = n/VR_final →
V_final = π×n×d/12 →
Validate: |V_final - 4000| ≤ 200 →

IF belt_type == 3V or 8V:
    P1, P2 (Figures 7-14/7-16) →
    Hr = P2 + ((VR-3.38)/(3.38-1))×(P2-P1)
ELIF belt_type == 5V:
    Hs (Figure 7-15) →
    H_added (Figure 7-17) →
    Hr = Hs + H_added
END IF →

CD_trial (user, check D < CD < 3(D+d)) →
Lp_calc = 2CD + 1.57(D+d) + (D-d)²/(4CD) →
Lp (standard - Table 7-2, closest) →
B = 4Lp - 6.28(D+d) →
CD = (B + √(B² - 32(D-d)²))/16 →

θd = π - 2sin⁻¹((D-d)/(2CD)) →
θD = π + 2sin⁻¹((D-d)/(2CD)) →
Cθ (graph or interpolation) →
CL (graph or interpolation) →
Hc = Hr × Cθ × CL →

N_belts_calc = Hd/Hc →
N_belts = ⌈N_belts_calc⌉
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (conditional logic for 3V/8V vs 5V)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against PDF
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has high complexity due to extensive graph lookups. Correction factors (C<sub>θ</sub>, C<sub>L</sub>) can be automated via interpolation if graph data is digitized. Power rating graphs (P<sub>1</sub>, P<sub>2</sub>, H<sub>s</sub>, H<sub>added</sub>) remain manual lookups unless extensive digitization is performed.

**Specification Author:** Claude Code
**Date Created:** 2025-10-20
**Last Updated:** 2025-10-20

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
