# Calculator Specification: Bevel Gear Stress Analysis

---

## 1. Metadata

- **Calculator Name:** Bevel Gear Stress Calculator
- **Source PDF:** bevel_stress.pdf
- **PDF Location:** `documents/gears/bevel_stress.pdf`
- **Description:** Complete bending and contact stress analysis for bevel gears, including bevel-specific factors (K<sub>mb</sub>, K<sub>s</sub>, C<sub>s</sub>, C<sub>xc</sub>) and stress cycle factors (K<sub>L</sub>, C<sub>L</sub>).
- **Complexity:** High (bevel-specific equations, dual stress analysis, material selection)

### PDF Page Number Reference

| PDF Page | Document Page | Content |
|----------|---------------|---------|
| 1 | 97 | Section 9.3.3.3 Bevel Gears, Steps 1-6 (K_o, K_s, K_mb, K_m) |
| 2 | 98 | Steps 7-9 (A_v Table 9-5, K_v graph, J graph intro) |
| 3 | 99 | J factor graph (bevel gear geometry factor for bending) |
| 4 | 100 | Steps 10-13 (Table 10-3 reliability, Table 9-12 design life, N_L calculation) |
| 5 | 101 | Step 14: K_L graph (bevel bending stress cycle factor) |
| 6 | 102 | Steps 17-20 (C_p Table 9-7, C_s, C_xc, I graph for bevel) |
| 7 | 103 | Step 22: C_L graph (bevel contact stress cycle factor) |
| 8 | 104 | Material graphs: Grade 1 and Grade 2 contact stress (s_ac vs HB) |
| 9 | 105 | Material graphs: Grade 1 and Grade 2 bending stress (s_at vs HB) |
- **Tabs to Include:**
  - [x] Procedure (25-step comprehensive analysis)
  - [x] General Equations (8 key equations)
  - [x] Variables (40+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear sequential procedure for bending and contact stress analysis
- **General Equations Tab**: Contains 8 key equations that benefit from standalone reference
- **Variables Tab**: 40+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P | Power | Transmitted power | HP | User Input | - | No | Basic input |
| n | Speed | Rotational speed | rpm | User Input | - | No | Basic input |
| D | Pitch Diameter | General pitch diameter | in | User Input | - | No | For velocity calc |
| D<sub>p</sub> | Pinion Pitch Diameter | Pinion pitch diameter | in | User Input | - | No | For contact stress |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Integer value |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | User Input | - | No | Integer value |
| P<sub>d</sub> | Diametral Pitch | Teeth per inch | teeth/in | User Input | - | No | Affects K<sub>s</sub> |
| F | Face Width | Axial width of gear teeth | in | User Input | - | No | Affects K<sub>m</sub>, C<sub>s</sub> |
| lifetime | Design Lifetime | Expected service life | hours | User Input | Table 9-12 | No | For N<sub>L</sub> calc |
| SF (assumed) | Assumed Safety Factor | Design safety factor | - | User Input | - | No | Typically 1-2 |
| Material | Material Name | Selected material | - | User Input | - | No | Text field |
| HB (actual) | Actual Hardness | Brinell hardness of material | - | User Input | - | No | From material |
| **Manual Lookups** |
| K<sub>o</sub> | Overload Factor | Accounts for shock loads | - | Table | Table 9-1 | No | Power source/driven machine |
| K<sub>mb</sub> | Mounting Factor | Bevel mounting configuration | - | Dropdown | - | No | 1.0/1.1/1.25 |
| A<sub>v</sub> | Quality Number | Manufacturing quality | - | Table | Table 9-5 | No | Application-based |
| K<sub>v</sub> | Dynamic Factor | Velocity-dependent factor | - | Graph | Dynamic factor graph | Maybe | From A<sub>v</sub> and v<sub>t</sub> |
| J | Geometry Factor (Bending) | Bending geometry factor | - | Graph | Bevel J graph | Maybe | From number of teeth |
| K<sub>R</sub> | Reliability Factor (Bending) | Bending reliability | - | Constant | - | No | 1.0 for 99% |
| K<sub>L</sub> | Stress Cycle Factor (Bending) | Bevel bending fatigue | - | Graph | Bevel K<sub>L</sub> graph | Maybe | From N<sub>L</sub> |
| HB (required) | Required Hardness (Bending) | Minimum hardness needed | - | Graph | Bevel material graph | Maybe | From s<sub>at</sub> |
| C<sub>p</sub> | Elastic Coefficient | Material elastic property | psi<sup>1/2</sup> | Table | Table 9-7 | No | 2300 for steel |
| C<sub>xc</sub> | Crown Factor | Tooth crowning factor | - | Dropdown | - | No | 1.5 (crowned) or 2.0 |
| I | Pitting Geometry Factor | Contact geometry factor | - | Graph | Bevel I graph | Maybe | From number of teeth |
| C<sub>R</sub> | Reliability Factor (Contact) | Contact reliability | - | Constant | - | No | 1.0 for 99% |
| C<sub>L</sub> | Stress Cycle Factor (Contact) | Bevel contact fatigue | - | Graph | Bevel C<sub>L</sub> graph | Maybe | From N<sub>L</sub> |
| HB (contact, required) | Required Hardness (Contact) | Hardness for pitting | - | Graph | Bevel material graph | Maybe | From s<sub>ac</sub> |
| s<sub>at</sub> (actual) | Actual Allowable Bending | Material bending strength | ksi | Graph | Bevel formula | Maybe | From HB (actual) |
| s<sub>ac</sub> (actual) | Actual Allowable Contact | Material contact strength | ksi | Graph | Bevel formula | Maybe | From HB (actual) |
| **Auto-Calculated** |
| v<sub>t</sub> | Pitch Line Velocity | Tangential velocity | ft/min | Equation | EQ1 | No | πDn/12 |
| W<sub>t</sub> | Transmitted Load | Tangential force | lbs | Equation | EQ2 | No | 33000P/v<sub>t</sub> |
| K<sub>s</sub> | Size Factor | Bevel size factor | - | Equation | EQ3 | No | Piecewise by P<sub>d</sub> |
| K<sub>m</sub> | Load Distribution Factor | Bevel load distribution | - | Equation | EQ4 | No | K<sub>mb</sub> + 0.0036F² |
| s<sub>t</sub> | Bending Stress | Actual bending stress | psi | Equation | EQ5 | No | Main bending formula |
| N<sub>L</sub> | Load Cycles | Total stress cycles | cycles | Equation | EQ6 | No | 60 × lifetime × n |
| s<sub>at</sub> (required) | Required Allowable Bending | Needed bending strength | psi/ksi | Equation | EQ7 | No | (s<sub>t</sub>×SF×K<sub>R</sub>)/K<sub>L</sub> |
| SF<sub>bending</sub> | Bending Safety Factor | Actual bending SF | - | Equation | EQ8 | No | (s<sub>at</sub>×K<sub>L</sub>)/(s<sub>t</sub>×K<sub>R</sub>) |
| C<sub>s</sub> | Pitting Size Factor | Bevel pitting size factor | - | Equation | EQ9 | No | 0.125F + 0.4375 |
| s<sub>c</sub> | Contact Stress | Actual contact stress | psi | Equation | EQ10 | No | Bevel contact formula |
| s<sub>ac</sub> (required) | Required Allowable Contact | Needed contact strength | psi/ksi | Equation | EQ11 | No | (s<sub>c</sub>×SF×C<sub>R</sub>)/C<sub>L</sub> |
| SF<sub>contact</sub> | Contact Safety Factor | Actual contact SF | - | Equation | EQ12 | No | (s<sub>ac</sub>×C<sub>L</sub>)/(s<sub>c</sub>×C<sub>R</sub>) |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `v_t = \frac{\pi D n}{12}` | Pitch Line Velocity | D, n | v<sub>t</sub> | Standard velocity formula |
| EQ2 | `W_t = \frac{33000 P}{v_t}` | Transmitted Load | P, v<sub>t</sub> | W<sub>t</sub> | Power-to-force conversion |
| EQ3 | `K_s = \begin{cases} 0.5 & P_d \geq 16 \\ 0.4867 + \frac{0.2133}{P_d} & P_d < 16 \end{cases}` | Size Factor (Bevel) | P<sub>d</sub> | K<sub>s</sub> | **BEVEL-SPECIFIC** piecewise |
| EQ4 | `K_m = K_{mb} + 0.0036 F^2` | Load Distribution (Bevel) | K<sub>mb</sub>, F | K<sub>m</sub> | **BEVEL-SPECIFIC** formula |
| EQ5 | `s_t = \frac{W_t P_d K_o K_s K_m K_v}{F J}` | Bending Stress | W<sub>t</sub>, P<sub>d</sub>, K<sub>o</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>v</sub>, F, J | s<sub>t</sub> | Main bending formula |
| EQ6 | `N_L = 60 \times \text{lifetime} \times n` | Load Cycles | lifetime, n | N<sub>L</sub> | Total revolutions |
| EQ7 | `s_{at} = \frac{s_t \times SF \times K_R}{K_L}` | Required Bending Strength | s<sub>t</sub>, SF, K<sub>R</sub>, K<sub>L</sub> | s<sub>at</sub> (required) | Material selection |
| EQ8 | `SF_{bending} = \frac{s_{at} K_L}{s_t K_R}` | Bending Safety Factor | s<sub>at</sub> (actual), K<sub>L</sub>, s<sub>t</sub>, K<sub>R</sub> | SF<sub>bending</sub> | Must be ≥ 1.0 |
| EQ9 | `C_s = 0.125 F + 0.4375` | Pitting Size Factor (Bevel) | F | C<sub>s</sub> | **BEVEL-SPECIFIC** |
| EQ10 | `s_c = C_p \sqrt{\frac{W_t K_o K_m K_v C_s C_{xc}}{F D_p I}}` | Contact Stress (Bevel) | C<sub>p</sub>, W<sub>t</sub>, K<sub>o</sub>, K<sub>m</sub>, K<sub>v</sub>, C<sub>s</sub>, C<sub>xc</sub>, F, D<sub>p</sub>, I | s<sub>c</sub> | **BEVEL-SPECIFIC** includes C<sub>s</sub>, C<sub>xc</sub> |
| EQ11 | `s_{ac} = \frac{s_c \times SF \times C_R}{C_L}` | Required Contact Strength | s<sub>c</sub>, SF, C<sub>R</sub>, C<sub>L</sub> | s<sub>ac</sub> (required) | Material selection |
| EQ12 | `SF_{contact} = \frac{s_{ac} C_L}{s_c C_R}` | Contact Safety Factor | s<sub>ac</sub> (actual), C<sub>L</sub>, s<sub>c</sub>, C<sub>R</sub> | SF<sub>contact</sub> | Must be ≥ 1.0 |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **BENDING STRESS ANALYSIS** |
| 1-2 | Overload Factor | Table | Look up K<sub>o</sub> from Table 9-1 | Based on power source and driven machine |
| 2 | Basic Gear Parameters | User Input | Enter N<sub>P</sub>, N<sub>G</sub>, P<sub>d</sub>, F | Fundamental geometry |
| 3a | Pitch Line Velocity | Equation | v<sub>t</sub> = πDn/12 (EQ1) | Auto-calculated |
| 3b | Transmitted Load | Equation | W<sub>t</sub> = 33000P/v<sub>t</sub> (EQ2) | Auto-calculated |
| 4 | Size Factor (BEVEL) | Equation | K<sub>s</sub> = 0.5 if P<sub>d</sub> ≥ 16, else 0.4867 + 0.2133/P<sub>d</sub> (EQ3) | **BEVEL-SPECIFIC** piecewise |
| 5-6 | Mounting Factor | Dropdown | Select K<sub>mb</sub>: 1.0 (both straddle), 1.1 (one straddle), 1.25 (neither) | **BEVEL-SPECIFIC** |
| 6 | Load Distribution Factor | Equation | K<sub>m</sub> = K<sub>mb</sub> + 0.0036F² (EQ4) | **BEVEL-SPECIFIC** formula |
| 7 | Quality Number | Table | Look up A<sub>v</sub> from Table 9-5 | Application or pitch line speed |
| 8 | Dynamic Factor | Graph | Look up K<sub>v</sub> from graph using A<sub>v</sub> and v<sub>t</sub> | Manual lookup |
| 9 | Geometry Factor | Graph | Look up J from bevel gear geometry factor graph | Using number of teeth in mate |
| 10 | Bending Stress | Equation | s<sub>t</sub> = (W<sub>t</sub>P<sub>d</sub>K<sub>o</sub>K<sub>s</sub>K<sub>m</sub>K<sub>v</sub>)/(FJ) (EQ5) | Auto-calculated |
| 11 | Safety Factor Input | User Input | Enter assumed SF and K<sub>R</sub> (typically 1 for 99% reliability) | Design assumptions |
| 12 | (Combined with 11) | - | - | - |
| 13 | Load Cycles | Equation | N<sub>L</sub> = 60 × lifetime × n (EQ6) | lifetime from Table 9-12 |
| 14 | Stress Cycle Factor (BEVEL) | Graph | Look up K<sub>L</sub> from bevel-specific graph using N<sub>L</sub> | **DIFFERENT FROM SPUR Y<sub>N</sub>** |
| 15a | Required Allowable Stress | Equation | s<sub>at</sub> = (s<sub>t</sub>×SF×K<sub>R</sub>)/K<sub>L</sub> (EQ7) | Auto-calculated |
| 15b | Convert to ksi | Conversion | s<sub>at</sub> (ksi) = s<sub>at</sub> (psi) / 1000 | For material selection |
| 16a | Material Selection | Graph/Table | Look up HB (required) from bevel material graph using s<sub>at</sub> | **BEVEL-SPECIFIC** material graph |
| 16b | Select Material | User Input | Enter material name, HB (actual), and s<sub>at</sub> (actual) | From material tables |
| 16c | Bending Safety Factor | Equation | SF<sub>bending</sub> = (s<sub>at</sub>×K<sub>L</sub>)/(s<sub>t</sub>×K<sub>R</sub>) (EQ8) | Must be ≥ 1.0 |
| **CONTACT STRESS ANALYSIS** |
| 17 | Elastic Coefficient | Table/Constant | Look up C<sub>p</sub> from Table 9-7 (2300 for steel) | Material property |
| 18 | Pitting Size Factor (BEVEL) | Equation | C<sub>s</sub> = 0.125F + 0.4375 (EQ9) | **BEVEL-SPECIFIC** |
| 19 | Crown Factor (BEVEL) | Dropdown | Select C<sub>xc</sub>: 1.5 (properly crowned) or 2.0 (non-crowned) | **BEVEL-SPECIFIC** |
| 20 | Pitting Geometry Factor | Graph | Look up I from bevel gear pitting geometry factor graph | From number of teeth |
| 21a | Pinion Diameter | User Input | Enter D<sub>p</sub> (pinion pitch diameter) | For contact stress |
| 21b | Contact Stress (BEVEL) | Equation | s<sub>c</sub> = C<sub>p</sub>√[(W<sub>t</sub>K<sub>o</sub>K<sub>m</sub>K<sub>v</sub>C<sub>s</sub>C<sub>xc</sub>)/(FD<sub>p</sub>I)] (EQ10) | **BEVEL-SPECIFIC** formula |
| 22 | Reliability Factor | User Input | Enter C<sub>R</sub> (typically 1 for 99% reliability) | Same concept as K<sub>R</sub> |
| 23 | Contact Cycle Factor (BEVEL) | Graph | Look up C<sub>L</sub> from bevel-specific graph using N<sub>L</sub> | **DIFFERENT FROM SPUR Z<sub>N</sub>** |
| 24a | Required Contact Stress | Equation | s<sub>ac</sub> = (s<sub>c</sub>×SF×C<sub>R</sub>)/C<sub>L</sub> (EQ11) | Auto-calculated |
| 24b | Convert to ksi | Conversion | s<sub>ac</sub> (ksi) = s<sub>ac</sub> (psi) / 1000 | For material selection |
| 25a | Material Selection (Contact) | Graph/Table | Look up HB (contact, required) from bevel material graph | May be higher than bending |
| 25b | Verify Material | Comparison | Ensure selected material meets both bending and contact requirements | Use higher HB if needed |
| 25c | Contact Safety Factor | Equation | SF<sub>contact</sub> = (s<sub>ac</sub>×C<sub>L</sub>)/(s<sub>c</sub>×C<sub>R</sub>) (EQ12) | Must be ≥ 1.0 |

---

## 5. Additional Notes

### Validation Rules
- **Integer Values**: N<sub>P</sub>, N<sub>G</sub> must be integers
- **Safety Factors**: SF<sub>bending</sub> ≥ 1.0, SF<sub>contact</sub> ≥ 1.0
- **Design Success**: Both bending and contact safety factors must be ≥ 1.0
- **Material Selection**: Final material must satisfy both bending and contact requirements (use higher HB if conflict)
- **Positive Values**: All physical quantities (P, n, D, F, etc.) must be > 0

### Standard Values / Constants
- **C<sub>p</sub> (steel-steel)**: 2300 psi<sup>1/2</sup>
- **K<sub>R</sub> (99% reliability)**: 1.0
- **C<sub>R</sub> (99% reliability)**: 1.0
- **K<sub>xc</sub> (properly crowned)**: 1.5
- **K<sub>xc</sub> (non-crowned)**: 2.0

### Dropdown Options

**K<sub>mb</sub> (Mounting Factor):**
- 1.0 - Both gears straddle mounted
- 1.1 - One gear straddle mounted
- 1.25 - Neither gear straddle mounted

**C<sub>xc</sub> (Crown Factor):**
- 1.5 - Properly crowned teeth (typical)
- 2.0 - Non-crowned teeth

### Bevel-Specific Differences from Spur Gears

**Key Differences:**
1. **Size Factor (K<sub>s</sub>)**: Different formula than spur gears
   - Bevel: K<sub>s</sub> = 0.5 if P<sub>d</sub> ≥ 16, else 0.4867 + 0.2133/P<sub>d</sub>
   - Spur: Based on face width and module

2. **Load Distribution (K<sub>m</sub>)**: Uses mounting factor K<sub>mb</sub>
   - Bevel: K<sub>m</sub> = K<sub>mb</sub> + 0.0036F²
   - Spur: Different formula based on face width and mounting

3. **Pitting Size Factor (C<sub>s</sub>)**: Bevel-specific linear formula
   - Bevel: C<sub>s</sub> = 0.125F + 0.4375
   - Spur: Not used (or C<sub>s</sub> = 1)

4. **Crown Factor (C<sub>xc</sub>)**: Only in bevel gears
   - Bevel: Required (1.5 or 2.0)
   - Spur: Not applicable

5. **Stress Cycle Factors**: Different graphs
   - Bevel: K<sub>L</sub> and C<sub>L</sub> (bevel-specific graphs)
   - Spur: Y<sub>N</sub> and Z<sub>N</sub> (spur-specific graphs)

6. **Contact Stress Formula**: Includes C<sub>s</sub> and C<sub>xc</sub>
   - Bevel: s<sub>c</sub> = C<sub>p</sub>√[(W<sub>t</sub>K<sub>o</sub>K<sub>m</sub>K<sub>v</sub>C<sub>s</sub>C<sub>xc</sub>)/(FD<sub>p</sub>I)]
   - Spur: s<sub>c</sub> = C<sub>p</sub>√[(W<sub>t</sub>K<sub>o</sub>K<sub>m</sub>K<sub>v</sub>)/(FD<sub>p</sub>I)]

### Future Enhancement Opportunities
- **Table 9-1**: Implement dropdown with automatic K<sub>o</sub> lookup
- **Table 9-5**: Implement dropdown with automatic A<sub>v</sub> lookup
- **Dynamic Factor Graph**: Could implement formula approximation for K<sub>v</sub>
- **Geometry Factors (J, I)**: Could implement formula-based calculation from tooth count
- **Stress Cycle Factors (K<sub>L</sub>, C<sub>L</sub>)**: Could implement curve fitting for automatic lookup
- **Material Graphs**: Could implement automatic HB and stress strength lookup by material
- **Design Optimization**: Suggest optimal P<sub>d</sub>, F, material combination
- **Dual Analysis Summary**: Show which requirement (bending or contact) governs design

### Special Considerations
- **Dual Stress Analysis**: Both bending and contact must pass. Design is governed by whichever requires higher material strength.
- **Mounting Configuration**: K<sub>mb</sub> significantly affects load distribution. Straddle mounting (both sides supported) is most rigid.
- **Tooth Crowning**: Properly crowned teeth (C<sub>xc</sub> = 1.5) reduce contact stress by ~15% compared to non-crowned.
- **Material Selection**: Final material must satisfy MAX(HB<sub>bending</sub>, HB<sub>contact</sub>). Contact often governs for hardened gears.
- **Status Indicators**: Calculator shows green ✅ "Design is SAFE" if SF ≥ 1, red ❌ "Design is UNSAFE" if SF < 1 for each analysis.

### Calculation Order / Dependencies

```
Basic Inputs (P, n, D, Dp, NP, NG, Pd, F) →

BENDING ANALYSIS:
Ko (Table 9-1) →
vt = πDn/12 →
Wt = 33000P/vt →
Ks = f(Pd) [piecewise] →
Kmb (dropdown) →
Km = Kmb + 0.0036F² →
Av (Table 9-5) →
Kv (graph from Av, vt) →
J (graph from teeth) →
st = (Wt×Pd×Ko×Ks×Km×Kv)/(F×J) →
SF (assumed), KR (user) →
lifetime (Table 9-12) →
NL = 60 × lifetime × n →
KL (bevel graph from NL) →
sat_required = (st×SF×KR)/KL →
HB_required (bevel material graph) →
Material selection (user) →
HB_actual, sat_actual (user from material) →
SF_bending = (sat_actual×KL)/(st×KR) →
Validate: SF_bending ≥ 1.0

CONTACT ANALYSIS:
Cp (Table 9-7, typically 2300) →
Cs = 0.125F + 0.4375 →
Cxc (dropdown: 1.5 or 2.0) →
I (graph from teeth) →
sc = Cp × √[(Wt×Ko×Km×Kv×Cs×Cxc)/(F×Dp×I)] →
CR (user, typically 1) →
CL (bevel graph from NL) →
sac_required = (sc×SF×CR)/CL →
HB_contact_required (bevel material graph) →
Verify material meets contact requirement →
sac_actual (user from material) →
SF_contact = (sac_actual×CL)/(sc×CR) →
Validate: SF_contact ≥ 1.0

FINAL:
Design OK if both SF_bending ≥ 1.0 AND SF_contact ≥ 1.0
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (25 steps)
- [x] Piecewise equations explained (K<sub>s</sub>)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Bevel-specific differences from spur gears highlighted
- [x] Dropdown options documented
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has high complexity due to dual stress analysis (bending and contact), bevel-specific factors and formulas, and extensive manual lookups from graphs. The calculator requires careful attention to bevel-specific equations that differ from standard spur gear formulas. All bevel-specific items are clearly marked with **⚠️ BEVEL-SPECIFIC** warnings in the UI.

**Key Implementation Points:**
1. K<sub>s</sub> uses piecewise formula (different from spur)
2. K<sub>m</sub> uses K<sub>mb</sub> mounting factor + F² term
3. C<sub>s</sub> and C<sub>xc</sub> only appear in bevel gears
4. K<sub>L</sub> and C<sub>L</sub> use bevel-specific graphs (not Y<sub>N</sub>, Z<sub>N</sub>)
5. Contact stress formula includes C<sub>s</sub> and C<sub>xc</sub> factors
6. Material must satisfy both bending and contact requirements

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
