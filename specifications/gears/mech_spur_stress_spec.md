# Calculator Specification: Spur Gear Stress Analysis (AGMA Standard)

---

## 1. Metadata

- **Calculator Name:** Spur Gear Stress Calculator
- **Source PDF:** spur_stress.pdf
- **PDF Location:** `documents/gears/spur_stress.pdf`
- **Description:** Comprehensive bending and contact stress analysis for spur gears following AGMA standards. Performs complete 30-step design procedure including material selection, stress calculations, and safety factor determination.
- **Complexity:** High (extensive table/graph lookups, iterative material selection, dual stress analysis)

### PDF Page Number Reference

| PDF Page | Document Page | Content |
|----------|---------------|---------|
| 1 | 72 | Section 9.3.3 Stress Calculations title page |
| 2 | 73 | Steps 1-2: Shock classification, Table 9-1 (Overload Factor K_o) |
| 3 | 74 | Steps 3-4: Design power calculation, Figure 9-11 (P_d selection graph) |
| 4 | 75 | Steps 5-12: Teeth selection, velocity ratio, geometry calculations |
| 5 | 76 | Step 13: Face width, Step 14: Table 9-5 (Quality Number A_v) |
| 6 | 77 | Step 15: Dynamic factor K_v graph |
| 7 | 78 | Step 16: J factor graphs (geometry factor for 20° and 25° pressure angles) |
| 8 | 79 | Step 17: Table 9-2 (Size Factor K_s), Step 18: Figures 9-12 and 9-13 (C_pf and C_ma) |
| 9 | 80 | Step 18 cont: Figure 9-13 (C_ma mesh alignment factor), Step 19: Figure 9-14 (K_B rim thickness factor) |
| 10 | 81 | Step 21: Table 9-11 (Reliability Factor K_R), Table 9-12 (Design Life) |
| 11 | 82 | Step 21: Figure 9-21 (Y_N stress cycle factor for bending) |
| 12 | 83 | Step 23: Figure 9-18 (s_at vs HB for bending - Grade 1 and Grade 2 steel) |
| 13-15 | 84-86 | Step 23: Appendix 3 (Material properties - carbon and alloy steels) and Appendix 5 (carburized steels) |
| 16 | 87 | Step 26: Table 9-7 (Elastic Coefficient C_p) |
| 17 | 88 | Step 27: Figure 9-17 (I pitting geometry factor) |
| 18 | 89 | Step 29: Figure 9-22 (Z_N stress cycle factor for contact) |
| 19 | 90 | Step 30: Figure 9-19 (s_ac vs HB for contact - Grade 1 and Grade 2 steel) |

- **Tabs to Include:**
  - [x] Procedure (30-step sequential process)
  - [x] General Equations (8 core equations)
  - [x] Variables (50+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 30-step sequential procedure for complete stress analysis
- **General Equations Tab**: Contains core equations for reference and standalone calculations
- **Variables Tab**: 50+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs - Basic Parameters** |
| P | Power | Transmitted power | HP | User Input | - | No | Basic requirement |
| n<sub>P</sub> | Pinion Speed | Rotational speed of pinion | rpm | User Input | - | No | Faster shaft |
| n<sub>G</sub> (target) | Target Gear Speed | Desired output speed (or range) | rpm | User Input | - | No | Can use midpoint if range |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Typically 17-20 |
| φ | Pressure Angle | Standard pressure angle | degrees | User Input | - | No | Typically 20° |
| **Manual Lookups - Load Factors** |
| K<sub>o</sub> | Overload Factor | Accounts for shock loads | - | Table | Table 9-1 | Maybe | Based on power source and driven machine |
| **Auto-Calculated - Design Power** |
| P<sub>design</sub> | Design Power | Adjusted power for design | HP | Equation | EQ1 | No | P<sub>design</sub> = P × K<sub>o</sub> |
| **Manual Lookups - Diametral Pitch** |
| P<sub>d</sub> | Diametral Pitch | Gear tooth size | teeth/in | Graph | Figure 9-11 | Maybe | From design power vs speed plot |
| **Auto-Calculated - Velocity Ratio** |
| VR (initial) | Initial Velocity Ratio | Speed ratio calculation | - | Equation | EQ2 | No | VR = n<sub>P</sub>/n<sub>G</sub> |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | Equation | EQ3 | No | Round(N<sub>P</sub> × VR) |
| VR (actual) | Actual Velocity Ratio | Recalculated ratio | - | Equation | EQ4 | No | VR = N<sub>G</sub>/N<sub>P</sub> |
| n<sub>G</sub> (actual) | Actual Gear Speed | Recalculated output speed | rpm | Equation | EQ5 | No | n<sub>G</sub> = n<sub>P</sub> × (N<sub>P</sub>/N<sub>G</sub>) |
| **Auto-Calculated - Geometry** |
| D<sub>P</sub> | Pinion Pitch Diameter | Pitch diameter of pinion | in | Equation | EQ6 | No | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Gear Pitch Diameter | Pitch diameter of gear | in | Equation | EQ7 | No | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> |
| C | Center Distance | Distance between shafts | in | Equation | EQ8 | No | C = (N<sub>P</sub>+N<sub>G</sub>)/(2P<sub>d</sub>) |
| v<sub>t</sub> | Pitch Line Velocity | Tangential velocity | ft/min | Equation | EQ9 | No | v<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 |
| W<sub>t</sub> | Tangential Force | Transmitted load | lbs | Equation | EQ10 | No | W<sub>t</sub> = 33000P/v<sub>t</sub> |
| W<sub>r</sub> | Radial Force | Separating force | lbs | Equation | EQ11 | No | W<sub>r</sub> = W<sub>t</sub> tan φ |
| **Auto-Calculated - Face Width** |
| F<sub>lower</sub> | Lower Limit Face Width | Minimum recommended | in | Equation | - | No | F = 8/P<sub>d</sub> |
| F<sub>nominal</sub> | Nominal Face Width | Standard recommendation | in | Equation | - | No | F = 12/P<sub>d</sub> |
| F<sub>upper</sub> | Upper Limit Face Width | Maximum recommended | in | Equation | - | No | F = 16/P<sub>d</sub> |
| F | Face Width (selected) | Chosen face width | in | User Input | - | No | Choose from range |
| **Manual Lookups - Quality & Dynamic** |
| A<sub>v</sub> | Quality Number | AGMA quality level | - | Table | Table 9-5 | Maybe | Based on application/speed |
| K<sub>v</sub> | Dynamic Factor | Velocity/quality adjustment | - | Graph | Figure (from A<sub>v</sub>, v<sub>t</sub>) | Maybe | Speed-dependent |
| **Manual Lookups - Geometry Factors** |
| J<sub>P</sub> | Geometry Factor (Pinion) | Bending geometry factor | - | Graph | Figure (from N<sub>P</sub>) | Maybe | Tooth form factor |
| J<sub>G</sub> | Geometry Factor (Gear) | Bending geometry factor | - | Graph | Figure (from N<sub>G</sub>) | Maybe | Tooth form factor |
| **Manual Lookups - Size Factor** |
| K<sub>s</sub> | Size Factor | Size adjustment | - | Table | Table 9-2 | Maybe | Based on P<sub>d</sub> |
| **Manual Lookups - Load Distribution** |
| C<sub>pf</sub> | Pinion Proportion Factor | Face width effect | - | Graph | Figure 9-12 | Maybe | From F and F/D<sub>P</sub> |
| C<sub>ma</sub> | Mesh Alignment Factor | Alignment correction | - | Graph | Figure 9-13 | Maybe | From F |
| K<sub>m</sub> | Load Distribution Factor | Combined distribution | - | Equation | EQ12 | No | K<sub>m</sub> = 1 + C<sub>pf</sub> + C<sub>ma</sub> |
| F / D<sub>P</sub> | Face Width Ratio | Aspect ratio | - | Equation | - | No | F/D<sub>P</sub> |
| **User Input - Rim Thickness** |
| K<sub>B</sub> | Rim Thickness Factor | Backup ratio effect | - | User Input | - | No | Typically 1.0 for commercial gears |
| **Auto-Calculated - Bending Stress** |
| s<sub>t,P</sub> | Bending Stress (Pinion) | Tooth root stress | psi | Equation | EQ13 | No | AGMA bending stress equation |
| s<sub>t,G</sub> | Bending Stress (Gear) | Tooth root stress | psi | Equation | EQ14 | No | s<sub>t,G</sub> = s<sub>t,P</sub> × J<sub>P</sub>/J<sub>G</sub> |
| **User Input - Life Parameters** |
| K<sub>R</sub> | Reliability Factor | Failure probability | - | User Input | - | No | Typically 1.0 for 99% reliability |
| SF (assumed) | Assumed Safety Factor | Initial design assumption | - | User Input | - | No | Typically 1.0 |
| Lifetime | Design Life | Operating hours | hours | Table | Table 9-12 | Maybe | Application-dependent |
| N<sub>cP</sub> | Load Cycles (Pinion) | Total stress cycles | cycles | Equation | EQ15 | No | N<sub>c</sub> = 60 × lifetime × n<sub>P</sub> |
| N<sub>cG</sub> | Load Cycles (Gear) | Total stress cycles | cycles | Equation | EQ16 | No | N<sub>c</sub> = 60 × lifetime × n<sub>G</sub> |
| **Manual Lookups - Life Factors** |
| Y<sub>NP</sub> | Stress Cycle Factor (Pinion) | Life adjustment for bending | - | Graph | Figure 9-21 | Maybe | From N<sub>cP</sub> |
| Y<sub>NG</sub> | Stress Cycle Factor (Gear) | Life adjustment for bending | - | Graph | Figure 9-21 | Maybe | From N<sub>cG</sub> |
| **Auto-Calculated - Required Bending Strength** |
| s<sub>at,P</sub> (required) | Required Allowable Bending Stress (Pinion) | Material requirement | psi | Equation | EQ17 | No | s<sub>at</sub> = s<sub>t</sub> K<sub>R</sub> SF / Y<sub>N</sub> |
| s<sub>at,G</sub> (required) | Required Allowable Bending Stress (Gear) | Material requirement | psi | Equation | EQ18 | No | Same formula for gear |
| s<sub>at,P</sub> (required) | Required Allowable Bending Stress (Pinion) | Material requirement | ksi | Conversion | - | No | For material selection |
| **Manual Lookups - Material Selection (Bending)** |
| HB<sub>P</sub> (required) | Required Brinell Hardness (Pinion) | Minimum hardness | HB | Graph | Figure 9-18 | Maybe | From s<sub>at</sub> |
| Material (Pinion) | Material Designation | Steel specification | - | Table | Appendix 3 or 5 | No | User selects from tables |
| HB<sub>P</sub> (actual) | Actual Brinell Hardness (Pinion) | Selected material hardness | HB | Table | Appendix 3 or 5 | Maybe | From material tables |
| s<sub>at,P</sub> (actual) | Actual Allowable Bending Stress (Pinion) | Material capability | ksi | Graph | Figure 9-18 | Maybe | From HB<sub>P</sub> actual |
| **Auto-Calculated - Bending Safety Factor** |
| SF<sub>bending</sub> | Safety Factor (Bending) | Final bending safety | - | Equation | EQ19 | No | SF = s<sub>at</sub> Y<sub>N</sub>/(s<sub>t</sub> K<sub>R</sub>) |
| **Manual Lookups - Contact Stress** |
| C<sub>p</sub> | Elastic Coefficient | Material pair property | psi<sup>1/2</sup> | Table | Table 9-7 | Maybe | 2300 for steel-steel |
| I | Pitting Resistance Geometry Factor | Contact geometry | - | Graph | Figure 9-17 | Maybe | From VR and N<sub>P</sub> |
| **Auto-Calculated - Contact Stress** |
| s<sub>c</sub> | Contact Stress | Hertzian contact stress | psi | Equation | EQ20 | No | AGMA contact stress equation |
| **Manual Lookups - Contact Life Factors** |
| Z<sub>NP</sub> | Stress Cycle Factor (Pinion, Contact) | Life adjustment for pitting | - | Graph | Figure 9-22 | Maybe | From N<sub>cP</sub> |
| Z<sub>NG</sub> | Stress Cycle Factor (Gear, Contact) | Life adjustment for pitting | - | Graph | Figure 9-22 | Maybe | From N<sub>cG</sub> |
| **Auto-Calculated - Required Contact Strength** |
| s<sub>ac</sub> (required) | Required Allowable Contact Stress | Material requirement | psi | Equation | EQ21 | No | s<sub>ac</sub> = s<sub>c</sub> K<sub>R</sub> SF / Z<sub>N</sub> |
| s<sub>ac</sub> (required) | Required Allowable Contact Stress | Material requirement | ksi | Conversion | - | No | For material selection |
| **Manual Lookups - Material Selection (Contact)** |
| HB (required, contact) | Required Hardness (Contact) | Minimum hardness | HB | Graph | Figure 9-19 | Maybe | From s<sub>ac</sub> |
| HB (actual, contact) | Actual Hardness (Contact) | Selected material hardness | HB | User Input | - | No | From material choice |
| s<sub>ac</sub> (actual) | Actual Allowable Contact Stress | Material capability | ksi | Graph | Figure 9-19 | Maybe | From HB actual |
| **Auto-Calculated - Contact Safety Factor** |
| SF<sub>contact</sub> | Safety Factor (Contact) | Final contact safety | - | Equation | EQ22 | No | SF = s<sub>ac</sub> Z<sub>N</sub>/(s<sub>c</sub> K<sub>R</sub>) |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_{design} = P \times K_o` | Design Power | P, K<sub>o</sub> | P<sub>design</sub> | Accounts for overloads |
| EQ2 | `VR = \frac{n_P}{n_G}` | Initial Velocity Ratio | n<sub>P</sub>, n<sub>G</sub> (target) | VR (initial) | - |
| EQ3 | `N_G = \text{Round}(N_P \times VR)` | Gear Teeth | N<sub>P</sub>, VR (initial) | N<sub>G</sub> | Round to integer |
| EQ4 | `VR = \frac{N_G}{N_P}` | Actual Velocity Ratio | N<sub>G</sub>, N<sub>P</sub> | VR (actual) | After rounding |
| EQ5 | `n_G = n_P \times \frac{N_P}{N_G}` | Actual Gear Speed | n<sub>P</sub>, N<sub>P</sub>, N<sub>G</sub> | n<sub>G</sub> (actual) | Verify in acceptable range |
| EQ6 | `D_P = \frac{N_P}{P_d}` | Pinion Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | Basic gear geometry |
| EQ7 | `D_G = \frac{N_G}{P_d}` | Gear Pitch Diameter | N<sub>G</sub>, P<sub>d</sub> | D<sub>G</sub> | Basic gear geometry |
| EQ8 | `C = \frac{N_P + N_G}{2 P_d}` | Center Distance | N<sub>P</sub>, N<sub>G</sub>, P<sub>d</sub> | C | Standard mounting |
| EQ9 | `v_t = \frac{\pi D_P n_P}{12}` | Pitch Line Velocity | D<sub>P</sub>, n<sub>P</sub> | v<sub>t</sub> | Converts in⋅rpm to ft/min |
| EQ10 | `W_t = \frac{33000 P}{v_t}` | Tangential Force | P, v<sub>t</sub> | W<sub>t</sub> | Transmitted load |
| EQ11 | `W_r = W_t \tan \phi` | Radial Force | W<sub>t</sub>, φ | W<sub>r</sub> | φ in radians |
| EQ12 | `K_m = 1 + C_{pf} + C_{ma}` | Load Distribution Factor | C<sub>pf</sub>, C<sub>ma</sub> | K<sub>m</sub> | Alignment effects |
| EQ13 | `s_{t,P} = \frac{W_t P_d}{F J_P} K_o K_s K_m K_B K_v` | Bending Stress (Pinion) | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>P</sub>, K<sub>o</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub> | s<sub>t,P</sub> | AGMA bending stress |
| EQ14 | `s_{t,G} = s_{t,P} \times \frac{J_P}{J_G}` | Bending Stress (Gear) | s<sub>t,P</sub>, J<sub>P</sub>, J<sub>G</sub> | s<sub>t,G</sub> | Gear stress from pinion |
| EQ15 | `N_{cP} = 60 \times \text{lifetime} \times n_P` | Load Cycles (Pinion) | lifetime, n<sub>P</sub> | N<sub>cP</sub> | Total stress cycles |
| EQ16 | `N_{cG} = 60 \times \text{lifetime} \times n_G` | Load Cycles (Gear) | lifetime, n<sub>G</sub> | N<sub>cG</sub> | Total stress cycles |
| EQ17 | `s_{at,P} = s_{t,P} \frac{K_R (SF)}{Y_{NP}}` | Required Allowable Bending Stress (Pinion) | s<sub>t,P</sub>, K<sub>R</sub>, SF (assumed), Y<sub>NP</sub> | s<sub>at,P</sub> (required) | Material requirement |
| EQ18 | `s_{at,G} = s_{t,G} \frac{K_R (SF)}{Y_{NG}}` | Required Allowable Bending Stress (Gear) | s<sub>t,G</sub>, K<sub>R</sub>, SF (assumed), Y<sub>NG</sub> | s<sub>at,G</sub> (required) | Material requirement |
| EQ19 | `SF_{bending} = \frac{s_{at} Y_N}{s_t K_R}` | Safety Factor (Bending) | s<sub>at</sub> (actual), Y<sub>N</sub>, s<sub>t</sub>, K<sub>R</sub> | SF<sub>bending</sub> | Final validation |
| EQ20 | `s_c = C_p \sqrt{\frac{W_t K_o K_s K_m K_B K_v}{F D_P I}}` | Contact Stress | C<sub>p</sub>, W<sub>t</sub>, K<sub>o</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub>, F, D<sub>P</sub>, I | s<sub>c</sub> | AGMA contact stress (Hertzian) |
| EQ21 | `s_{ac} = s_c \frac{K_R (SF)}{Z_N}` | Required Allowable Contact Stress | s<sub>c</sub>, K<sub>R</sub>, SF (assumed), Z<sub>N</sub> | s<sub>ac</sub> (required) | Material requirement |
| EQ22 | `SF_{contact} = \frac{s_{ac} Z_N}{s_c K_R}` | Safety Factor (Contact) | s<sub>ac</sub> (actual), Z<sub>N</sub>, s<sub>c</sub>, K<sub>R</sub> | SF<sub>contact</sub> | Final validation |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **BENDING STRESS ANALYSIS** |
| 1-2 | Overload Factor | Table Lookup | Look up K<sub>o</sub> from Table 9-1 | Based on shock from power source and driven machine |
| 3 | Design Power | Equation | P<sub>design</sub> = P × K<sub>o</sub> (EQ1) | Auto-calculated |
| 4 | Diametral Pitch | Graph Lookup | Plot P<sub>design</sub> vs n<sub>P</sub> on Figure 9-11 to find P<sub>d</sub> | ⚠️ Round to smallest standard number |
| 5 | Pinion Teeth Selection | User Input | Choose N<sub>P</sub> in range 17 < N<sub>P</sub> < 20 | Design choice |
| 6 | Target Gear Speed | User Input | Choose n<sub>G</sub> (typically middle of given range) | Or use given value |
| 7 | Initial Velocity Ratio | Equation | VR = n<sub>P</sub>/n<sub>G</sub> (target) (EQ2) | Auto-calculated |
| 8 | Gear Teeth Calculation | Equation | N<sub>G</sub> = Round(N<sub>P</sub> × VR) (EQ3) | Round to integer |
| 9 | Actual Velocity Ratio | Equation | VR (actual) = N<sub>G</sub>/N<sub>P</sub> (EQ4) | After rounding |
| 10 | Actual Gear Speed | Equation | n<sub>G</sub> (actual) = n<sub>P</sub> × (N<sub>P</sub>/N<sub>G</sub>) (EQ5) | Verify in acceptable range |
| 11 | Pitch Diameters | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub>, D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ6, EQ7) | Auto-calculated |
| 12a | Center Distance | Equation | C = (N<sub>P</sub>+N<sub>G</sub>)/(2P<sub>d</sub>) (EQ8) | Auto-calculated |
| 12b | Pitch Line Velocity | Equation | v<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 (EQ9) | Auto-calculated |
| 12c | Tangential Force | Equation | W<sub>t</sub> = 33000P/v<sub>t</sub> (EQ10) | Auto-calculated |
| 12d | Radial Force | Equation | W<sub>r</sub> = W<sub>t</sub> tan φ (EQ11) | Typically φ = 20° |
| 13 | Face Width Range | Equation | F<sub>lower</sub> = 8/P<sub>d</sub>, F<sub>nominal</sub> = 12/P<sub>d</sub>, F<sub>upper</sub> = 16/P<sub>d</sub> | Show range |
| 13 | Face Width Selection | User Input | Select F from range | Design choice |
| 14 | Quality Number | Table Lookup | Look up A<sub>v</sub> from Table 9-5 | Based on application or pitch line speed |
| 15 | Dynamic Factor | Graph Lookup | Look up K<sub>v</sub> from graph using A<sub>v</sub> and v<sub>t</sub> | Speed-quality interaction |
| 16 | Geometry Factors | Graph Lookup | Look up J<sub>P</sub> and J<sub>G</sub> from graphs using N<sub>P</sub> and N<sub>G</sub> | Tooth form factors |
| 17 | Size Factor | Table Lookup | Look up K<sub>s</sub> from Table 9-2 based on P<sub>d</sub> | Size effects |
| 18a | Face Width Ratio | Equation | F/D<sub>P</sub> | For next lookup |
| 18b | Pinion Proportion Factor | Graph Lookup | Look up C<sub>pf</sub> from Figure 9-12 using F and F/D<sub>P</sub> | Face width effects |
| 18c | Mesh Alignment Factor | Graph Lookup | Look up C<sub>ma</sub> from Figure 9-13 using F | Alignment effects |
| 18d | Load Distribution Factor | Equation | K<sub>m</sub> = 1 + C<sub>pf</sub> + C<sub>ma</sub> (EQ12) | Auto-calculated |
| 19 | Rim Thickness Factor | User Input | Enter K<sub>B</sub> (typically 1.0 for commercial gears) | Design parameter |
| 20a | Bending Stress (Pinion) | Equation | s<sub>t,P</sub> = (W<sub>t</sub>P<sub>d</sub>)/(FJ<sub>P</sub>) K<sub>o</sub>K<sub>s</sub>K<sub>m</sub>K<sub>B</sub>K<sub>v</sub> (EQ13) | AGMA bending stress |
| 20b | Bending Stress (Gear) | Equation | s<sub>t,G</sub> = s<sub>t,P</sub> × J<sub>P</sub>/J<sub>G</sub> (EQ14) | Auto-calculated |
| 21a | Reliability Factor | User Input | Enter K<sub>R</sub> (typically 1.0 for 99% reliability) | Design choice |
| 21b | Assumed Safety Factor | User Input | Enter SF (assumed) (typically 1.0) | Design assumption |
| 21c | Design Lifetime | Table Lookup | Choose lifetime from Table 9-12 | Application-dependent |
| 21d | Load Cycles (Pinion) | Equation | N<sub>cP</sub> = 60 × lifetime × n<sub>P</sub> (EQ15) | Auto-calculated |
| 21e | Load Cycles (Gear) | Equation | N<sub>cG</sub> = 60 × lifetime × n<sub>G</sub> (EQ16) | Auto-calculated |
| 21f | Stress Cycle Factors | Graph Lookup | Look up Y<sub>NP</sub> and Y<sub>NG</sub> from Figure 9-21 using N<sub>c</sub> | Life adjustment |
| 22a | Required Allowable Bending Stress (Pinion) | Equation | s<sub>at,P</sub> = s<sub>t,P</sub> K<sub>R</sub> SF/Y<sub>NP</sub> (EQ17) | Material requirement (psi) |
| 22b | Required Allowable Bending Stress (Gear) | Equation | s<sub>at,G</sub> = s<sub>t,G</sub> K<sub>R</sub> SF/Y<sub>NG</sub> (EQ18) | Material requirement (psi) |
| 22c | Convert to ksi | Conversion | Divide by 1000 | For material selection |
| 23a | Required Hardness (Bending) | Graph Lookup | Use Figure 9-18 to find HB from s<sub>at</sub> (ksi) | Material selection |
| 23b | Material Selection (Pinion) | Table Lookup | Choose material from Appendix 3 or 5 with HB ≥ required | User selects |
| 23c | Actual Hardness (Pinion) | Table Lookup | Extract HB<sub>P</sub> (actual) from material tables | From material choice |
| 23d | Actual Allowable Bending Stress | Graph Lookup | Use Figure 9-18 to find s<sub>at,P</sub> (actual) from HB<sub>P</sub> | Material capability |
| 24 | Actual Safety Factor (Bending) | Equation | SF<sub>bending</sub> = s<sub>at</sub> Y<sub>N</sub>/(s<sub>t</sub> K<sub>R</sub>) (EQ19) | Must be ≥ 1.0 |
| **CONTACT STRESS ANALYSIS** |
| 25 | Contact Stress Formula | Reference | Show AGMA contact stress equation | - |
| 26 | Elastic Coefficient | Table Lookup | C<sub>p</sub> = 2300 psi<sup>1/2</sup> for steel (Table 9-7 for other materials) | Material pair property |
| 27 | Pitting Geometry Factor | Graph Lookup | Look up I from Figure 9-17 using VR and N<sub>P</sub> | Contact geometry |
| 28 | Contact Stress | Equation | s<sub>c</sub> = C<sub>p</sub> √[(W<sub>t</sub>K<sub>o</sub>K<sub>s</sub>K<sub>m</sub>K<sub>B</sub>K<sub>v</sub>)/(FD<sub>P</sub>I)] (EQ20) | Hertzian stress |
| 29a | Stress Cycle Factors (Contact) | Graph Lookup | Look up Z<sub>NP</sub> and Z<sub>NG</sub> from Figure 9-22 using N<sub>c</sub> | Life adjustment |
| 29b | Required Allowable Contact Stress | Equation | s<sub>ac</sub> = s<sub>c</sub> K<sub>R</sub> SF/Z<sub>N</sub> (EQ21) | Material requirement (psi) |
| 29c | Convert to ksi | Conversion | Divide by 1000 | For material selection |
| 30a | Required Hardness (Contact) | Graph Lookup | Use Figure 9-19 to find HB from s<sub>ac</sub> (ksi) | Material selection |
| 30b | Actual Hardness (Contact) | User Input | Enter HB (actual) from material choice | From material tables |
| 30c | Actual Allowable Contact Stress | Graph Lookup | Use Figure 9-19 to find s<sub>ac</sub> (actual) from HB | Material capability |
| 30d | Actual Safety Factor (Contact) | Equation | SF<sub>contact</sub> = s<sub>ac</sub> Z<sub>N</sub>/(s<sub>c</sub> K<sub>R</sub>) (EQ22) | Must be ≥ 1.0 |

---

## 5. Additional Notes

### Validation Rules

**Bending Stress Analysis:**
- **Pinion Teeth Range**: 17 < N<sub>P</sub> < 20 (typical design practice)
- **Integer Teeth**: N<sub>G</sub> must be rounded to integer
- **Face Width Range**: 8/P<sub>d</sub> ≤ F ≤ 16/P<sub>d</sub>
- **Safety Factor**: SF<sub>bending</sub> ≥ 1.0 (design is safe)
- **Material Selection**: HB<sub>P</sub> (actual) ≥ HB<sub>P</sub> (required)

**Contact Stress Analysis:**
- **Safety Factor**: SF<sub>contact</sub> ≥ 1.0 (design is safe)
- **Material Consistency**: Use same material for both bending and contact analysis
- **Hardness Compatibility**: Ensure contact hardness requirements are met

**General:**
- **Pressure Angle**: Typically φ = 20° (standard)
- **Reliability**: K<sub>R</sub> = 1.0 for 99% reliability (standard)
- **Rim Thickness**: K<sub>B</sub> = 1.0 for commercial gears (standard)
- **Design Power**: Must use P<sub>d</sub> from Figure 9-11 (smallest standard number)

### Standard Values / Constants

- **Pressure Angle (φ)**: 20° (most common)
- **Reliability Factor (K<sub>R</sub>)**: 1.0 (99% reliability)
- **Assumed Safety Factor**: 1.0 (conservative starting point)
- **Rim Thickness Factor (K<sub>B</sub>)**: 1.0 (commercial gears)
- **Elastic Coefficient (C<sub>p</sub>)**: 2300 psi<sup>1/2</sup> (steel-steel)
- **Conversion Constant**: 33000 (HP to lb⋅ft/min)

### Dropdown Options

**None explicitly implemented in current version**

Future enhancement opportunities for dropdowns:
- Table 9-1: Overload Factor (K<sub>o</sub>) based on power source and driven machine types
- Table 9-5: Quality Number (A<sub>v</sub>) based on application categories
- Table 9-12: Design Life categories
- Appendix 3/5: Material selection from standard steel specifications

### Future Enhancement Opportunities

- **Table 9-1**: Implement dropdown matrix for K<sub>o</sub> (power source × driven machine)
- **Figure 9-11**: Digitize P<sub>d</sub> selection chart for automatic recommendations
- **Table 9-5**: Implement dropdown for A<sub>v</sub> selection
- **Figures for K<sub>v</sub>, J, C<sub>pf</sub>, C<sub>ma</sub>, I**: Digitize graphs for automatic lookup
- **Figure 9-21**: Digitize Y<sub>N</sub> curves for automatic interpolation
- **Figure 9-22**: Digitize Z<sub>N</sub> curves for automatic interpolation
- **Figure 9-18**: Digitize s<sub>at</sub> vs HB relationship
- **Figure 9-19**: Digitize s<sub>ac</sub> vs HB relationship
- **Material Database**: Implement searchable material tables from Appendices 3 and 5
- **Optimization**: Suggest optimal N<sub>P</sub>, P<sub>d</sub>, F combinations for minimum weight/cost
- **Dual Analysis**: Show both pinion and gear requirements side-by-side

### Special Considerations

**Iterative Material Selection:**
- Bending stress analysis may require different material than contact stress analysis
- Designer must select material that satisfies BOTH requirements
- Typically contact stress governs for hardened steels
- Bending stress may govern for lower hardness materials

**Graph Interpolation Requirements:**
- Multiple graphs require manual interpolation (K<sub>v</sub>, J, C<sub>pf</sub>, C<sub>ma</sub>, I, Y<sub>N</sub>, Z<sub>N</sub>)
- Figure 9-11 requires 2D interpolation (power vs speed)
- Hardness-stress correlations (Figures 9-18, 9-19) require curve fitting

**Design Power from Figure 9-11:**
- Plot design power vs pinion speed
- Find intersection with power curves
- Round to SMALLEST standard number (critical for safety)
- Standard P<sub>d</sub> values: 2, 2.5, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 48, 64, etc.

**Load Factors Interaction:**
- All K factors multiply together in stress equations
- Small changes in factors can significantly affect stress
- Dynamic factor (K<sub>v</sub>) typically most significant after K<sub>o</sub>

**Safety Factor Interpretation:**
- SF ≥ 1.0: Design is adequate
- SF < 1.0: Design will fail (choose stronger material or change geometry)
- Typical targets: SF = 1.2-1.5 for normal applications
- Higher SF needed for critical applications or uncertain loads

### Calculation Order / Dependencies

```
INPUTS: P, nP, nG_target, NP, φ

STEP 1-3: DESIGN POWER
Ko (Table 9-1) →
P_design = P × Ko →

STEP 4-6: INITIAL SIZING
Pd (Figure 9-11 from P_design, nP) →
NP (user: 17-20) →
nG_target (user) →

STEP 7-10: GEAR TEETH AND SPEED
VR_initial = nP / nG_target →
NG = Round(NP × VR_initial) →
VR_actual = NG / NP →
nG_actual = nP × (NP / NG) →

STEP 11-12: GEOMETRY AND LOADS
DP = NP / Pd →
DG = NG / Pd →
C = (NP + NG) / (2Pd) →
vt = π DP nP / 12 →
Wt = 33000 P / vt →
Wr = Wt tan φ →

STEP 13: FACE WIDTH
F_range = [8/Pd, 12/Pd, 16/Pd] →
F (user selects from range) →

STEP 14-19: LOAD FACTORS
Av (Table 9-5) →
Kv (graph from Av, vt) →
JP, JG (graphs from NP, NG) →
Ks (Table 9-2 from Pd) →
F/DP = F / DP →
Cpf (Figure 9-12 from F, F/DP) →
Cma (Figure 9-13 from F) →
Km = 1 + Cpf + Cma →
KB (user, typically 1.0) →

STEP 20-24: BENDING STRESS ANALYSIS
st_P = (Wt Pd)/(F JP) Ko Ks Km KB Kv →
st_G = st_P × (JP / JG) →
KR (user, typically 1.0) →
SF_assumed (user, typically 1.0) →
lifetime (Table 9-12) →
NcP = 60 × lifetime × nP →
NcG = 60 × lifetime × nG →
YNP, YNG (Figure 9-21 from Nc) →
sat_P_required = st_P KR SF / YNP →
sat_G_required = st_G KR SF / YNG →
HB_P_required (Figure 9-18 from sat_P) →
Material selection (Appendix 3/5) →
HB_P_actual (from material) →
sat_P_actual (Figure 9-18 from HB_P) →
SF_bending = sat_P_actual YNP / (st_P KR) →
✓ CHECK: SF_bending ≥ 1.0

STEP 25-30: CONTACT STRESS ANALYSIS
Cp (Table 9-7, typically 2300 for steel) →
I (Figure 9-17 from VR, NP) →
sc = Cp √[(Wt Ko Ks Km KB Kv) / (F DP I)] →
ZNP, ZNG (Figure 9-22 from Nc) →
sac_required = sc KR SF / ZN →
HB_contact_required (Figure 9-19 from sac) →
HB_contact_actual (from material choice) →
sac_actual (Figure 9-19 from HB) →
SF_contact = sac_actual ZN / (sc KR) →
✓ CHECK: SF_contact ≥ 1.0

FINAL: Material must satisfy BOTH bending and contact requirements
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML calculator listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (30 steps)
- [x] Piecewise equations explained (N/A)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This specification documents an EXISTING HTML calculator. The calculator has HIGH complexity due to extensive table/graph interpolation requirements. The current implementation uses manual lookup with info boxes for all tables and graphs. Future enhancements could digitize:
- 10+ tables and graphs requiring manual lookup
- Material database integration
- Automatic P<sub>d</sub> selection algorithm
- Bidirectional variable synchronization (already implemented)

**HTML Calculator Features:**
- Three-tab interface (Procedure, General Equations, Variables)
- Automatic calculation as user types
- Bidirectional synchronization across all variable instances
- LocalStorage session persistence (save/load)
- MathJax LaTeX rendering
- Visual status indicators (green for computed values)
- Success/warning messages for safety factors
- Design summary panel

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md template structure
