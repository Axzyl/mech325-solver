# Calculator Specification: Bevel Gear Design (Fizz Guide)

---

## 1. Metadata

- **Calculator Name:** Bevel Gear Design Calculator (Fizz Guide)
- **Source PDF:** fizz_bevel.pdf
- **PDF Location:** `documents/gears/fizz_bevel.pdf`
- **Description:** Complete 26-step bevel gear design calculator following the Fizz methodology, transitioning to spur guide steps 35-40 for material selection. Calculates geometry, cone angles, stress analysis, and required hardness values.
- **Complexity:** High (extensive geometry calculations, piecewise equations, chart lookups, transitions to spur guide)
- **Tabs to Include:**
  - [x] Procedure (26 bevel steps + 4 spur steps)
  - [x] General Equations (20+ reference equations)
  - [x] Variables (50+ variables with centralized management)

**Rationale for Tab Structure:**
- **Procedure Tab**: Sequential 26-step bevel design process with transition to spur steps 35-40
- **General Equations Tab**: Extensive reference equations for geometry, forces, and dimensions
- **Variables Tab**: 50+ variables across multiple categories requiring central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs - Basic** |
| P | Power | Power requirement | hp | User Input | - | No | Starting value |
| K<sub>O</sub> | Shock Factor | Overload factor | - | Table | Table 8-1 | Maybe | Based on application |
| n<sub>P</sub> | Pinion Speed | Driving pinion speed | rpm | User Input | - | No | Input shaft speed |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Integer value |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | User Input | - | No | Integer value |
| P<sub>d</sub> | Diametral Pitch | Teeth per inch | teeth/in | Dropdown | Table 8-2 | Maybe | Standard values |
| F | Face Width | Face width of gear | in | User Input | - | No | F ≤ A<sub>o</sub>/3 |
| **Manual Lookups - Quality & Factors** |
| Q<sub>v</sub> | Quality Number | Manufacturing quality | - | Table | Table 8-3 | Maybe | Based on velocity |
| K<sub>v</sub> | Dynamic Factor | Velocity factor | - | Table | Table 8-4 | Maybe | Function of Q<sub>v</sub>, V<sub>t</sub> |
| J<sub>P</sub> | Pinion Bending Factor | Geometry factor for pinion | - | Chart | Bevel charts | No | From N<sub>P</sub>, angles |
| J<sub>G</sub> | Gear Bending Factor | Geometry factor for gear | - | Chart | Bevel charts | No | From N<sub>G</sub>, angles |
| I | Contact Geometry Factor | Surface contact factor | - | Chart | Bevel charts | No | Based on geometry |
| K<sub>mb</sub> | Base Load Distribution | Base distribution factor | - | Graph | Figure 8-16 | No | Function of F |
| C<sub>s</sub> | Mounting Factor | Mounting condition | - | User Input | - | No | Default 1.0 |
| C<sub>xc</sub> | Crowning Factor | Tooth crowning adjustment | - | User Input | - | No | Default 1.0 |
| K<sub>B</sub> | Rim Thickness Factor | Backup ratio factor | - | User Input | - | No | Default 1.0 |
| N<sub>c</sub> | Design Life Cycles | Expected load cycles | cycles | User Input | - | No | For life factors |
| K<sub>L</sub> | Life Factor Bending | Bending life adjustment | - | Chart | Life charts | No | Function of N<sub>c</sub> |
| C<sub>L</sub> | Life Factor Contact | Contact life adjustment | - | Chart | Life charts | No | Function of N<sub>c</sub> |
| C<sub>p</sub> | Elastic Coefficient | Material property | psi<sup>0.5</sup> | Table | Table 8-8 | Maybe | 2300 for steel |
| K<sub>R</sub> | Reliability Factor | Design reliability | - | User Input | - | No | Default 1.0 (99%) |
| **Auto-Calculated - Geometry** |
| D<sub>P</sub> | Pinion Diameter | Pitch diameter of pinion | in | Equation | EQ1 | No | N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Gear Diameter | Pitch diameter of gear | in | Equation | EQ2 | No | N<sub>G</sub>/P<sub>d</sub> |
| m | Gear Ratio | Speed ratio | - | Equation | EQ3 | No | N<sub>G</sub>/N<sub>P</sub> |
| γ | Pinion Cone Angle | Pitch cone angle pinion | degrees | Equation | EQ4 | No | arctan(1/m) |
| Γ | Gear Cone Angle | Pitch cone angle gear | degrees | Equation | EQ5 | No | 90° - γ |
| A<sub>o</sub> | Outer Cone Distance | Cone distance | in | Equation | EQ6 | No | D<sub>P</sub>/(2sin(γ)) |
| V<sub>t</sub> | Pitch Line Velocity | Tangential velocity | ft/min | Equation | EQ7 | No | πD<sub>P</sub>n<sub>P</sub>/12 |
| **Auto-Calculated - Factors (Piecewise)** |
| K<sub>s</sub> | Size Factor | Size adjustment | - | Equation | EQ8 | No | Piecewise on P<sub>d</sub> |
| K<sub>m</sub> | Load Distribution Factor | Combined distribution | - | Equation | EQ9 | No | From K<sub>mb</sub>, C<sub>s</sub>, C<sub>xc</sub> |
| **Auto-Calculated - Loads & Stresses** |
| W<sub>t</sub> | Transmitted Load | Tangential force | lbf | Equation | EQ10 | No | 33000×P×K<sub>O</sub>/V<sub>t</sub> |
| σ<sub>tP</sub> | Pinion Bending Stress | Bending stress pinion | psi | Equation | EQ11 | No | Complex formula |
| σ<sub>tG</sub> | Gear Bending Stress | Bending stress gear | psi | Equation | EQ12 | No | Complex formula |
| σ<sub>c</sub> | Contact Stress | Hertzian contact stress | psi | Equation | EQ13 | No | With square root |
| **Auto-Calculated - Allowable Stresses** |
| σ<sub>acP</sub> | Allowable Pinion Bending | Adjusted pinion stress | psi | Equation | EQ14 | No | σ<sub>tP</sub>/K<sub>L</sub> |
| σ<sub>acG</sub> | Allowable Gear Bending | Adjusted gear stress | psi | Equation | EQ15 | No | σ<sub>tG</sub>/K<sub>L</sub> |
| σ<sub>ac</sub> | Allowable Contact | Adjusted contact stress | psi | Equation | EQ16 | No | σ<sub>c</sub>/C<sub>L</sub> |
| **Auto-Calculated - Design Strengths (Spur Steps)** |
| S<sub>acP</sub> | Design Pinion Strength | Final pinion strength | psi | Equation | EQ17 | No | σ<sub>acP</sub>×K<sub>R</sub> |
| S<sub>acG</sub> | Design Gear Strength | Final gear strength | psi | Equation | EQ18 | No | σ<sub>acG</sub>×K<sub>R</sub> |
| S<sub>ac</sub> | Design Contact Strength | Final contact strength | psi | Equation | EQ19 | No | σ<sub>ac</sub>×K<sub>R</sub> |
| **Auto-Calculated - Required Hardness** |
| HB<sub>P,b</sub> | Pinion Brinell (Bending) | Required hardness pinion | HB | Equation | EQ20 | No | (S<sub>acP</sub>/1000-12.8)/0.0773 |
| HB<sub>G,b</sub> | Gear Brinell (Bending) | Required hardness gear | HB | Equation | EQ21 | No | (S<sub>acG</sub>/1000-12.8)/0.0773 |
| HB<sub>c</sub> | Brinell (Contact) | Required hardness contact | HB | Equation | EQ22 | No | (S<sub>ac</sub>/1000-29.10)/0.322 |
| **Reference Equations Only (Not in main flow)** |
| d | Pinion Diameter | Same as D<sub>P</sub> | in | - | - | No | Notation variant |
| D | Gear Diameter | Same as D<sub>G</sub> | in | - | - | No | Notation variant |
| m<sub>G</sub> | Gear Ratio | Same as m | - | - | - | No | Notation variant |
| A<sub>oG</sub> | Gear Outer Cone Distance | Cone distance gear | in | Equation | Ref | No | D/(2sin(Γ)) |
| A<sub>oP</sub> | Pinion Outer Cone Distance | Cone distance pinion | in | Equation | Ref | No | d/(2sin(γ)) |
| F<sub>nom</sub> | Nominal Face Width | Recommended F | in | Equation | Ref | No | 0.3×A<sub>o</sub> |
| F<sub>max</sub> | Maximum Face Width | Upper limit for F | in | Equation | Ref | No | min(A<sub>o</sub>/3, 10/P<sub>d</sub>) |
| A<sub>m</sub> | Mean Cone Distance | Mid-face cone distance | in | Equation | Ref | No | A<sub>o</sub>-0.5F |
| p<sub>m</sub> | Mean Circular Pitch | Pitch at mid-face | in | Equation | Ref | No | πA<sub>m</sub>/(P<sub>d</sub>A<sub>o</sub>) |
| h | Mean Working Depth | Tooth working depth | in | Equation | Ref | No | 2/P<sub>d</sub> |
| c | Clearance | Tooth clearance | in | Equation | Ref | No | 0.188/P<sub>d</sub>+0.002 |
| h<sub>m</sub> | Mean Whole Depth | Total tooth depth | in | Equation | Ref | No | h+c |
| a<sub>G</sub> | Gear Mean Addendum | Gear tooth height | in | Equation | Ref | No | h-a<sub>P</sub> |
| a<sub>P</sub> | Pinion Mean Addendum | Pinion tooth height | in | Equation | Ref | No | Complex |
| b<sub>G</sub> | Gear Mean Dedendum | Gear root depth | in | Equation | Ref | No | h<sub>m</sub>-a<sub>G</sub> |
| b<sub>P</sub> | Pinion Mean Dedendum | Pinion root depth | in | Equation | Ref | No | h<sub>m</sub>-a<sub>P</sub> |
| δ<sub>G</sub> | Gear Dedendum Angle | Gear root angle | degrees | Equation | Ref | No | arctan(b<sub>G</sub>/A<sub>mG</sub>) |
| δ<sub>P</sub> | Pinion Dedendum Angle | Pinion root angle | degrees | Equation | Ref | No | arctan(b<sub>P</sub>/A<sub>mP</sub>) |
| a<sub>oG</sub> | Gear Outer Addendum | Outer addendum gear | in | Equation | Ref | No | a<sub>G</sub>+0.5F×tan(δ<sub>P</sub>) |
| a<sub>oP</sub> | Pinion Outer Addendum | Outer addendum pinion | in | Equation | Ref | No | a<sub>P</sub>+0.5F×tan(δ<sub>G</sub>) |
| D<sub>o</sub> | Gear Outside Diameter | Outer diameter gear | in | Equation | Ref | No | D+2a<sub>oG</sub>cos(Γ) |
| d<sub>o</sub> | Pinion Outside Diameter | Outer diameter pinion | in | Equation | Ref | No | d+2a<sub>oP</sub>cos(γ) |
| φ | Pressure Angle | Tooth pressure angle | degrees | - | - | No | Typically 20° |
| n<sub>G</sub> | Gear Speed | Output shaft speed | rpm | - | - | No | n<sub>P</sub>/m |
| T | Torque | Shaft torque | lbf·in | Equation | Ref | No | 63000P/n |
| r<sub>m</sub> | Pinion Mean Radius | Mean radius pinion | in | Equation | Ref | No | Complex |
| R<sub>m</sub> | Gear Mean Radius | Mean radius gear | in | Equation | Ref | No | Complex |
| W<sub>r</sub> | Radial Load | Radial force component | lbf | Equation | Ref | No | W<sub>t</sub>tan(φ)cos(γ) |
| W<sub>a</sub> | Axial Load | Axial force component | lbf | Equation | Ref | No | W<sub>t</sub>tan(φ)sin(γ) |

---

## 3. Equations

### Main Procedure Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `D_P = \frac{N_P}{P_d}` | Pinion Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | Step 1 |
| EQ2 | `D_G = \frac{N_G}{P_d}` | Gear Pitch Diameter | N<sub>G</sub>, P<sub>d</sub> | D<sub>G</sub> | Step 2 |
| EQ3 | `m = \frac{N_G}{N_P}` | Gear Ratio | N<sub>G</sub>, N<sub>P</sub> | m | Step 3 |
| EQ4 | `\gamma = \arctan\left(\frac{1}{m}\right)` | Pinion Cone Angle | m | γ | Step 4, degrees |
| EQ5 | `\Gamma = 90° - \gamma` | Gear Cone Angle | γ | Γ | Step 4, degrees |
| EQ6 | `A_o = \frac{D_P}{2\sin(\gamma)}` | Outer Cone Distance | D<sub>P</sub>, γ | A<sub>o</sub> | Step 5-6 |
| EQ7 | `V_t = \frac{\pi D_P n_P}{12}` | Pitch Line Velocity | D<sub>P</sub>, n<sub>P</sub> | V<sub>t</sub> | Step 7 |
| EQ8a | `K_s = 0.4867 + \frac{0.2132}{P_d}` | Size Factor (Fine) | P<sub>d</sub> | K<sub>s</sub> | If P<sub>d</sub> ≥ 5 |
| EQ8b | `K_s = 0.5 - 0.007 \cdot P_d` | Size Factor (Coarse) | P<sub>d</sub> | K<sub>s</sub> | If P<sub>d</sub> < 5 |
| EQ9 | `K_m = K_{mb} + C_s(K_{mb} - 1)C_{xc}` | Load Distribution Factor | K<sub>mb</sub>, C<sub>s</sub>, C<sub>xc</sub> | K<sub>m</sub> | Step 15-17 |
| EQ10 | `W_t = \frac{33000 \times P \times K_O}{V_t}` | Transmitted Load | P, K<sub>O</sub>, V<sub>t</sub> | W<sub>t</sub> | Step 21 |
| EQ11 | `\sigma_{tP} = \frac{W_t P_d}{F J_P} K_O K_m K_s K_B K_v` | Pinion Bending Stress | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>P</sub>, K<sub>O</sub>, K<sub>m</sub>, K<sub>s</sub>, K<sub>B</sub>, K<sub>v</sub> | σ<sub>tP</sub> | Step 22 |
| EQ12 | `\sigma_{tG} = \frac{W_t P_d}{F J_G} K_O K_m K_s K_B K_v` | Gear Bending Stress | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>G</sub>, K<sub>O</sub>, K<sub>m</sub>, K<sub>s</sub>, K<sub>B</sub>, K<sub>v</sub> | σ<sub>tG</sub> | Step 23 |
| EQ13 | `\sigma_c = C_p \sqrt{\frac{W_t K_O K_m K_s K_v}{F D_P I}}` | Contact Stress | C<sub>p</sub>, W<sub>t</sub>, K<sub>O</sub>, K<sub>m</sub>, K<sub>s</sub>, K<sub>v</sub>, F, D<sub>P</sub>, I | σ<sub>c</sub> | Step 24-25 |
| EQ14 | `\sigma_{acP} = \frac{\sigma_{tP}}{K_L}` | Allowable Pinion Bending | σ<sub>tP</sub>, K<sub>L</sub> | σ<sub>acP</sub> | Step 26 |
| EQ15 | `\sigma_{acG} = \frac{\sigma_{tG}}{K_L}` | Allowable Gear Bending | σ<sub>tG</sub>, K<sub>L</sub> | σ<sub>acG</sub> | Step 26 |
| EQ16 | `\sigma_{ac} = \frac{\sigma_c}{C_L}` | Allowable Contact | σ<sub>c</sub>, C<sub>L</sub> | σ<sub>ac</sub> | Step 26 |
| EQ17 | `S_{acP} = \sigma_{acP} \times K_R` | Design Pinion Strength | σ<sub>acP</sub>, K<sub>R</sub> | S<sub>acP</sub> | Spur Step 38 |
| EQ18 | `S_{acG} = \sigma_{acG} \times K_R` | Design Gear Strength | σ<sub>acG</sub>, K<sub>R</sub> | S<sub>acG</sub> | Spur Step 38 |
| EQ19 | `S_{ac} = \sigma_{ac} \times K_R` | Design Contact Strength | σ<sub>ac</sub>, K<sub>R</sub> | S<sub>ac</sub> | Spur Step 38 |
| EQ20 | `HB_{P,bending} = \frac{S_{acP}/1000 - 12.8}{0.0773}` | Required Pinion Hardness | S<sub>acP</sub> | HB<sub>P,b</sub> | Spur Step 39-40 |
| EQ21 | `HB_{G,bending} = \frac{S_{acG}/1000 - 12.8}{0.0773}` | Required Gear Hardness | S<sub>acG</sub> | HB<sub>G,b</sub> | Spur Step 39-40 |
| EQ22 | `HB_{contact} = \frac{S_{ac}/1000 - 29.10}{0.322}` | Required Contact Hardness | S<sub>ac</sub> | HB<sub>c</sub> | Spur Step 39-40 |

### General Reference Equations

| ID | LaTeX Formula | Description | Notes |
|----|---------------|-------------|-------|
| REF1 | `P_d = \frac{N_P}{d} = \frac{N_G}{D}` | Diametral Pitch | Alternative forms |
| REF2 | `m_G = \frac{N_G}{N_P} = \frac{D}{d}` | Gear Ratio | Alternative forms |
| REF3 | `\tan \gamma = \frac{N_P}{N_G} = \frac{d}{D}` | Pinion Cone Angle | Tangent form |
| REF4 | `\tan \Gamma = \frac{N_G}{N_P} = \frac{D}{d}` | Gear Cone Angle | Tangent form |
| REF5 | `A_{oG} = \frac{D}{2 \sin \Gamma}, \quad A_{oP} = \frac{d}{2 \sin \gamma}` | Outer Cone Distances | Both gears |
| REF6 | `F_{nom} = 0.3 A_o` | Nominal Face Width | Recommended |
| REF7 | `F_{max} = \min\left(\frac{A_o}{3}, \frac{10}{P_d}\right)` | Maximum Face Width | Upper limit |
| REF8 | `A_m = A_o - 0.5F` | Mean Cone Distance | Mid-face |
| REF9 | `p_m = \frac{\pi A_m}{P_d A_o}` | Mean Circular Pitch | At mean cone |
| REF10 | `h = \frac{2}{P_d}` | Mean Working Depth | Standard |
| REF11 | `c = \frac{0.188}{P_d} + 0.002` | Clearance | Standard |
| REF12 | `h_m = h + c` | Mean Whole Depth | Total depth |
| REF13 | `a_P = h - a_G` | Pinion Mean Addendum | Complement |
| REF14 | `b_G = h_m - a_G` | Gear Mean Dedendum | From whole depth |
| REF15 | `b_P = h_m - a_P` | Pinion Mean Dedendum | From whole depth |
| REF16 | `\tan \delta_G = \frac{b_G}{A_{mG}}` | Gear Dedendum Angle | Root angle |
| REF17 | `\tan \delta_P = \frac{b_P}{A_{mP}}` | Pinion Dedendum Angle | Root angle |
| REF18 | `a_{oG} = a_G + 0.5F \tan \delta_P` | Gear Outer Addendum | At large end |
| REF19 | `a_{oP} = a_P + 0.5F \tan \delta_G` | Pinion Outer Addendum | At large end |
| REF20 | `D_o = D + 2a_{oG}\cos \Gamma` | Gear Outside Diameter | Outer limit |
| REF21 | `d_o = d + 2a_{oP}\cos \gamma` | Pinion Outside Diameter | Outer limit |
| REF22 | `v_t = \frac{\pi Dn_G}{12} = \frac{\pi dn_P}{12}` | Pitch Line Speed | ft/min |
| REF23 | `T = \frac{63000P}{n}` | Torque | lbf·in |
| REF24 | `r_m = \frac{d_F}{2} - \frac{F}{2\sin \gamma}` | Pinion Mean Radius | Complex |
| REF25 | `R_m = \frac{D_F}{2} - \frac{F}{2\sin \Gamma}` | Gear Mean Radius | Complex |
| REF26 | `W_t = \frac{T_P}{r_m} = \frac{T_G}{R_m}` | Transmitted Load | From torque |
| REF27 | `W_r = W_t \tan \phi \cos \gamma = W_t \tan \phi \cos \Gamma` | Radial Load | Both forms |
| REF28 | `W_a = W_t \tan \phi \sin \gamma = W_t \tan \phi \sin \Gamma` | Axial Load | Both forms |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Bevel-Specific Steps (1-26)** |
| Given | Input Basic Parameters | User Input | P [hp], K<sub>O</sub> (Table 8-1), n<sub>P</sub> [rpm], N<sub>P</sub>, N<sub>G</sub>, P<sub>d</sub> (Table 8-2), F [in] | Seven initial inputs |
| 1 | Calculate Pinion Diameter | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> (EQ1) | Auto-calculated |
| 2 | Calculate Gear Diameter | Equation | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ2) | Auto-calculated |
| 3 | Calculate Gear Ratio | Equation | m = N<sub>G</sub>/N<sub>P</sub> (EQ3) | Auto-calculated |
| 4 | Calculate Cone Angles | Equation | γ = arctan(1/m), Γ = 90° - γ (EQ4, EQ5) | Both angles in degrees |
| 5-6 | Calculate Cone Distance | Equation | A<sub>o</sub> = D<sub>P</sub>/(2sin(γ)) (EQ6) | Check F ≤ A<sub>o</sub>/3 |
| 7 | Calculate Velocity | Equation | V<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 (EQ7) | ft/min |
| 8 | Select Quality Number | Table | Choose Q<sub>v</sub> from Table 8-3 | Based on V<sub>t</sub> |
| 9 | Calculate Dynamic Factor | Table | K<sub>v</sub> from Table 8-4 using Q<sub>v</sub>, V<sub>t</sub> | Formula: [(A+√V<sub>t</sub>)/A]<sup>B</sup> |
| 10 | Find Pinion Bending Factor | Chart | Look up J<sub>P</sub> from bevel charts | Based on N<sub>P</sub>, geometry |
| 11 | Find Gear Bending Factor | Chart | Look up J<sub>G</sub> from bevel charts | Based on N<sub>G</sub>, geometry |
| 11b | Find Contact Geometry Factor | Chart | Look up I from bevel charts | Surface contact factor |
| 12-14 | Calculate Size Factor | Equation | K<sub>s</sub> using EQ8a or EQ8b | **Piecewise on P<sub>d</sub>** |
| 15 | Find Base Load Distribution | Graph | Look up K<sub>mb</sub> from Figure 8-16 | Function of F |
| 16 | Input Mounting Factor | User Input | C<sub>s</sub> (default 1.0) | Mounting condition |
| 16b | Input Crowning Factor | User Input | C<sub>xc</sub> (default 1.0) | Tooth crowning |
| 17 | Calculate Load Distribution | Equation | K<sub>m</sub> = K<sub>mb</sub> + C<sub>s</sub>(K<sub>mb</sub>-1)C<sub>xc</sub> (EQ9) | Auto-calculated |
| 18 | Input Rim Thickness Factor | User Input | K<sub>B</sub> (default 1.0) | Backup ratio |
| 19 | Input Design Life | User Input | N<sub>c</sub> [cycles] | Expected cycles |
| 20 | Find Life Factors | Chart | Look up K<sub>L</sub>, C<sub>L</sub> from life charts | Based on N<sub>c</sub> |
| 21 | Calculate Transmitted Load | Equation | W<sub>t</sub> = 33000×P×K<sub>O</sub>/V<sub>t</sub> (EQ10) | Auto-calculated |
| 22 | Calculate Pinion Bending Stress | Equation | σ<sub>tP</sub> = (W<sub>t</sub>P<sub>d</sub>)/(FJ<sub>P</sub>) × K<sub>O</sub>K<sub>m</sub>K<sub>s</sub>K<sub>B</sub>K<sub>v</sub> (EQ11) | Auto-calculated |
| 23 | Calculate Gear Bending Stress | Equation | σ<sub>tG</sub> = (W<sub>t</sub>P<sub>d</sub>)/(FJ<sub>G</sub>) × K<sub>O</sub>K<sub>m</sub>K<sub>s</sub>K<sub>B</sub>K<sub>v</sub> (EQ12) | Auto-calculated |
| 24 | Input Elastic Coefficient | Table | C<sub>p</sub> from Table 8-8 | 2300 for steel |
| 25 | Calculate Contact Stress | Equation | σ<sub>c</sub> = C<sub>p</sub>√((W<sub>t</sub>K<sub>O</sub>K<sub>m</sub>K<sub>s</sub>K<sub>v</sub>)/(FD<sub>P</sub>I)) (EQ13) | Auto-calculated |
| 26 | Calculate Allowable Stresses | Equation | σ<sub>acP</sub> = σ<sub>tP</sub>/K<sub>L</sub>, σ<sub>acG</sub> = σ<sub>tG</sub>/K<sub>L</sub>, σ<sub>ac</sub> = σ<sub>c</sub>/C<sub>L</sub> (EQ14-16) | Three values |
| **Transition to Spur Guide (Steps 35-40)** |
| Divider | Continue with Spur Steps 35-40 | Transition | Visual divider in UI | Procedural note |
| 37 | Input Reliability Factor | User Input | K<sub>R</sub> (default 1.0) | 99% reliability |
| 38 | Calculate Design Allowable Strengths | Equation | S<sub>acP</sub> = σ<sub>acP</sub>×K<sub>R</sub>, S<sub>acG</sub> = σ<sub>acG</sub>×K<sub>R</sub>, S<sub>ac</sub> = σ<sub>ac</sub>×K<sub>R</sub> (EQ17-19) | Three final strengths |
| 39-40a | Calculate Required Pinion Hardness | Equation | HB<sub>P,b</sub> = (S<sub>acP</sub>/1000 - 12.8)/0.0773 (EQ20) | Brinell hardness |
| 39-40b | Calculate Required Gear Hardness | Equation | HB<sub>G,b</sub> = (S<sub>acG</sub>/1000 - 12.8)/0.0773 (EQ21) | Brinell hardness |
| 39-40c | Calculate Required Contact Hardness | Equation | HB<sub>c</sub> = (S<sub>ac</sub>/1000 - 29.10)/0.322 (EQ22) | Brinell hardness |
| Final | Select Material | Table | Choose from Table 8-6 that meets/exceeds all HB values | Material selection |

---

## 5. Additional Notes

### Validation Rules
- **Integer Teeth**: N<sub>P</sub>, N<sub>G</sub> must be positive integers
- **Face Width Constraint**: F ≤ A<sub>o</sub>/3 for best results, F ≤ min(A<sub>o</sub>/3, 10/P<sub>d</sub>) absolute max
- **Positive Values**: All diameters, speeds, power must be > 0
- **Angle Range**: Cone angles 0° < γ, Γ < 90°, typically γ + Γ = 90° for 90° shaft angle
- **Standard Diametral Pitch**: Choose from Table 8-2 standard values
- **Factor Ranges**: Most K and C factors typically 0.5 to 2.0

### Standard Values / Constants
- **Pressure Angle φ**: Typically 20° for bevel gears
- **Elastic Coefficient C<sub>p</sub>**: 2300 psi<sup>0.5</sup> for steel-on-steel
- **Reliability Factor K<sub>R</sub>**: 1.0 for 99% reliability, 1.25 for 99.9%, 0.85 for 90%
- **Mounting/Crowning Factors**: C<sub>s</sub> = C<sub>xc</sub> = 1.0 (default, well-aligned)
- **Rim Thickness Factor K<sub>B</sub>**: 1.0 (default, solid gear)
- **π**: Use Math.PI (3.14159...)
- **Shaft Angle**: 90° assumed (perpendicular shafts) unless otherwise specified

### Piecewise Equation Details

**Size Factor K<sub>s</sub> (Step 12-14):**
```
IF P_d >= 5:
    K_s = 0.4867 + 0.2132/P_d    (Fine pitch)
ELSE:
    K_s = 0.5 - 0.007 * P_d      (Coarse pitch)
```

**Threshold**: P<sub>d</sub> = 5 teeth/in
- **Indicator**: Display which formula is active and current P<sub>d</sub> value
- **Example**: "K<sub>s</sub> = 0.4867 + 0.2132/P<sub>d</sub> (P<sub>d</sub> = 8.00 ≥ 5)"

### Dropdown Options

**Diametral Pitch P<sub>d</sub> (Table 8-2 - Standard Values):**
- 2, 2.5, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96, 120, 150, 200

**Quality Number Q<sub>v</sub> (Table 8-3):**
- 5 (Commercial)
- 6 (Precision)
- 7 (High precision)
- 8-10 (Ultra precision)
- 11-12 (Aerospace)

**Shock Factor K<sub>O</sub> (Table 8-1 - Common Values):**
| Load Type | Uniform | Light Shock | Medium Shock | Heavy Shock |
|-----------|---------|-------------|--------------|-------------|
| Value | 1.00 | 1.25 | 1.50 | 1.75-2.00 |

### Future Enhancement Opportunities
- **Table 8-1**: Implement dropdown for K<sub>O</sub> based on power source and driven machine
- **Table 8-2**: Dropdown for standard P<sub>d</sub> values
- **Table 8-3**: Automatic Q<sub>v</sub> selection based on V<sub>t</sub> ranges
- **Table 8-4**: Implement K<sub>v</sub> calculation formula with A, B constants
- **Table 8-8**: Material properties lookup (C<sub>p</sub>, density, modulus)
- **Figure 8-16**: Graph digitization for automatic K<sub>mb</sub> lookup
- **Bevel Charts**: Interactive chart lookups for J<sub>P</sub>, J<sub>G</sub>, I factors
- **Life Factor Charts**: Automatic K<sub>L</sub>, C<sub>L</sub> interpolation from N<sub>c</sub>
- **Table 8-6**: Material selection assistant showing options that meet HB requirements
- **Geometry Validation**: Auto-check F ≤ A<sub>o</sub>/3 constraint and warn user
- **Design Iteration**: Allow user to compare multiple P<sub>d</sub>, N<sub>P</sub>, N<sub>G</sub> combinations

### Special Considerations

**Multi-Source Information:**
- This calculator combines bevel gear design methodology (steps 1-26) with spur gear material selection (steps 35-40)
- The transition point is clearly marked with a visual divider
- Some variables like φ (pressure angle) are implicit in chart lookups

**Chart/Graph Dependencies:**
- J<sub>P</sub>, J<sub>G</sub>, I: Require bevel-specific geometry charts (not simple formulas)
- K<sub>mb</sub>: Requires graph interpolation from Figure 8-16
- K<sub>L</sub>, C<sub>L</sub>: Require life factor charts with multiple curves
- These are the most complex manual lookups in the calculator

**Stress Analysis:**
- Three separate failure modes: pinion bending, gear bending, contact fatigue
- Material must satisfy ALL THREE hardness requirements (HB<sub>P,b</sub>, HB<sub>G,b</sub>, HB<sub>c</sub>)
- Contact stress typically drives material selection (highest HB requirement)

**Dynamic Factor Calculation:**
- K<sub>v</sub> formula from Table 8-4: `K_v = [(A + √V_t)/A]^B`
- A and B constants depend on Q<sub>v</sub> (quality number)
- Example: Q<sub>v</sub> = 6: A = 56, B = 0.25

**Unit Consistency:**
- Velocity: ft/min
- Diameters, distances: inches
- Angles: degrees (convert to radians for trig functions)
- Stress: psi
- Power: hp
- Torque: lbf·in
- Force: lbf

### Calculation Order / Dependencies

```
[User Inputs]
P, KO, nP, NP, NG, Pd, F

[Step 1-6: Basic Geometry]
DP = NP/Pd
DG = NG/Pd
m = NG/NP
γ = arctan(1/m)
Γ = 90° - γ
Ao = DP/(2sin(γ))
Validate: F ≤ Ao/3

[Step 7-9: Velocity & Dynamic]
Vt = π·DP·nP/12
Qv (user/table lookup)
Kv (table/formula from Qv, Vt)

[Step 10-11: Geometry Factors]
JP (chart lookup)
JG (chart lookup)
I (chart lookup)

[Step 12-14: Size Factor - PIECEWISE]
IF Pd >= 5:
    Ks = 0.4867 + 0.2132/Pd
ELSE:
    Ks = 0.5 - 0.007·Pd

[Step 15-18: Load Distribution]
Kmb (graph lookup from F)
Cs (user, default 1.0)
Cxc (user, default 1.0)
Km = Kmb + Cs(Kmb-1)Cxc
KB (user, default 1.0)

[Step 19-20: Life Factors]
Nc (user input)
KL (chart lookup from Nc)
CL (chart lookup from Nc)

[Step 21-23: Loads & Bending Stress]
Wt = 33000·P·KO/Vt
σtP = (Wt·Pd)/(F·JP) · KO·Km·Ks·KB·Kv
σtG = (Wt·Pd)/(F·JG) · KO·Km·Ks·KB·Kv

[Step 24-25: Contact Stress]
Cp (table lookup, 2300 for steel)
σc = Cp·√((Wt·KO·Km·Ks·Kv)/(F·DP·I))

[Step 26: Allowable Stresses]
σacP = σtP/KL
σacG = σtG/KL
σac = σc/CL

[Spur Step 37-38: Design Strengths]
KR (user, default 1.0)
SacP = σacP × KR
SacG = σacG × KR
Sac = σac × KR

[Spur Step 39-40: Required Hardness]
HB_P,b = (SacP/1000 - 12.8)/0.0773
HB_G,b = (SacG/1000 - 12.8)/0.0773
HB_c = (Sac/1000 - 29.10)/0.322

[Final: Material Selection]
Select material from Table 8-6 where:
    HB >= max(HB_P,b, HB_G,b, HB_c)
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized (50+ variables)
- [x] Source references noted for all table/chart variables
- [x] All main procedure equations extracted (22 equations)
- [x] All reference equations documented (28 equations)
- [x] LaTeX formulas converted and verified
- [x] Procedure steps documented (26 bevel + 4 spur steps)
- [x] Piecewise equations explained (K<sub>s</sub> on P<sub>d</sub>)
- [x] Validation rules documented
- [x] Standard values and constants listed
- [x] Dropdown options specified
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Special considerations documented
- [x] Calculation order/dependencies mapped
- [x] Unit consistency verified
- [x] Transition to spur guide documented
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Notes:**

This calculator has **high complexity** due to:
1. **Extensive chart lookups**: J<sub>P</sub>, J<sub>G</sub>, I, K<sub>mb</sub>, K<sub>L</sub>, C<sub>L</sub> all require manual lookups
2. **Piecewise equation**: K<sub>s</sub> formula changes at P<sub>d</sub> = 5
3. **Cross-guide transition**: Steps 1-26 from bevel guide, steps 35-40 from spur guide
4. **50+ variables**: Requires careful synchronization across three tabs
5. **Three failure modes**: Must track pinion bending, gear bending, and contact stresses independently

**Current Implementation:**
- All 22 main procedure equations implemented in JavaScript `calculate()` function
- Piecewise K<sub>s</sub> with visual indicator showing active formula
- Three-tier input system: white (user), yellow (manual lookup), green (auto-calculated)
- Three-tab structure: Procedure, General Equations, Variables
- Session persistence via LocalStorage
- Real-time calculation on input change

**Recommended Manual Lookups** (with info boxes):
- K<sub>O</sub> from Table 8-1
- P<sub>d</sub> selection from Table 8-2
- Q<sub>v</sub> from Table 8-3
- K<sub>v</sub> formula parameters from Table 8-4
- J<sub>P</sub>, J<sub>G</sub>, I from bevel gear charts
- K<sub>mb</sub> from Figure 8-16
- K<sub>L</sub>, C<sub>L</sub> from life factor charts
- C<sub>p</sub> from Table 8-8
- Material selection from Table 8-6 based on HB requirements

**Known Limitations:**
- General Equations tab has 28+ reference equations for geometry details (not auto-calculated in main flow)
- Some reference variables (a<sub>P</sub>, a<sub>G</sub>, tooth dimensions) require additional formulas not in main procedure
- Chart lookups cannot be automated without digitizing PDF charts/graphs

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
