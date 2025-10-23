# Calculator Specification: Helical Gear Stress Analysis

---

## 1. Metadata

- **Calculator Name:** Helical Gear Stress Calculator
- **Source PDF:** helical_stress.pdf
- **PDF Location:** `documents/gears/helical_stress.pdf`
- **Description:** Complete bending and contact stress analysis for helical gears, including helical-specific geometry calculations (helix angle, normal/transverse pressure angles, axial pitch), bending stress analysis, and contact stress (pitting) analysis with safety factor determination.
- **Complexity:** High (helical-specific geometry, dual stress analyses, multiple table lookups)

### PDF Page Number Reference
**Note:** The helical_stress.pdf shares pages 1-18 with spur_stress.pdf (document pages 72-89), containing common tables and graphs. Helical-specific content begins at PDF page 19.

| PDF Page | Document Page | Content |
|----------|---------------|---------|
| 1-18 | 72-89 | Shared spur gear content (Table 9-1, Figure 9-11, K_v graph, Table 9-2, Table 9-5, Appendix 3, Appendix 5, etc.) |
| 19 | 90 | Section 9.3.3.2 Helical Gears starts |
| 20 | 91 | Helical calculations (steps 3-11) |
| 21-22 | 92-93 | Figure 10-5: Geometry factor (J) for 15° normal pressure angle |
| 23 | 94 | Figure 10-6: Geometry factor (J) for 20° normal pressure angle |
| 24 | 95 | Figure 10-7: Geometry factor (J) for 22° normal pressure angle |
| 25 | 96 | Table 10-1: Geometry factors (I) for 20° normal pressure angle |
| 25 | 96 | Table 10-2: Geometry factors (I) for 25° normal pressure angle |
- **Tabs to Include:**
  - [x] Procedure (step-by-step stress analysis)
  - [x] General Equations (8 key equations)
  - [x] Variables (40+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF follows structured procedure from Steps 4-14 for helical stress analysis
- **General Equations Tab**: Contains 8 key equations for reference without step context
- **Variables Tab**: 40+ variables across helical geometry, stress factors, and results require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **Helical-Specific Parameters** |
| P<sub>nd</sub> | Normal Diametral Pitch | Pitch in normal plane | teeth/in | User Input | - | No | Typically 12 |
| ψ | Helix Angle | Angle of helix | ° | User Input | - | No | Typical range 15-30° |
| P<sub>d</sub> | Transverse Diametral Pitch | Pitch in transverse plane | teeth/in | Auto-Calculated | EQ1 | No | P<sub>d</sub> = P<sub>nd</sub> cos(ψ) |
| p<sub>x</sub> | Axial Pitch | Pitch along axis | in | Auto-Calculated | EQ2 | No | Used for face width |
| φ<sub>n</sub> | Normal Pressure Angle | Pressure angle in normal plane | ° | User Input | - | No | Standard: 15°, 20°, 22.5°, 25° |
| φ<sub>t</sub> | Transverse Pressure Angle | Pressure angle in transverse plane | ° | Auto-Calculated | EQ3 | No | Converts from φ<sub>n</sub> |
| **Basic Gear Parameters** |
| P | Power | Transmitted power | HP | User Input | - | No | Design parameter |
| n<sub>P</sub> | Pinion Speed | Rotational speed of pinion | rpm | User Input | - | No | Faster shaft |
| n<sub>G</sub> | Gear Speed | Rotational speed of gear | rpm | User Input | - | No | Slower shaft |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Typically 24 for helical |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | Auto-Calculated | Rounding | No | Round(N<sub>P</sub> × VR) |
| VR | Velocity Ratio | Speed ratio | - | Auto-Calculated | Calculation | No | n<sub>P</sub>/n<sub>G</sub> |
| D<sub>P</sub> | Pinion Pitch Diameter | Pitch diameter of pinion | in | Auto-Calculated | EQ9 | No | N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Gear Pitch Diameter | Pitch diameter of gear | in | Auto-Calculated | EQ9 | No | N<sub>G</sub>/P<sub>d</sub> |
| F<sub>nom</sub> | Nominal Face Width | Recommended face width | in | Auto-Calculated | EQ10 | No | 2p<sub>x</sub> |
| F | Face Width | Selected face width | in | User Input | - | No | User selects based on F<sub>nom</sub> |
| C | Center Distance | Distance between centers | in | Auto-Calculated | EQ11 | No | (N<sub>P</sub>+N<sub>G</sub>)/(2P<sub>d</sub>) |
| v<sub>t</sub> | Pitch Line Velocity | Linear velocity at pitch line | ft/min | Auto-Calculated | EQ4 | No | Used for dynamic factor |
| W<sub>t</sub> | Tangential Force | Tangential load | lbs | Auto-Calculated | EQ5 | No | 33000P/v<sub>t</sub> |
| **Stress Factors (Same as Spur)** |
| K<sub>o</sub> | Overload Factor | Application factor | - | Table | Table 9-1 | Maybe | Load classification |
| P<sub>des</sub> | Design Power | Adjusted power | HP | Auto-Calculated | P × K<sub>o</sub> | No | - |
| A<sub>v</sub> | Quality Number | Gear quality grade | - | Table | Table 9-5 | Maybe | Quality classification |
| K<sub>v</sub> | Dynamic Factor | Velocity factor | - | Graph | From graph | Maybe | Based on v<sub>t</sub> and A<sub>v</sub> |
| K<sub>s</sub> | Size Factor | Size adjustment | - | Manual | Procedure | No | Typically 1.0 |
| K<sub>m</sub> | Load Distribution Factor | Load distribution | - | Graph/Table | Complex | No | Face width dependent |
| K<sub>B</sub> | Rim Thickness Factor | Backup ratio factor | - | Graph | Based on backup ratio | Maybe | Typically 1.0 |
| K<sub>R</sub> | Reliability Factor | Reliability adjustment | - | Table | Standard values | Maybe | 0.85-1.5 |
| **Helical Geometry Factors (DIFFERENT FROM SPUR)** |
| J<sub>P</sub> | Bending Geometry Factor (Pinion) | Pinion bending geometry | - | Graph | Fig 10-5/10-6/10-7 | No | Based on φ<sub>n</sub> |
| J<sub>G</sub> | Bending Geometry Factor (Gear) | Gear bending geometry | - | Graph | Fig 10-5/10-6/10-7 | No | Based on φ<sub>n</sub> |
| I | Pitting Geometry Factor | Contact geometry | - | Table | Table 10-1/10-2 | No | Based on φ<sub>n</sub>, ψ, VR, N<sub>P</sub> |
| **Bending Stress Analysis** |
| s<sub>t,P</sub> | Bending Stress (Pinion) | Calculated bending stress | psi | Auto-Calculated | EQ6 | No | - |
| s<sub>t,G</sub> | Bending Stress (Gear) | Calculated bending stress | psi | Auto-Calculated | Derived | No | s<sub>t,P</sub> × (J<sub>P</sub>/J<sub>G</sub>) |
| Lifetime | Design Lifetime | Expected operating life | hours | User Input | - | No | For cycle calculations |
| SF (assumed) | Assumed Safety Factor | Initial safety factor | - | User Input | - | No | For material selection |
| Y<sub>NP</sub> | Bending Stress Cycle Factor (Pinion) | Pinion life factor | - | Graph | From graph | Maybe | Based on cycles |
| Y<sub>NG</sub> | Bending Stress Cycle Factor (Gear) | Gear life factor | - | Graph | From graph | Maybe | Based on cycles |
| s<sub>at,P</sub> (required) | Required Allowable Bending Stress | Minimum required strength | ksi | Auto-Calculated | Calculation | No | For material selection |
| HB<sub>P</sub> (required) | Required Brinell Hardness | Minimum hardness | - | Manual | - | No | Reference value |
| s<sub>at,P</sub> (actual) | Actual Allowable Bending Stress | Selected material strength | ksi | User Input | Material table | No | From chosen material |
| SF<sub>bending</sub> | Safety Factor (Bending) | Actual bending safety factor | - | Auto-Calculated | Calculation | No | Must be > 1.0 |
| **Contact Stress Analysis** |
| C<sub>p</sub> | Elastic Coefficient | Material property | psi<sup>1/2</sup> | Table | Material table | Maybe | 2300 for steel |
| s<sub>c</sub> | Contact Stress | Calculated contact stress | psi | Auto-Calculated | EQ7 | No | Hertzian contact stress |
| Z<sub>NP</sub> | Contact Stress Cycle Factor (Pinion) | Pinion pitting life factor | - | Graph | From graph | Maybe | Based on cycles |
| Z<sub>NG</sub> | Contact Stress Cycle Factor (Gear) | Gear pitting life factor | - | Graph | From graph | Maybe | Based on cycles |
| s<sub>ac</sub> (actual) | Actual Allowable Contact Stress | Selected material contact strength | ksi | User Input | Material table | No | From chosen material |
| SF<sub>contact</sub> | Safety Factor (Contact) | Actual contact safety factor | - | Auto-Calculated | EQ8 | No | Must be > 1.0 |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_d = P_{nd} \cos(\psi)` | Transverse Diametral Pitch | P<sub>nd</sub>, ψ | P<sub>d</sub> | ψ in degrees, convert to radians |
| EQ2 | `p_x = \frac{\pi}{P_d \tan(\psi)}` | Axial Pitch | P<sub>d</sub>, ψ | p<sub>x</sub> | ψ in degrees, convert to radians |
| EQ3 | `\phi_t = \arctan\left(\frac{\tan(\phi_n)}{\cos(\psi)}\right)` | Transverse Pressure Angle | φ<sub>n</sub>, ψ | φ<sub>t</sub> | Bidirectional: can solve for φ<sub>n</sub> if φ<sub>t</sub> given |
| EQ4 | `v_t = \frac{\pi D_P n_P}{12}` | Pitch Line Velocity | D<sub>P</sub>, n<sub>P</sub> | v<sub>t</sub> | Converts in·rpm to ft/min |
| EQ5 | `W_t = \frac{33000 P}{v_t}` | Tangential Force | P, v<sub>t</sub> | W<sub>t</sub> | Standard power-force conversion |
| EQ6 | `s_{t,P} = \frac{W_t P_d}{F J_P} K_o K_s K_m K_B K_v` | Bending Stress (Pinion) | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>P</sub>, K<sub>o</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub> | s<sub>t,P</sub> | All K factors multiply |
| EQ7 | `s_c = C_p \sqrt{\frac{W_t K_o K_s K_m K_B K_v}{F D_P I}}` | Contact Stress | C<sub>p</sub>, W<sub>t</sub>, K<sub>o</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub>, F, D<sub>P</sub>, I | s<sub>c</sub> | Square root of ratio |
| EQ8 | `SF_{contact} = \frac{s_{ac} Z_{NP}}{s_c K_R}` | Contact Safety Factor | s<sub>ac</sub>, Z<sub>NP</sub>, s<sub>c</sub>, K<sub>R</sub> | SF<sub>contact</sub> | s<sub>ac</sub> in psi (convert from ksi) |
| EQ9 | `D = \frac{N}{P_d}` | Pitch Diameter | N, P<sub>d</sub> | D | Used for both pinion and gear |
| EQ10 | `F_{nom} = 2p_x` | Nominal Face Width | p<sub>x</sub> | F<sub>nom</sub> | Recommendation only |
| EQ11 | `C = \frac{N_P + N_G}{2P_d}` | Center Distance | N<sub>P</sub>, N<sub>G</sub>, P<sub>d</sub> | C | - |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Helical Gear Parameters (Steps 4-11)** |
| 4 | Input Normal Diametral Pitch and Teeth | User Input | Enter P<sub>nd</sub> (typically 12 teeth/in), N<sub>P</sub> (typically 24) | Starting parameters |
| 5 | Calculate Transverse Diametral Pitch | Equation | P<sub>d</sub> = P<sub>nd</sub> cos(ψ) (EQ1) | Requires helix angle ψ |
| 6 | Calculate Axial Pitch | Equation | p<sub>x</sub> = π / (P<sub>d</sub> tan(ψ)) (EQ2) | Auto-calculated |
| 7a | Input Speeds | User Input | Enter n<sub>P</sub> [rpm], n<sub>G</sub> [rpm] | - |
| 7b | Calculate Velocity Ratio | Equation | VR = n<sub>P</sub>/n<sub>G</sub> | Auto-calculated |
| 7c | Calculate Gear Teeth | Equation | N<sub>G</sub> = Round(N<sub>P</sub> × VR) | Round to integer |
| 8 | Pressure Angle Conversion | Equation | φ<sub>t</sub> = arctan(tan(φ<sub>n</sub>)/cos(ψ)) (EQ3) | Bidirectional - provide either φ<sub>n</sub> OR φ<sub>t</sub> |
| 9a | Calculate Pinion Pitch Diameter | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> (EQ9) | Auto-calculated |
| 9b | Calculate Gear Pitch Diameter | Equation | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ9) | Auto-calculated |
| 10a | Calculate Nominal Face Width | Equation | F<sub>nom</sub> = 2p<sub>x</sub> (EQ10) | Recommendation |
| 10b | Select Face Width | User Input | Enter F (based on F<sub>nom</sub>) | User selects actual value |
| 11a | Calculate Center Distance | Equation | C = (N<sub>P</sub>+N<sub>G</sub>)/(2P<sub>d</sub>) (EQ11) | Auto-calculated |
| 11b | Calculate Pitch Line Velocity | Equation | v<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 (EQ4) | Auto-calculated |
| 11c | Input Power and Calculate Force | Equation | W<sub>t</sub> = 33000P/v<sub>t</sub> (EQ5) | Requires power input |
| **Bending Stress Analysis** |
| 1-2 | Look Up Overload Factor | Table | K<sub>o</sub> from Table 9-1 | Based on application |
| 3 | Calculate Design Power | Equation | P<sub>des</sub> = P × K<sub>o</sub> | Auto-calculated |
| - | Input Quality Number and Dynamic Factor | Table/Graph | A<sub>v</sub> from Table 9-5, K<sub>v</sub> from graph | Manual lookups |
| 13 | Look Up Geometry Factor J (HELICAL) | Graph | J<sub>P</sub>, J<sub>G</sub> from Fig 10-5/10-6/10-7 | **DIFFERENT FROM SPUR** - based on φ<sub>n</sub> |
| - | Input Size, Distribution, and Rim Factors | Manual | K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub> | From procedures/graphs |
| - | Calculate Bending Stress | Equation | s<sub>t,P</sub> using EQ6, s<sub>t,G</sub> = s<sub>t,P</sub> × (J<sub>P</sub>/J<sub>G</sub>) | Auto-calculated |
| - | Input Stress Cycle Factors | Graph | Y<sub>NP</sub>, Y<sub>NG</sub> based on lifetime | Manual lookup |
| - | Input Reliability and Assumed SF | User Input | K<sub>R</sub>, SF (assumed), Lifetime [hours] | For material selection |
| - | Calculate Required Allowable Stress | Calculation | s<sub>at,P</sub> (required) = s<sub>t,P</sub> × K<sub>R</sub> × SF / Y<sub>NP</sub> | Auto-calculated (ksi) |
| - | Select Material and Input Actual Stress | Material Table | s<sub>at,P</sub> (actual) [ksi] | User selects material |
| - | Calculate Bending Safety Factor | Equation | SF<sub>bending</sub> = (s<sub>at,P</sub> × Y<sub>NP</sub>) / (s<sub>t,P</sub> × K<sub>R</sub>) | Auto-calculated |
| **Contact Stress Analysis** |
| - | Input Elastic Coefficient | Table | C<sub>p</sub> (2300 for steel) | Material property |
| 14 | Look Up Pitting Geometry Factor I (HELICAL) | Table | I from Table 10-1 or 10-2 | **DIFFERENT FROM SPUR** - based on φ<sub>n</sub>, ψ, VR, N<sub>P</sub> |
| - | Calculate Contact Stress | Equation | s<sub>c</sub> using EQ7 | Auto-calculated |
| - | Input Contact Stress Cycle Factors | Graph | Z<sub>NP</sub>, Z<sub>NG</sub> based on lifetime | Manual lookup |
| - | Input Actual Allowable Contact Stress | Material Table | s<sub>ac</sub> (actual) [ksi] | From chosen material |
| - | Calculate Contact Safety Factor | Equation | SF<sub>contact</sub> using EQ8 | Auto-calculated |

---

## 5. Additional Notes

### Validation Rules
- **Integer Values**: N<sub>P</sub>, N<sub>G</sub> must be integers
- **Positive Values**: All diameters, pitches, powers, speeds must be > 0
- **Helix Angle Range**: 0° < ψ < 90° (typical: 15-30°)
- **Pressure Angle Standards**: φ<sub>n</sub> typically 15°, 20°, 22.5°, or 25°
- **Safety Factors**: SF<sub>bending</sub> > 1.0, SF<sub>contact</sub> > 1.0 (typically aim for > 1.2)
- **Bidirectional Pressure Angle**: Must provide EITHER φ<sub>n</sub> OR φ<sub>t</sub>, not both

### Standard Values / Constants
- **Typical P<sub>nd</sub>**: 12 teeth/in
- **Typical N<sub>P</sub>**: 24 teeth (helical gears)
- **Typical ψ**: 15-30°
- **Standard φ<sub>n</sub>**: 15°, 20°, 22.5°, 25°
- **C<sub>p</sub> for steel**: 2300 psi<sup>1/2</sup>
- **π**: Use Math.PI (3.14159...)
- **Power conversion constant**: 33000 (ft·lbf/min per HP)

### Dropdown/Lookup Options

**Overload Factor K<sub>o</sub> (Table 9-1):**
- Uniform load + uniform source: 1.00
- Moderate shock + uniform source: 1.25
- Heavy shock + uniform source: 1.75
- (Similar patterns for light shock and medium shock sources)

**Quality Number A<sub>v</sub> (Table 9-5):**
- Commercial quality: A<sub>v</sub> = 6-8
- Precision quality: A<sub>v</sub> = 11-12
- High precision: A<sub>v</sub> = 13+

**Reliability Factor K<sub>R</sub>:**
- 50% reliability: K<sub>R</sub> = 0.85
- 90% reliability: K<sub>R</sub> = 1.00
- 99% reliability: K<sub>R</sub> = 1.25
- 99.9% reliability: K<sub>R</sub> = 1.50

### Helical-Specific Differences from Spur Gears

**Key Differences:**
1. **Geometry Factor J**: Use Figures 10-5, 10-6, or 10-7 (based on φ<sub>n</sub>) instead of spur gear figures
   - φ<sub>n</sub> = 15°: Figure 10-5
   - φ<sub>n</sub> = 20°: Figure 10-6
   - φ<sub>n</sub> = 22.5°: Figure 10-7

2. **Pitting Geometry Factor I**: Use Tables 10-1 or 10-2 instead of spur tables
   - φ<sub>n</sub> = 20°: Table 10-1
   - φ<sub>n</sub> = 25°: Table 10-2
   - Requires interpolation based on ψ, VR, and N<sub>P</sub>

3. **Additional Parameters**: Helix angle (ψ), axial pitch (p<sub>x</sub>), normal vs. transverse pressure angles

4. **Typical Teeth Count**: N<sub>P</sub> typically 24 for helical (not 17-20 as in spur)

### Future Enhancement Opportunities
- **Table 9-1**: Implement K<sub>o</sub> dropdown selector
- **Table 9-5**: Implement A<sub>v</sub> dropdown with quality descriptions
- **K<sub>v</sub> Graph**: Could implement calculation formula if available
- **Figures 10-5/10-6/10-7**: Could digitize J factor graphs for automatic lookup
- **Tables 10-1/10-2**: Could implement 3D interpolation for I factor
- **Material Database**: Auto-populate C<sub>p</sub>, s<sub>at</sub>, s<sub>ac</sub> based on material selection
- **Y<sub>N</sub> and Z<sub>N</sub> Graphs**: Could implement cycle factor calculations
- **Validation**: Auto-check that selected material meets required strengths

### Special Considerations
- **Bidirectional Pressure Angle Conversion**: Calculator must handle both φ<sub>n</sub> → φ<sub>t</sub> and φ<sub>t</sub> → φ<sub>n</sub>
- **Angle Units**: All angles in degrees for user input, must convert to radians for calculations
- **Unit Conversions**: Careful with ft/min vs in/rpm conversions, ksi vs psi
- **Rounding**: N<sub>G</sub> must be rounded to nearest integer
- **J Factor Selection**: Must match correct figure to φ<sub>n</sub> value
- **I Factor Interpolation**: Table 10-1/10-2 require 3-variable interpolation (ψ, VR, N<sub>P</sub>)
- **Safety Factor Interpretation**: Both bending and contact must meet design requirements

### Calculation Order / Dependencies

```
User Inputs: P, nP, nG, Pnd, ψ, NP, phi_n (or phi_t), F →

VR = nP / nG →
NG = Round(NP × VR) →

Pd = Pnd × cos(ψ) (EQ1) →
px = π / (Pd × tan(ψ)) (EQ2) →

If phi_n given:
  phi_t = arctan(tan(phi_n) / cos(ψ)) (EQ3)
Else if phi_t given:
  phi_n = arctan(tan(phi_t) × cos(ψ)) (EQ3 inverse)
→

DP = NP / Pd (EQ9) →
DG = NG / Pd (EQ9) →
Fnom = 2 × px (EQ10) →
C = (NP + NG) / (2 × Pd) (EQ11) →

vt = π × DP × nP / 12 (EQ4) →
Wt = 33000 × P / vt (EQ5) →

Ko (table lookup) →
Pdes = P × Ko →

Av (table), Kv (graph), Ks, Km, KB (manual) →
JP, JG (Figs 10-5/10-6/10-7 based on phi_n) →

st_P = (Wt × Pd / (F × JP)) × Ko × Ks × Km × KB × Kv (EQ6) →
st_G = st_P × (JP / JG) →

KR, SF_assumed, Lifetime (user) →
YNP, YNG (graph based on lifetime) →

sat_P_required = st_P × KR × SF_assumed / YNP →
sat_P_actual (user selects material) →
SF_bending = (sat_P_actual × YNP) / (st_P × KR) →

Cp (table) →
I (Table 10-1 or 10-2 based on phi_n, psi, VR, NP) →

sc = Cp × sqrt((Wt × Ko × Ks × Km × KB × Kv) / (F × DP × I)) (EQ7) →

ZNP, ZNG (graph based on lifetime) →
sac_actual (user selects material) →
SF_contact = (sac_actual × ZNP) / (sc × KR) (EQ8) →

Summary: psi, Pnd, Pd, NP, NG, DP, DG, F, st_P, sc, SF_bending, SF_contact
```

---

## 6. Critical Implementation Requirements

### 6.1 Equation Block Pattern - ALL Variables Must Have Input Fields

**CRITICAL REQUIREMENT**: Every equation block must include input fields for ALL variables in the equation, not just the output variable.

**Wrong Implementation (Missing Variables)**:
```html
<div class="equation-block">
    <div class="equation-latex">
        $P_d = P_{nd} \cos(\psi)$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>P<sub>d</sub> =</label>
            <input type="number" id="Pd" step="0.0001" oninput="syncAllInstances('Pd'); markAsUser('Pd'); calculate()">
        </div>
    </div>
</div>
```

**Correct Implementation (All Variables)**:
```html
<div class="equation-block">
    <div class="equation-latex">
        $P_d = P_{nd} \cos(\psi)$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>P<sub>d</sub> =</label>
            <input type="number" id="Pd" step="0.0001" oninput="syncAllInstances('Pd'); markAsUser('Pd'); calculate()">
        </div>
        <span>=</span>
        <div class="var-input">
            <label>P<sub>nd</sub> =</label>
            <input type="number" id="Pnd_eq" step="1" oninput="syncAllInstances('Pnd'); markAsUser('Pnd'); calculate()">
        </div>
        <span>× cos(</span>
        <div class="var-input">
            <label>ψ =</label>
            <input type="number" id="psi_eq" step="0.1" oninput="syncAllInstances('psi'); markAsUser('psi'); calculate()">
        </div>
        <span>)</span>
    </div>
</div>
```

**Key Points**:
- Every variable in the LaTeX equation must have a corresponding input field
- Input fields should be arranged to visually match the equation structure
- Use span elements with operators (`=`, `×`, `+`, `/`, etc.) to separate inputs
- Each input must have: `oninput="syncAllInstances('varName'); markAsUser('varName'); calculate()"`
- Use appropriate `step` values for each variable type (0.0001 for decimals, 1 for integers, 0.1 for angles)

**Example for Complex Equation**:
```html
<div class="equation-block">
    <div class="equation-latex">
        $s_{t,P} = \frac{W_t P_d}{F J_P} K_o K_s K_m K_B K_v$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>s<sub>t,P</sub> =</label>
            <input type="number" id="st_P" step="1" oninput="syncAllInstances('st_P'); markAsUser('st_P'); calculate()">
        </div>
        <span>= (</span>
        <div class="var-input">
            <label>W<sub>t</sub> =</label>
            <input type="number" id="Wt_eq1" step="0.1" oninput="syncAllInstances('Wt'); markAsUser('Wt'); calculate()">
        </div>
        <span>×</span>
        <div class="var-input">
            <label>P<sub>d</sub> =</label>
            <input type="number" id="Pd_eq1" step="0.0001" oninput="syncAllInstances('Pd'); markAsUser('Pd'); calculate()">
        </div>
        <span>) / (</span>
        <div class="var-input">
            <label>F =</label>
            <input type="number" id="F_eq1" step="0.01" oninput="syncAllInstances('F'); markAsUser('F'); calculate()">
        </div>
        <span>×</span>
        <div class="var-input">
            <label>J<sub>P</sub> =</label>
            <input type="number" id="JP_eq1" step="0.001" oninput="syncAllInstances('JP'); markAsUser('JP'); calculate()">
        </div>
        <span>) ×</span>
        <div class="var-input">
            <label>K<sub>o</sub> =</label>
            <input type="number" id="Ko_eq1" step="0.01" oninput="syncAllInstances('Ko'); markAsUser('Ko'); calculate()">
        </div>
        <!-- ... continue for all K factors ... -->
    </div>
</div>
```

### 6.2 Load Distribution Factor Decomposition (C_pf and C_ma)

**CRITICAL REQUIREMENT**: The Load Distribution Factor (K<sub>m</sub>) must be decomposed into its components C<sub>pf</sub> (Pinion Proportion Factor) and C<sub>ma</sub> (Mesh Alignment Factor) with dedicated equation calculators.

**Implementation Requirements**:

1. **Equation Block Showing Decomposition**:
```html
<h3>Load Distribution Factor Components</h3>

<div class="equation-block">
    <div class="equation-latex">
        $K_m = 1 + C_{pf} + C_{ma}$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>K<sub>m</sub> =</label>
            <input type="number" id="Km" step="0.0001" oninput="syncAllInstances('Km'); markAsUser('Km'); calculate()">
        </div>
        <span>= 1 +</span>
        <div class="var-input">
            <label>C<sub>pf</sub> =</label>
            <input type="number" id="Cpf" step="0.0001" oninput="syncAllInstances('Cpf'); markAsUser('Cpf'); calculate()">
        </div>
        <span>+</span>
        <div class="var-input">
            <label>C<sub>ma</sub> =</label>
            <input type="number" id="Cma" step="0.0001" oninput="syncAllInstances('Cma'); markAsUser('Cma'); calculate()">
        </div>
    </div>
</div>
```

2. **C_pf Equation Calculator**:
```html
<div class="info-box" style="background-color: #e8f4f8; border-left: 4px solid #2196F3;">
    🧮 <strong>C<sub>pf</sub> EQUATION CALCULATOR</strong> (Pinion Proportion Factor)<br>
    <div style="margin-top: 10px;">
        <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 10px;">
            <label>F =</label>
            <input type="number" id="cpf_F_input" step="0.001" style="width: 100px;" oninput="calculateCpf()">
            <span>in</span>
            <label style="margin-left: 10px;">D<sub>P</sub> =</label>
            <input type="number" id="cpf_DP_input" step="0.001" style="width: 100px;" oninput="calculateCpf()">
            <span>in</span>
        </div>
        <div style="display: flex; align-items: center; gap: 10px;">
            <label>C<sub>pf</sub> =</label>
            <input type="number" id="cpf_output" readonly style="width: 120px; background-color: #d4edda;">
        </div>
    </div>
</div>
```

**JavaScript Function for C_pf**:
```javascript
// C_pf Calculator (Pinion Proportion Factor)
function calculateCpf() {
    const F = parseFloat(document.getElementById('cpf_F_input').value);
    const DP = parseFloat(document.getElementById('cpf_DP_input').value);

    if (!isNaN(F) && !isNaN(DP) && DP > 0) {
        let Cpf;
        if (F <= 1.0) {
            Cpf = F / (10 * DP) + 0.025;
        } else if (F >= 1.0 && F < 15) {
            Cpf = F / (10 * DP) + 0.0375 + 0.0125 * F;
        } else {
            Cpf = 0;
        }
        document.getElementById('cpf_output').value = Cpf.toFixed(4);
    } else {
        document.getElementById('cpf_output').value = '';
    }
}
```

3. **C_ma Equation Calculator**:
```html
<div class="info-box" style="background-color: #fff3cd; border-left: 4px solid #ffc107;">
    🧮 <strong>C<sub>ma</sub> EQUATION CALCULATOR</strong> (Mesh Alignment Factor)<br>
    <div style="margin-top: 10px;">
        <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 10px;">
            <label>F =</label>
            <input type="number" id="cma_F_input" step="0.001" style="width: 100px;" oninput="calculateCma()">
            <span>in</span>
            <label style="margin-left: 10px;">Gear Type:</label>
            <select id="cma_gear_type" style="width: 200px;" onchange="calculateCma()">
                <option value="">Select...</option>
                <option value="open">Open Gearing</option>
                <option value="commercial">Commercial Enclosed</option>
                <option value="precision">Precision Enclosed</option>
                <option value="extra">Extra Precision</option>
            </select>
        </div>
        <div style="display: flex; align-items: center; gap: 10px;">
            <label>C<sub>ma</sub> =</label>
            <input type="number" id="cma_output" readonly style="width: 120px; background-color: #d4edda;">
        </div>
    </div>
</div>
```

**JavaScript Function for C_ma**:
```javascript
// C_ma Calculator (Mesh Alignment Factor)
function calculateCma() {
    const F = parseFloat(document.getElementById('cma_F_input').value);
    const gearType = document.getElementById('cma_gear_type').value;

    if (!isNaN(F) && gearType) {
        const F2 = F * F;
        let Cma;

        switch (gearType) {
            case 'open':
                Cma = 0.247 + 0.0167 * F - 0.765e-4 * F2;
                break;
            case 'commercial':
                Cma = 0.127 + 0.0158 * F - 1.093e-4 * F2;
                break;
            case 'precision':
                Cma = 0.0675 + 0.0128 * F - 0.926e-4 * F2;
                break;
            case 'extra':
                Cma = 0.0380 + 0.0102 * F - 0.822e-4 * F2;
                break;
            default:
                Cma = 0;
        }
        document.getElementById('cma_output').value = Cma.toFixed(4);
    } else {
        document.getElementById('cma_output').value = '';
    }
}
```

4. **Integration into calculate() Function**:
```javascript
// Inside calculate() function, add:
// Km calculation from components
const Cpf = getValue('Cpf');
const Cma = getValue('Cma');
if (Cpf !== null && Cma !== null && !userValues.has('Km')) {
    setValue('Km', 1 + Cpf + Cma);
}
```

5. **Variable Map Registration**:
```javascript
// Add to varMap object:
'Cpf': { ids: ['Cpf'], description: 'Pinion Proportion Factor' },
'Cma': { ids: ['Cma'], description: 'Mesh Alignment Factor' },
```

**Placement**: Insert this section after the main stress equations and before the material selection section (approximately line 880-970 in the HTML file).

**Design Rationale**:
- C_pf depends on face width (F) and pinion pitch diameter (D<sub>P</sub>)
- C_ma depends on face width (F) and gear housing quality
- Both calculators use piecewise (C_pf) and polynomial (C_ma) formulas
- Calculators use blue/yellow info-box styling to distinguish from main equations
- Output fields are readonly with green background to indicate calculated values

### 6.3 Implementation Checklist

When implementing any stress analysis calculator (spur or helical), verify:

- [ ] Every equation block includes input fields for ALL variables in the equation
- [ ] Equation inputs are arranged to visually match the LaTeX equation structure
- [ ] All inputs have proper `syncAllInstances()`, `markAsUser()`, and `calculate()` handlers
- [ ] C_pf and C_ma decomposition section is included with equation block
- [ ] C_pf calculator with F and D<sub>P</sub> inputs is present
- [ ] C_ma calculator with F and gear type dropdown is present
- [ ] Both calculator functions (`calculateCpf()` and `calculateCma()`) are implemented
- [ ] Cpf and Cma variables are added to varMap
- [ ] Km calculation from Cpf and Cma is added to calculate() function
- [ ] All calculator output fields are readonly with green background
- [ ] Info-box styling distinguishes calculators from main equations

---

## 7. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented
- [x] Piecewise equations explained (N/A)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Helical-specific differences from spur gears documented
- [x] Bidirectional calculations noted (pressure angle conversion)
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has high complexity due to:
1. Helical-specific geometry calculations requiring trigonometric conversions
2. Bidirectional pressure angle conversion (φ<sub>n</sub> ↔ φ<sub>t</sub>)
3. Multiple graph-based lookups (J factors from Figs 10-5/10-6/10-7, I factor from Tables 10-1/10-2)
4. Dual stress analyses (bending and contact) with different geometry factors than spur gears
5. Complex table interpolation for I factor (3 variables: ψ, VR, N<sub>P</sub>)

**Critical Warning for Users:** The PDF instructs users to "Scroll to page 20 of the document, follow the steps starting from step 4, and then scroll to the top and follow as usual. However, skip steps in the spur guide if you already found values in that step (ex. do not set N<sub>P</sub> to something between 17-20, it should be 24)". This indicates the procedure weaves between helical-specific and spur-like steps.

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template
