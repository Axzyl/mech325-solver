# Calculator Specification: Tapered Roller Bearings and Shafts/Keys

---

## 1. Metadata

- **Calculator Name:** Tapered Roller Bearings Selection + Shafts and Keys Design
- **Source PDF:** fizz_tapered_roll.pdf
- **PDF Location:** `documents/bearings/fizz_tapered_roll.pdf`
- **Description:** Two-part calculator: (1) Tapered roller bearing selection with iterative induced load calculations and K-factor convergence, using Weibull reliability analysis (similar to ball/cylindrical bearings but with tapered-specific formulas). (2) Shaft and key design for power transmission with spur/helical gears, including force calculations and torque verification.
- **Complexity:** Complex
- **Tabs to Include:**
  - [x] Procedure (Tapered bearing: 9 iterative steps; Shafts: partial procedure visible)
  - [x] General Equations (15+ equations for bearings + shaft forces)
  - [x] Variables (Essential for 45+ variables)

**Rationale for Tab Structure:**
Contains iterative tapered roller bearing procedure (9 steps with K-factor convergence), plus shaft/gear force calculations. With 45+ variables, multiple table lookups, and complex iteration logic, three tabs are necessary.

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Tapered Roller Bearing - Loading** | | | | | | | |
| R<sub>xA</sub> | Reaction X at Bearing A | X-component at bearing A | lbf or N | User Input | Page 1 | No | From FBD |
| R<sub>yA</sub> | Reaction Y at Bearing A | Y-component at bearing A | lbf or N | User Input | Page 1 | No | From FBD |
| F<sub>rA</sub> | Radial Load at A | Radial load magnitude | lbf or N | Equation | EQ-FRA | No | √(RxA² + RyA²) |
| R<sub>xB</sub> | Reaction X at Bearing B | X-component at bearing B | lbf or N | User Input | Page 1 | No | From FBD |
| R<sub>yB</sub> | Reaction Y at Bearing B | Y-component at bearing B | lbf or N | User Input | Page 1 | No | From FBD |
| F<sub>rB</sub> | Radial Load at B | Radial load magnitude | lbf or N | Equation | EQ-FRB | No | √(RxB² + RyB²) |
| F<sub>a</sub> | Axial Load | External axial force | lbf or N | User Input | Page 2 | No | Applied thrust |
| F<sub>ae</sub> | Applied Axial Load | Axial load on specific bearing | lbf or N | Assignment | Step 1 | No | Usually Fae = Fa on bearing A |
| **Induced Loads and K Factors** | | | | | | | |
| K<sub>A</sub> | K Factor for Bearing A | Tapered bearing constant A | - | Table/User Input | Page 2, Step 2 | Maybe | Initially 1.5, then from table |
| K<sub>B</sub> | K Factor for Bearing B | Tapered bearing constant B | - | Table/User Input | Page 2, Step 2 | Maybe | Initially 1.5, then from table |
| F<sub>iA</sub> | Induced Load at A | Induced thrust at A | lbf or N | Equation | EQ-FIA | No | 0.47 FrA / KA |
| F<sub>iB</sub> | Induced Load at B | Induced thrust at B | lbf or N | Equation | EQ-FIB | No | 0.47 FrB / KB |
| **Equivalent Loads** | | | | | | | |
| F<sub>eA</sub> | Equivalent Load at A | Combined equivalent at A | lbf or N | Equation | EQ-FEA | No | Piecewise based on FiA comparison |
| F<sub>eB</sub> | Equivalent Load at B | Combined equivalent at B | lbf or N | Equation | EQ-FEB | No | Piecewise based on FiA comparison |
| **Life, Reliability, Rating (per bearing)** | | | | | | | |
| 𝓛<sub>D</sub> | Design Life | Expected hours | kh | Table/User Input | Table 11-4 | Maybe | Same as ball/cyl spec |
| n<sub>D</sub> | Angular Speed | Bearing rpm | rpm | User Input | Page 2 | No | Rotational speed |
| L<sub>D</sub> | Design Life (revs) | Life in revolutions | revolutions | Equation | EQ-LD | No | 60 × 𝓛D × nD |
| L<sub>10</sub> | Rating Life | Standard life | 10⁶ rev | Table | Weibull table | No | Mfr dependent |
| x<sub>D</sub> | Life Multiple | LD / L10 | - | Equation | EQ-XD | No | Ratio |
| R<sub>tot</sub> | Total Reliability | Assembly reliability | decimal | User Input | Page 3 | No | e.g., 0.99 |
| n_bearings | Number of Bearings | Count | - | User Input | Page 3 | No | Usually 2 |
| R<sub>i</sub> | Individual Reliability | Per-bearing reliability | decimal | Equation | EQ-RI | No | ⁿ√Rtot |
| a<sub>f</sub> | Application Factor | Load factor | - | Table | Table 11-5 | Maybe | Same as ball/cyl |
| x<sub>0</sub>, θ, b | Weibull Parameters | Reliability params | - | Table | Weibull table | Maybe | Mfr dependent |
| C<sub>10A</sub> | Load Rating at A | Required rating A | kN | Equation | EQ-C10 | No | For bearing A |
| C<sub>10B</sub> | Load Rating at B | Required rating B | kN | Equation | EQ-C10 | No | For bearing B |
| **Bearing Selection** | | | | | | | |
| bore_A | Bore at A | Inner diameter A | mm | Table | Tapered table | Maybe | From selection |
| bore_B | Bore at B | Inner diameter B | mm | Table | Tapered table | Maybe | From selection |
| cone_part_A | Cone Part Number A | Cone ID for A | - | Table | Tapered table | Maybe | From selection |
| cup_part_A | Cup Part Number A | Cup ID for A | - | Table | Tapered table | Maybe | From selection |
| cone_part_B | Cone Part Number B | Cone ID for B | - | Table | Tapered table | Maybe | From selection |
| cup_part_B | Cup Part Number B | Cup ID for B | - | Table | Tapered table | Maybe | From selection |
| K_new_A | Updated K for A | K from selected bearing A | - | Table | Tapered table, K column | Maybe | For next iteration |
| K_new_B | Updated K for B | K from selected bearing B | - | Table | Tapered table, K column | Maybe | For next iteration |
| **Iteration Tracking** | | | | | | | |
| iteration | Iteration Count | Loop counter | - | Counter | Step 9 | No | Until convergence |
| converged | Convergence Flag | Same bearing twice? | boolean | Check | Step 9 | No | Stop condition |
| **Shafts and Keys - Power/Torque** | | | | | | | |
| P | Power | Transmitted power | hp | User Input | Page 6 | No | At each gear |
| n | Angular Speed (shaft) | Shaft rpm | rpm | User Input | Page 6 | No | For torque calc |
| T | Torque | Torque at gear/sheave | lbf·in | Equation | EQ-T-SHAFT | No | 63000P / n |
| **Gear Forces** | | | | | | | |
| D_gear | Pitch Diameter | Gear diameter | in | User Input | Page 6 | No | For force calc |
| φ | Pressure Angle | Spur gear pressure angle | degrees | User Input | Page 6 | No | Typically 20° |
| φ<sub>n</sub> | Normal Pressure Angle | Helical normal angle | degrees | User Input | Page 6 | No | For helical gears |
| ψ | Helix Angle | Helical helix angle | degrees | User Input | Page 6 | No | For helical gears |
| W<sub>t</sub> | Tangential Force | Tangential gear force | lbf | Equation | EQ-WT | No | 2T / D |
| W<sub>r</sub> | Radial Force | Radial gear force | lbf | Equation | EQ-WR-SPUR or EQ-WR-HEL | No | Piecewise: spur vs helical |
| W<sub>x</sub> | Axial Force | Axial helical force | lbf | Equation | EQ-WX | No | Only for helical |

**Notes:**
- Tapered bearing procedure requires iteration on K factors until same bearing selected twice
- Shafts section continues beyond visible pages (specification covers visible portion)
- a = 10/3 for tapered roller bearings (consistent with roller bearing formulas)

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Tapered Roller - Radial Loads** | | | | | |
| EQ-FRA | `F_{rA} = \sqrt{R_{xA}^2 + R_{yA}^2}` | Radial load at bearing A | RxA, RyA | FrA | - |
| EQ-FRB | `F_{rB} = \sqrt{R_{xB}^2 + R_{yB}^2}` | Radial load at bearing B | RxB, RyB | FrB | - |
| **Induced Loads** | | | | | |
| EQ-FIA | `F_{iA} = \frac{0.47 F_{rA}}{K_A}` | Induced thrust at A | FrA, KA | FiA | KA initially 1.5 |
| EQ-FIB | `F_{iB} = \frac{0.47 F_{rB}}{K_B}` | Induced thrust at B | FrB, KB | FiB | KB initially 1.5 |
| **Equivalent Loads (Piecewise)** | | | | | |
| EQ-FEA-CASE1 | `F_{eA} = 0.4F_{rA} + K_A(F_{iB} + F_{ae})` | Equivalent at A (low axial) | FrA, KA, FiB, Fae | FeA | If FiA ≤ (FiB + Fae) |
| EQ-FEB-CASE1 | `F_{eB} = F_{rB}` | Equivalent at B (low axial) | FrB | FeB | If FiA ≤ (FiB + Fae) |
| EQ-FEB-CASE2 | `F_{eB} = 0.4F_{rB} + K_B(F_{iA} - F_{ae})` | Equivalent at B (high axial) | FrB, KB, FiA, Fae | FeB | If FiA > (FiB + Fae) |
| EQ-FEA-CASE2 | `F_{eA} = F_{rA}` | Equivalent at A (high axial) | FrA | FeA | If FiA > (FiB + Fae) |
| **Life and Reliability** | | | | | |
| EQ-LD | `L_D = 60 \mathcal{L}_D n_D` | Design life in revolutions | 𝓛D, nD | LD | - |
| EQ-XD | `x_D = \frac{L_D}{L_{10}}` | Multiple of rating life | LD, L10 | xD | - |
| EQ-RI | `R_i = \sqrt[n]{R_{tot}}` | Individual reliability | Rtot, n | Ri | n-th root |
| **Load Rating (Both Bearings)** | | | | | |
| EQ-C10 | `C_{10} = a_f F_D \left[\frac{x_D}{x_0 + (\theta - x_0)[\ln(1/R_i)]^{1/b}}\right]^{1/a}` | Required load rating | af, FD (FeA or FeB), xD, x0, θ, b, Ri, a | C10 | a = 10/3 for tapered |
| EQ-C10-UPDATE | `\text{new } C_{10} = \frac{\text{new } F_e}{\text{old } F_e}(\text{old } C_{10})` | Iteration shortcut | new Fe, old Fe, old C10 | new C10 | For iteration |
| **Shafts - Torque** | | | | | |
| EQ-T-SHAFT | `T = \frac{63000P}{n}` | Torque from power | P, n | T | P in hp, n in rpm, T in lbf·in |
| **Gear Forces - Spur** | | | | | |
| EQ-WT | `W_t = \frac{2T}{D}` | Tangential force | T, D | Wt | Both spur and helical |
| EQ-WR-SPUR | `W_r = W_t \tan \phi` | Radial force (spur) | Wt, φ | Wr | Spur gears |
| **Gear Forces - Helical** | | | | | |
| EQ-WR-HEL | `W_r = W_t \frac{\tan \phi_n}{\cos \psi}` | Radial force (helical) | Wt, φn, ψ | Wr | Helical gears |
| EQ-WX | `W_x = W_t \tan \psi` | Axial force (helical) | Wt, ψ | Wx | Helical gears only |

**Notes:**
- Equivalent load equations are piecewise based on whether FiA ≤ (FiB + Fae)
- a = 10/3 for all tapered roller bearings
- Iteration updates K factors from bearing selection table

---

## 4. Procedure Steps

### 4.3.2 Tapered Roller Bearing Design Selection

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Step 1** | **Calculate radial and axial loads** | | | |
| 1-1 | Input reaction components | User Input | Enter RxA, RyA, RxB, RyB from FBD | From shaft analysis |
| 1-2 | Calculate radial loads | Equation | FrA = √(RxA² + RyA²), FrB = √(RxB² + RyB²) | Use EQ-FRA, EQ-FRB |
| 1-3 | Input external axial load | User Input | Enter Fa | If thrust load present |
| 1-4 | Assume bearing A carries axial load | Assignment | Fae = Fa | Typical assumption |
| **Step 2** | **Calculate induced loads** | | | |
| 2-1 | Set initial K factors | User Input/Default | KA = 1.5, KB = 1.5 | Initial guess for first iteration |
| 2-2 | Calculate induced loads | Equation | FiA = 0.47FrA / KA, FiB = 0.47FrB / KB | Use EQ-FIA, EQ-FIB |
| **Step 3** | **Calculate equivalent loads** | | | |
| 3-1 | Check condition | Comparison | Is FiA ≤ (FiB + Fae)? | Determines which formulas |
| 3-2a | If FiA ≤ (FiB + Fae) | Equation | FeA = 0.4FrA + KA(FiB + Fae), FeB = FrB | Use EQ-FEA-CASE1, EQ-FEB-CASE1 |
| 3-2b | If FiA > (FiB + Fae) | Equation | FeB = 0.4FrB + KB(FiA - Fae), FeA = FrA | Use EQ-FEB-CASE2, EQ-FEA-CASE2 |
| **Step 4** | **Get design life** | | | |
| 4-1 | Select application type | Dropdown | Choose from Table 11-4 | Same as ball/cyl bearing |
| 4-2 | Get 𝓛D | Table Lookup | Based on application type | Yellow input, in kh |
| **Step 5** | **Calculate life multiple** | | | |
| 5-1 | Input speed nD | User Input | Enter nD in rpm | Bearing speed |
| 5-2 | Select manufacturer | Dropdown | Timken (Mfr 1) or SKF (Mfr 2) | Usually Timken for tapered |
| 5-3 | Get Weibull parameters | Table | Get L10, x0, θ, b | Yellow input from Weibull table |
| 5-4 | Calculate LD | Equation | LD = 60 × 𝓛D × nD | Use EQ-LD |
| 5-5 | Calculate xD | Equation | xD = LD / L10 | Use EQ-XD |
| **Step 6** | **Calculate bearing reliability** | | | |
| 6-1 | Input total reliability | User Input | Enter Rtot (e.g., 0.99) | If given |
| 6-2 | Input number of bearings | User Input | Enter n_bearings (usually 2) | - |
| 6-3 | Calculate individual reliability | Equation | Ri = ⁿ√Rtot | Use EQ-RI |
| **Step 7** | **Calculate load ratings** | | | |
| 7-1 | Get application factor | Table Lookup | Table 11-5 based on application class | Yellow input |
| 7-2 | Set a value | Constant | a = 10/3 | For tapered roller |
| 7-3 | Calculate C10 for bearing A | Equation | Use EQ-C10 with FD = af × FeA | Weibull formula |
| 7-4 | Calculate C10 for bearing B | Equation | Use EQ-C10 with FD = af × FeB | Weibull formula |
| **Step 8** | **Select tentative bearings** | | | |
| 8-1 | Select bearing A from table | Table Lookup | Tapered table (pp 4-5), one-row radial ≥ C10A | Yellow input, record cone/cup parts |
| 8-2 | Select bearing B from table | Table Lookup | Tapered table (pp 4-5), one-row radial ≥ C10B | Yellow input, record cone/cup parts |
| 8-3 | Record new K values | Table | Get K values from selected bearings | From K column in table |
| **Step 9** | **Check convergence and iterate** | | | |
| 9-1 | Compare to previous selection | Comparison | Same bearings as previous iteration? | - |
| 9-2a | If same: Converged! | Success | Done! | Display final bearings |
| 9-2b | If different: Update K factors | Assignment | KA = K_new_A, KB = K_new_B | Use K from Step 8 |
| 9-3 | Recalculate equivalent loads | Shortcut | new C10 = (new Fe / old Fe) × old C10 | Use EQ-C10-UPDATE |
| 9-4 | Repeat Steps 2-8 | Iteration | Loop until convergence | Auto-run or manual step |

### 4.4 Shafts and Keys (Partial - page 6 only)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Step 1** | **Calculate torque at each location** | | | |
| 1-1 | Input power and speed | User Input | Enter P (hp), n (rpm) for each gear/sheave | - |
| 1-2 | Calculate torque | Equation | T = 63000P / n | Use EQ-T-SHAFT |
| 1-3 | Verify torque balance | Check | Σ T = 0 | Sum of torques should be zero |
| **Step 2** | **Calculate forces from gears/sheaves** | | | |
| 2a-1 | For spur gears: Input D, φ | User Input | Pitch diameter, pressure angle | - |
| 2a-2 | Calculate tangential force | Equation | Wt = 2T / D | Use EQ-WT |
| 2a-3 | Calculate radial force | Equation | Wr = Wt tan φ | Use EQ-WR-SPUR |
| 2b-1 | For helical gears: Input D, φn, ψ | User Input | Diameter, normal pressure angle, helix angle | - |
| 2b-2 | Calculate tangential force | Equation | Wt = 2T / D | Use EQ-WT |
| 2b-3 | Calculate radial force | Equation | Wr = Wt tan φn / cos ψ | Use EQ-WR-HEL |
| 2b-4 | Calculate axial force | Equation | Wx = Wt tan ψ | Use EQ-WX |
| **Step 3+** | **(Procedure continues beyond visible pages)** | | | |
| ... | ... | ... | ... | PDF continues |

**Notes:**
- Tapered bearing procedure requires iteration (Steps 2-8 loop) until convergence
- Initial K factors: KA = KB = 1.5
- Convergence: same bearings selected twice in a row
- Shafts section incomplete in visible pages

---

## 5. Additional Notes

### Piecewise Equations

**Equivalent Loads (Step 3):**
Formulas depend on whether FiA ≤ (FiB + Fae):
- **Case 1 (FiA ≤ (FiB + Fae)):** FeA = 0.4FrA + KA(FiB + Fae), FeB = FrB
- **Case 2 (FiA > (FiB + Fae)):** FeB = 0.4FrB + KB(FiA - Fae), FeA = FrA

**Gear Forces (Step 2):**
- **Spur gears:** Wr = Wt tan φ
- **Helical gears:** Wr = Wt tan φn / cos ψ, plus Wx = Wt tan ψ

### Validation Rules

**Convergence:**
- Must iterate Steps 2-8 until same bearings selected twice
- Maximum iterations: suggest 10-20 to prevent infinite loops

**Bearing Selection:**
- C10_table ≥ C10_calculated for both bearings

**Torque Balance (Shafts):**
- Σ T = 0 (sum of all torques must be zero)

**Positive Values:**
- All loads (FrA, FrB, Fa, FeA, FeB) > 0
- All K factors > 0
- Reliability: 0 < Ri, Rtot < 1

### Standard Values / Constants

**Initial K Factors:**
- KA = 1.5 (first iteration)
- KB = 1.5 (first iteration)

**Load-Life Exponent:**
- a = 10/3 for all tapered roller bearings

**Induced Load Coefficient:**
- 0.47 (constant in FiA, FiB formulas)

**Equivalent Load Coefficients:**
- 0.4 (in FeA, FeB formulas)

**Typical Gear Angles:**
- φ = 20° (spur gear pressure angle)
- φn = 20° (helical normal pressure angle)
- ψ = 15° to 30° (typical helix angle)

### Dropdown Options

**bearing_assumed_axial:**
- Bearing A carries axial load (Fae = Fa)
- Bearing B carries axial load (Fae = Fa, swap formulas)

**gear_type (for force calculations):**
- Spur gear (Wr = Wt tan φ, no Wx)
- Helical gear (Wr = Wt tan φn / cos ψ, Wx = Wt tan ψ)

**manufacturer:**
- Timken (Manufacturer 1) - Common for tapered roller bearings
- SKF (Manufacturer 2) - Less common for tapered

### Future Enhancement Opportunities

**Interactive Table Lookups:**
- K factors from tapered bearing table (auto-fill from selection)
- Bearing dimensions from selection (cone/cup part numbers, bore, OD, etc.)
- 𝓛D from Table 11-4
- Weibull parameters (x0, θ, b, L10) from manufacturer
- af from Table 11-5

**Iteration Automation:**
- Auto-run iteration loop for Steps 2-8
- Display iteration progress (K factors, Fe values, bearing selections)
- Convergence indicator

**Bearing Comparison:**
- Show multiple bearing options meeting C10 requirements
- Compare dimensions and K factors

**Shaft FBD Helper:**
- Interactive shaft diagram to calculate reaction forces
- Auto-calculate bearing locations and forces from gear positions

### Special Considerations

**Large Tapered Bearing Table:**
- Pages 4-5 show extensive table with many columns
- Columns: bore (d), outside diameter (D), width (T), rating @ 500 rpm, thrust, K, a (effective center), part numbers (cone, cup), shoulder diameters, etc.
- May need searchable/filterable implementation
- Both metric and imperial units in table

**Iteration Differences vs. Ball/Cylindrical:**
- Ball/cyl iterates on Y2 factor until bearing converges
- Tapered iterates on K factors (KA, KB) until bearings converge
- Both use same Weibull C10 formula but different load formulas

**K Factor Importance:**
- K factor significantly affects induced loads
- Updated from bearing table after each selection
- Critical for convergence

**Shafts Section:**
- Only first 2 steps visible in PDF
- Likely continues with:
  - Free body diagrams
  - Bearing reaction calculations
  - Bending moment and shear diagrams
  - Stress analysis
  - Deflection checks
  - Critical speed
- Specification covers visible portion only

### Calculation Order / Dependencies

**Tapered Roller Bearing:**
```
Inputs: RxA, RyA, RxB, RyB, Fa, nD, Rtot, ... →

FrA = √(RxA² + RyA²), FrB = √(RxB² + RyB²) →
Fae = Fa (assume bearing A carries axial) →

KA = 1.5, KB = 1.5 (initial) →

LOOP:
  FiA = 0.47FrA / KA
  FiB = 0.47FrB / KB →

  If FiA ≤ (FiB + Fae):
    FeA = 0.4FrA + KA(FiB + Fae)
    FeB = FrB
  Else:
    FeB = 0.4FrB + KB(FiA - Fae)
    FeA = FrA →

  Calculate C10A, C10B (using Weibull) →

  Select bearings from table (C10_table ≥ C10_calc) →
  Get new KA, KB from table →

  Same bearings as previous iteration?
    If yes: CONVERGED!
    If no: Update KA, KB, recalculate Fe, repeat
```

**Shafts and Gears:**
```
For each gear/sheave:
  P, n (input) →
  T = 63000P / n →

Verify Σ T = 0 →

For each gear:
  If spur:
    D, φ (input) →
    Wt = 2T / D
    Wr = Wt tan φ
  If helical:
    D, φn, ψ (input) →
    Wt = 2T / D
    Wr = Wt tan φn / cos ψ
    Wx = Wt tan ψ

(Continue with FBD, reactions, etc. - not visible in PDF)
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from visible pages listed (~45 variables)
- [x] Source references noted for table variables
- [x] All equations extracted and converted to LaTeX (15+ equations)
- [x] Procedure steps documented (Tapered: 9 iterative steps; Shafts: partial)
- [x] Piecewise equations explained (equivalent loads, gear forces)
- [x] Validation rules documented (convergence, torque balance)
- [x] Future enhancements noted
- [x] Tab structure justified
- [x] Specification reviewed against visible pages (1-6)
- [x] Consistent formatting

**Note:** Shafts and Keys section (4.4) continues beyond page 6. This specification covers the visible portion.

---

## 7. Implementation Ready

**Specification Status:** [x] Complete (for visible pages)  [ ] Ready for Implementation

**Specification Author:** Claude Code
**Date Created:** 2025-01-09
**Last Updated:** 2025-01-09

**Implementation Notes:**

**Complexity:** Complex calculator with iteration

**Key Features:**
1. **Iteration Engine:** Automatic loop for Steps 2-8 until K-factor convergence
2. **Dual Bearing Selection:** Select both bearing A and B simultaneously
3. **Piecewise Logic:** Equivalent load formulas switch based on FiA comparison
4. **Large Table:** Tapered bearing table spans 2 pages with many columns

**Recommended Structure:**
- **Procedure Tab:** Show iteration progress table with K factors, Fe values, bearing selections, convergence status
- **General Equations Tab:** Group by Loads, Induced, Equivalent, Life, Rating, Shafts
- **Variables Tab:** Organize by Bearing A / Bearing B / Both / Shafts

**Testing Priorities:**
- Verify piecewise equivalent load selection
- Test K-factor iteration convergence
- Validate bearing table lookup
- Check torque balance for shafts

---

## Template Version

Template Version: 1.0
Specification Version: 1.0 (Partial - covers visible pages 1-6)
