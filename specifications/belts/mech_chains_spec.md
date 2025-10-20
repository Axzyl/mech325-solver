# Calculator Specification: Chain Drive Design (Mott Method)

---

## 1. Metadata

- **Calculator Name:** Chain Drive Design Calculator (Mott Method)
- **Source PDF:** Mech_325_Bible_Mech-chains.pdf
- **PDF Location:** `documents/belts/Mech_325_Bible_Mech-chains.pdf`
- **Description:** Calculates chain drive parameters including design horsepower, sprocket sizes, center distance, chain length, wrap angles, and required number of strands following the Mott methodology.
- **Complexity:** Medium
- **Tabs to Include:**
  - [x] Procedure (8-step process)
  - [x] General Equations (10+ equations)
  - [x] Variables (20+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 8-step numbered procedure that must be followed sequentially
- **General Equations Tab**: Contains 10+ equations that benefit from having a reference section
- **Variables Tab**: 20+ variables require central management and overview

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| H<sub>nom</sub> | Nominal Horsepower | Input power requirement | hp | User Input | - | No | Starting value |
| n<sub>1</sub> | Input Speed | Driving sprocket speed | rpm | User Input | - | No | Also called n<sub>driving</sub> |
| n<sub>2</sub> | Output Speed | Driven sprocket speed (nominal) | rpm | User Input | - | No | Can be a range, use midpoint |
| VR | Velocity Ratio | Speed ratio | - | User Input | - | No | VR = n<sub>driving</sub>/n<sub>driven</sub> |
| SF | Service Factor | Load type adjustment factor | - | Dropdown | Table 7-17 | No | 9 options based on load type and driver |
| H<sub>d</sub> | Design Horsepower | Adjusted power for design | hp | Equation | EQ1 | No | H<sub>d</sub> = H<sub>nom</sub> × SF |
| Chain Type | Chain Number | Chain size designation | - | Dropdown | - | No | No. 40, No. 60, or No. 80 |
| N<sub>d</sub> | Driving Sprocket Teeth | Number of teeth on input sprocket | teeth | Table | Appendix B | Maybe | From chain selection table, ≥17 recommended |
| p | Pitch | Chain pitch | in | Table | Appendix B | No | 0.5 for No.40, 0.75 for No.60, 1.0 for No.80 |
| H<sub>r</sub> | Rated Power | Power rating from table | hp | Table | Appendix B | Maybe | From chain selection table for chosen chain |
| N<sub>D</sub> | Driven Sprocket Teeth | Number of teeth on output sprocket | teeth | Equation | EQ2 | No | N<sub>D</sub> = N<sub>d</sub> × VR, round to integer |
| VR<sub>actual</sub> | Actual Velocity Ratio | Recalculated speed ratio | - | Equation | EQ3 | No | After rounding N<sub>D</sub> |
| n<sub>2,actual</sub> | Actual Output Speed | Recalculated output speed | rpm | Equation | EQ4 | No | n<sub>2,actual</sub> = n<sub>1</sub>/VR<sub>actual</sub> |
| PD<sub>d</sub> | Driving Pitch Diameter | Pitch circle diameter of driving sprocket | in | Equation | EQ5 | No | PD<sub>d</sub> = p/sin(180°/N<sub>d</sub>) |
| PD<sub>D</sub> | Driven Pitch Diameter | Pitch circle diameter of driven sprocket | in | Equation | EQ6 | No | PD<sub>D</sub> = p/sin(180°/N<sub>D</sub>) |
| CD<sub>trial</sub> | Trial Center Distance | Initial guess for center distance | pitches | User Input | - | No | Typically 30-50 pitches, 40 recommended |
| L<sub>c</sub> | Chain Length | Length of chain | pitches | Equation | EQ7 | No | Iterative calculation, round to even integer |
| L<sub>c,in</sub> | Chain Length (inches) | Length of chain in inches | in | Equation | - | No | L<sub>c</sub> × p |
| CD | Center Distance | Final center distance | pitches | Equation | EQ8 | No | Recalculated from rounded L<sub>c</sub> |
| CD<sub>in</sub> | Center Distance (inches) | Center distance in inches | in | Equation | - | No | CD × p |
| φ<sub>d</sub> | Driving Wrap Angle | Contact angle on driving sprocket | degrees | Equation | EQ9 | No | φ<sub>d</sub> = 180 - 2sin⁻¹((PD<sub>D</sub>-PD<sub>d</sub>)/(2CD<sub>in</sub>)) |
| φ<sub>D</sub> | Driven Wrap Angle | Contact angle on driven sprocket | degrees | Equation | EQ10 | No | φ<sub>D</sub> = 180 + 2sin⁻¹((PD<sub>D</sub>-PD<sub>d</sub>)/(2CD<sub>in</sub>)) |
| Strand Factor | Multiple Strand Factor | Power multiplier for multiple strands | - | Dropdown | - | No | 1.0, 1.7, 2.5, or 3.3 |
| Required Strands | Number of Strands Needed | Minimum strands required | - | Equation | EQ11 | No | H<sub>d</sub>/(H<sub>r</sub> × Strand Factor) < 1 |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `H_d = H_{nom} \times SF` | Design Horsepower | H<sub>nom</sub>, SF | H<sub>d</sub> | - |
| EQ2 | `N_D = N_d \times VR` | Driven Sprocket Teeth (initial) | N<sub>d</sub>, VR | N<sub>D</sub> | Round to nearest integer |
| EQ3 | `VR_{actual} = \frac{N_D}{N_d}` | Actual Velocity Ratio | N<sub>D</sub> (rounded), N<sub>d</sub> | VR<sub>actual</sub> | Recalculated after rounding N<sub>D</sub> |
| EQ4 | `n_{2,actual} = \frac{n_1}{VR_{actual}}` | Actual Output Speed | n<sub>1</sub>, VR<sub>actual</sub> | n<sub>2,actual</sub> | Check if still in acceptable range |
| EQ5 | `PD_d = \frac{p}{\sin(180°/N_d)}` | Driving Pitch Diameter | p, N<sub>d</sub> | PD<sub>d</sub> | Angle in degrees |
| EQ6 | `PD_D = \frac{p}{\sin(180°/N_D)}` | Driven Pitch Diameter | p, N<sub>D</sub> | PD<sub>D</sub> | Angle in degrees |
| EQ7 | `L_c = 2CD + \frac{N_2 + N_1}{2} + \frac{(N_2 - N_1)^2}{4\pi^2 CD}` | Chain Length (pitches) | CD<sub>trial</sub>, N<sub>D</sub>, N<sub>d</sub> | L<sub>c</sub> | Round to nearest even integer |
| EQ8 | `CD = \frac{1}{4}\left[L_c - \frac{N_2+N_1}{2} + \sqrt{\left(L_c - \frac{N_2+N_1}{2}\right)^2 - \frac{8(N_2-N_1)^2}{4\pi^2}}\right]` | Center Distance (recalculated) | L<sub>c</sub> (rounded), N<sub>D</sub>, N<sub>d</sub> | CD | Use rounded L<sub>c</sub> |
| EQ9 | `\phi_d = 180 - 2\sin^{-1}\left(\frac{PD_D - PD_d}{2CD_{in}}\right)` | Driving Wrap Angle | PD<sub>D</sub>, PD<sub>d</sub>, CD<sub>in</sub> | φ<sub>d</sub> | Result in degrees, CD in inches |
| EQ10 | `\phi_D = 180 + 2\sin^{-1}\left(\frac{PD_D - PD_d}{2CD_{in}}\right)` | Driven Wrap Angle | PD<sub>D</sub>, PD<sub>d</sub>, CD<sub>in</sub> | φ<sub>D</sub> | Result in degrees, CD in inches |
| EQ11 | `\frac{H_d}{H_r \times \text{Strand Factor}} < 1` | Strand Check | H<sub>d</sub>, H<sub>r</sub>, Strand Factor | Pass/Fail | Must be < 1 for safe operation |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1 | Input Parameters | User Input | Enter H<sub>nom</sub> [hp], n<sub>1</sub> [rpm], n<sub>2</sub> [rpm], VR | If n<sub>2</sub> is a range, use midpoint for VR |
| 2 | Service Factor | Dropdown | Select SF from Table 7-17 based on load type and driver type | 9 options available |
| 2b | Design Horsepower | Equation | H<sub>d</sub> = H<sub>nom</sub> × SF (EQ1) | Auto-calculated |
| 3 | Chain Selection | Table Lookup | Choose chain type (No. 40, 60, or 80) from Appendix B using H<sub>d</sub> and n<sub>1</sub> | Yellow inputs: N<sub>d</sub>, p, H<sub>r</sub>. Tip: No. 60 is reliable choice |
| 4 | Driven Sprocket Teeth | Equation | N<sub>D</sub> = N<sub>d</sub> × VR (EQ2), round to integer | Must recalculate VR and n<sub>2</sub> after rounding |
| 4b | Recalculate Ratios | Equation | VR<sub>actual</sub> = N<sub>D</sub>/N<sub>d</sub> (EQ3), n<sub>2,actual</sub> = n<sub>1</sub>/VR<sub>actual</sub> (EQ4) | Verify n<sub>2,actual</sub> is in acceptable range |
| 5 | Pitch Diameters | Equation | PD<sub>d</sub> = p/sin(180°/N<sub>d</sub>) (EQ5), PD<sub>D</sub> = p/sin(180°/N<sub>D</sub>) (EQ6) | Auto-calculated |
| 6a | Trial Center Distance | User Input | Enter CD<sub>trial</sub> in pitches | Recommended: 30-50 pitches, typically 40 |
| 6b | Chain Length | Iteration | Calculate L<sub>c</sub> using EQ7, round to nearest even integer | First iteration |
| 6c | Final Center Distance | Iteration | Recalculate CD using EQ8 with rounded L<sub>c</sub> | Second iteration |
| 6d | Convert to Inches | Equation | CD<sub>in</sub> = CD × p, L<sub>c,in</sub> = L<sub>c</sub> × p | Display both units |
| 7 | Wrap Angles | Equation | φ<sub>d</sub> = 180 - 2sin⁻¹((PD<sub>D</sub>-PD<sub>d</sub>)/(2CD<sub>in</sub>)) (EQ9), φ<sub>D</sub> = 180 + 2sin⁻¹((PD<sub>D</sub>-PD<sub>d</sub>)/(2CD<sub>in</sub>)) (EQ10) | Use CD in inches |
| 8a | Strand Factor Selection | Dropdown | Select strand configuration (1, 2, 3, or 4 strands) | Corresponds to factors 1.0, 1.7, 2.5, 3.3 |
| 8b | Strand Check | Equation | H<sub>d</sub>/(H<sub>r</sub> × Strand Factor) < 1 (EQ11) | Show which configuration works |

---

## 5. Additional Notes

### Validation Rules
- **N<sub>D</sub>** must be rounded to nearest integer
- **L<sub>c</sub>** must be rounded to nearest **even** integer (important for chain loop)
- **N<sub>d</sub>** should be ≥17 teeth for best performance
- **n<sub>2,actual</sub>** should be verified to be within acceptable range after recalculation
- **CD<sub>trial</sub>** typically 30-50 pitches (40 is a good default)

### Standard Values / Constants
- **Pitch values**:
  - No. 40 chain: p = 0.5 in
  - No. 60 chain: p = 0.75 in
  - No. 80 chain: p = 1.0 in
- **No. 60 chain** is recommended as most reliable (No. 40 often doesn't work, No. 80 is overkill)
- **Minimum teeth**: N<sub>d</sub> ≥ 17 is best practice

### Dropdown Options

**Service Factor (SF) - Table 7-17:**
- **Smooth Load:**
  - 1.0 - Hydraulic drive
  - 1.0 - Electric motor or turbine
  - 1.2 - Internal combustion engine with mechanical drive
- **Moderate Shock:**
  - 1.2 - Hydraulic drive
  - 1.3 - Electric motor or turbine
  - 1.4 - Internal combustion engine with mechanical drive
- **Heavy Shock:**
  - 1.4 - Hydraulic drive
  - 1.5 - Electric motor or turbine
  - 1.7 - Internal combustion engine with mechanical drive

**Chain Type:**
- No. 40
- No. 60 (recommended)
- No. 80

**Strand Factor:**
- 1.0 (1 strand)
- 1.7 (2 strands)
- 2.5 (3 strands)
- 3.3 (4 strands)

### Future Enhancement Opportunities
- **Appendix B tables** could be made interactive with automatic chain selection based on H<sub>d</sub> and n<sub>1</sub>
- **Service Factor table** is already converted to dropdown (improvement over manual lookup)
- **Automatic strand recommendation** based on H<sub>d</sub>/H<sub>r</sub> ratio

### Special Considerations
- **Iterative calculation** in Step 6: Calculate L<sub>c</sub> from trial CD, round L<sub>c</sub>, then recalculate CD from rounded L<sub>c</sub>
- **Rounding requirements** are critical:
  - N<sub>D</sub> must be integer (affects actual VR and n<sub>2</sub>)
  - L<sub>c</sub> must be even integer (chain loop requirement)
- **Angle calculations** must handle degrees properly (convert for trig functions)
- **Multiple strands** increases capacity by strand factor (not linearly)
- PDF references **Shigley's method** briefly but focuses on **Mott method** (implemented here)

### Calculation Order / Dependencies

```
H_nom, n1, n2, VR (user) →
SF (dropdown) →
Hd = Hnom × SF →
Chain selection from Appendix B (manual: Nd, p, Hr) →
ND = Nd × VR (round to integer) →
VR_actual = ND/Nd →
n2_actual = n1/VR_actual →
PDd = p/sin(180°/Nd) →
PDD = p/sin(180°/ND) →
CD_trial (user) →
Lc = 2CD + (N2+N1)/2 + (N2-N1)²/(4π²CD) (round to even) →
CD = recalculate from Lc →
CD_in = CD × p, Lc_in = Lc × p →
φd = 180 - 2sin⁻¹((PDD-PDd)/(2CD_in)) →
φD = 180 + 2sin⁻¹((PDD-PDd)/(2CD_in)) →
Strand Factor (dropdown) →
Check: Hd/(Hr × Strand Factor) < 1
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
