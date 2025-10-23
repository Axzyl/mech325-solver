# Calculator Specification: Helical Gear Design (Fizz Guide)

---

## 1. Metadata

- **Calculator Name:** Helical Gear Design Calculator (Fizz Guide)
- **Source PDF:** fizz_helical.pdf
- **PDF Location:** `documents/gears/fizz_helical.pdf`
- **Description:** Calculates helical gear design parameters using a hybrid approach: helical-specific steps 1-13 followed by spur guide steps 32-40 for stress analysis and material selection. Includes normal/transverse pressure angle relationships, pitch conversions, geometry factors, and hardness requirements.
- **Complexity:** High (helical angle transformations, manual geometry factor lookups, hybrid procedure)
- **Tabs to Include:**
  - [x] Procedure (Steps 1-13 helical, then steps 32-40 spur)
  - [x] General Equations (15+ reference equations)
  - [x] Variables (40+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: Sequential 13-step helical procedure + 9 spur stress analysis steps
- **General Equations Tab**: 15+ equations covering angle relationships, pitch parameters, forces, and motion
- **Variables Tab**: 40+ variables across basic parameters, pitch parameters, power/motion, and forces

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs - Helical Parameters** |
| P | Input Power | Power requirement | hp | User Input | - | No | Starting value |
| K<sub>O</sub> | Shock Factor | Overload factor | - | Table | Table 8-1 in PDF | No | Load classification |
| n<sub>P</sub> | Pinion Speed | Driving gear speed | rpm | User Input | - | No | Faster shaft |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Integer, ≥17 recommended |
| N<sub>G</sub> | Gear Teeth | Number of teeth on gear | teeth | User Input | - | No | Integer |
| ψ | Helix Angle | Helix angle | degrees | User Input | - | No | Typically 15-30° |
| P<sub>nd</sub> | Normal Diametral Pitch | Normal diametral pitch | teeth/in | Table | Table 8-2 in PDF | Maybe | Standard values |
| F | Face Width | Gear face width | in | User Input | - | No | Typically 8-16 × circular pitch |
| φ<sub>n</sub> | Normal Pressure Angle | Normal pressure angle | degrees | User Input | - | No | Typically 20° |
| **Manual Lookups - Quality/Factors** |
| Q<sub>v</sub> | Quality Number | Transmission quality level | - | Table | Table 8-3 | No | Based on velocity |
| K<sub>v</sub> | Dynamic Factor | Velocity factor | - | Table | Table 8-4 | No | Function of Q<sub>v</sub> and V<sub>t</sub> |
| J<sub>P</sub> | Bending Factor (Pinion) | Geometry factor for bending (pinion) | - | Figure | Figures 8-16 to 8-18 | No | Based on N<sub>P</sub>, N<sub>G</sub>, φ<sub>t</sub> |
| J<sub>G</sub> | Bending Factor (Gear) | Geometry factor for bending (gear) | - | Figure | Figures 8-16 to 8-18 | No | Based on N<sub>P</sub>, N<sub>G</sub>, φ<sub>t</sub> |
| I | Contact Geometry Factor | Geometry factor for contact | - | Figure | Figures 8-16 to 8-18 | No | Based on N<sub>P</sub>, N<sub>G</sub>, φ<sub>t</sub> |
| **User Inputs - Life/Material Factors (Spur Steps 32-34)** |
| N<sub>c</sub> | Design Life Cycles | Number of stress cycles | cycles | User Input | - | No | Calculate or estimate |
| Y<sub>N</sub> | Stress Cycle Factor (Bending) | Life factor for bending | - | Figure | Figure 8-14 in spur PDF | No | From N<sub>c</sub> |
| Z<sub>N</sub> | Stress Cycle Factor (Contact) | Life factor for contact | - | Figure | Figure 8-15 in spur PDF | No | From N<sub>c</sub> |
| K<sub>m</sub> | Load Distribution Factor | Load distribution factor | - | User Input | - | No | Typically 1.0-1.6 |
| K<sub>s</sub> | Size Factor | Size adjustment factor | - | User Input | - | No | Typically 1.0 |
| K<sub>B</sub> | Rim Thickness Factor | Backup ratio factor | - | User Input | - | No | Typically 1.0 |
| C<sub>p</sub> | Elastic Coefficient | Material elastic coefficient | √psi | Table | Table 8-8 | No | 2300 for steel |
| K<sub>R</sub> | Reliability Factor | Reliability factor | - | User Input | - | No | 1.0 for 99% reliability |
| **Auto-Calculated - Geometry** |
| P<sub>d</sub> | Transverse Diametral Pitch | Transverse diametral pitch | teeth/in | Equation | EQ3 | No | P<sub>d</sub> = P<sub>nd</sub> cos(ψ) |
| D<sub>P</sub> | Pinion Pitch Diameter | Pitch diameter of pinion | in | Equation | EQ4 | No | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> |
| D<sub>G</sub> | Gear Pitch Diameter | Pitch diameter of gear | in | Equation | EQ5 | No | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> |
| C | Center Distance | Center distance | in | Equation | EQ6 | No | C = (D<sub>P</sub> + D<sub>G</sub>)/2 |
| V<sub>t</sub> | Pitch Line Velocity | Tangential velocity | ft/min | Equation | EQ7 | No | V<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 |
| φ<sub>t</sub> | Transverse Pressure Angle | Transverse pressure angle | degrees | Equation | EQ12 | No | arctan(tan(φ<sub>n</sub>)/cos(ψ)) |
| **Auto-Calculated - Forces and Stresses (Spur Steps 35-36)** |
| W<sub>t</sub> | Transmitted Load | Tangential force | lbf | Equation | EQ35a | No | W<sub>t</sub> = 33000 × P<sub>des</sub>/V<sub>t</sub> |
| σ<sub>tP</sub> | Bending Stress (Pinion) | Actual bending stress (pinion) | psi | Equation | EQ35b | No | Complex formula |
| σ<sub>tG</sub> | Bending Stress (Gear) | Actual bending stress (gear) | psi | Equation | EQ35c | No | Complex formula |
| σ<sub>acP</sub> | Allowable Bending Stress (Pinion) | Allowable bending stress (pinion) | psi | Equation | EQ35d | No | σ<sub>tP</sub>/Y<sub>N</sub> |
| σ<sub>acG</sub> | Allowable Bending Stress (Gear) | Allowable bending stress (gear) | psi | Equation | EQ35e | No | σ<sub>tG</sub>/Y<sub>N</sub> |
| σ<sub>c</sub> | Contact Stress | Actual contact stress | psi | Equation | EQ36a | No | Hertzian contact formula |
| σ<sub>ac</sub> | Allowable Contact Stress | Allowable contact stress | psi | Equation | EQ36b | No | σ<sub>c</sub>/Z<sub>N</sub> |
| **Auto-Calculated - Design Strengths (Spur Step 38)** |
| S<sub>acP</sub> | Design Bending Strength (Pinion) | Required bending strength (pinion) | psi | Equation | EQ38a | No | σ<sub>acP</sub> × K<sub>R</sub> |
| S<sub>acG</sub> | Design Bending Strength (Gear) | Required bending strength (gear) | psi | Equation | EQ38b | No | σ<sub>acG</sub> × K<sub>R</sub> |
| S<sub>ac</sub> | Design Contact Strength | Required contact strength | psi | Equation | EQ38c | No | σ<sub>ac</sub> × K<sub>R</sub> |
| **Auto-Calculated - Hardness (Spur Steps 39-40)** |
| HB<sub>P,b</sub> | Required Hardness (Pinion Bending) | Brinell hardness for pinion bending | HB | Equation | EQ39a | No | (S<sub>acP</sub>/1000 - 12.8)/0.0773 |
| HB<sub>G,b</sub> | Required Hardness (Gear Bending) | Brinell hardness for gear bending | HB | Equation | EQ39b | No | (S<sub>acG</sub>/1000 - 12.8)/0.0773 |
| HB<sub>c</sub> | Required Hardness (Contact) | Brinell hardness for contact | HB | Equation | EQ39c | No | (S<sub>ac</sub>/1000 - 29.10)/0.322 |
| **General Equations Tab Variables** |
| p<sub>t</sub> | Transverse Circular Pitch | Transverse circular pitch | in | Equation | EQG1 | No | π/P<sub>d</sub> |
| p<sub>n</sub> | Normal Circular Pitch | Normal circular pitch | in | Equation | EQG2 | No | p<sub>t</sub> cos(ψ) |
| p<sub>x</sub> | Axial Pitch | Axial pitch | in | Equation | EQG3 | No | p<sub>t</sub>/tan(ψ) |
| m | Metric Module | Metric module (transverse) | mm | Equation | EQG5 | No | D/N |
| m<sub>n</sub> | Normal Metric Module | Normal metric module | mm | Equation | EQG6 | No | m cos(ψ) |
| T | Torque | Torque | lbf·in | Equation | EQG7 | No | 63000P/n |
| v<sub>t</sub> | Pitch Line Speed (alt) | Pitch line speed | ft/min | Equation | EQG8 | No | πDn/12 |
| W<sub>r</sub> | Radial Force | Radial force | lbf | Equation | EQG9 | No | W<sub>t</sub> tan(φ<sub>t</sub>) |
| W<sub>x</sub>, W<sub>a</sub> | Axial Force | Axial force (thrust) | lbf | Equation | EQG10 | No | W<sub>t</sub> tan(ψ) |
| FCR | Face Contact Ratio | Face contact ratio | - | Equation | EQG11 | No | F/p<sub>x</sub>, should be > 2.0 |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Procedure Tab - Helical Steps 1-13** |
| EQ3 | `P_d = P_{nd} \cos(\psi)` | Transverse Diametral Pitch | P<sub>nd</sub>, ψ | P<sub>d</sub> | Step 3 |
| EQ4 | `D_P = \frac{N_P}{P_d}` | Pinion Pitch Diameter | N<sub>P</sub>, P<sub>d</sub> | D<sub>P</sub> | Step 4 |
| EQ5 | `D_G = \frac{N_G}{P_d}` | Gear Pitch Diameter | N<sub>G</sub>, P<sub>d</sub> | D<sub>G</sub> | Step 5 |
| EQ6 | `C = \frac{D_P + D_G}{2}` | Center Distance | D<sub>P</sub>, D<sub>G</sub> | C | Step 6 |
| EQ7 | `V_t = \frac{\pi D_P n_P}{12}` | Pitch Line Velocity | D<sub>P</sub>, n<sub>P</sub> | V<sub>t</sub> | Step 7 |
| EQ12 | `\phi_t = \arctan\left(\frac{\tan(\phi_n)}{\cos(\psi)}\right)` | Transverse Pressure Angle | φ<sub>n</sub>, ψ | φ<sub>t</sub> | Step 12 |
| **Procedure Tab - Spur Steps 32-40** |
| EQ35a | `W_t = \frac{33000 \times P_{des}}{V_t}` | Transmitted Load | P, K<sub>O</sub>, V<sub>t</sub> | W<sub>t</sub> | P<sub>des</sub> = P × K<sub>O</sub> |
| EQ35b | `\sigma_{tP} = \frac{W_t P_d}{F J_P} K_O K_s K_m K_B K_v` | Bending Stress (Pinion) | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>P</sub>, K<sub>O</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub> | σ<sub>tP</sub> | Step 35 |
| EQ35c | `\sigma_{tG} = \frac{W_t P_d}{F J_G} K_O K_s K_m K_B K_v` | Bending Stress (Gear) | W<sub>t</sub>, P<sub>d</sub>, F, J<sub>G</sub>, K<sub>O</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>B</sub>, K<sub>v</sub> | σ<sub>tG</sub> | Step 35 |
| EQ35d | `\sigma_{acP} = \frac{\sigma_{tP}}{Y_N}` | Allowable Bending Stress (Pinion) | σ<sub>tP</sub>, Y<sub>N</sub> | σ<sub>acP</sub> | Step 35 |
| EQ35e | `\sigma_{acG} = \frac{\sigma_{tG}}{Y_N}` | Allowable Bending Stress (Gear) | σ<sub>tG</sub>, Y<sub>N</sub> | σ<sub>acG</sub> | Step 35 |
| EQ36a | `\sigma_c = C_p \sqrt{\frac{W_t K_O K_s K_m K_v}{F D_P I}}` | Contact Stress | C<sub>p</sub>, W<sub>t</sub>, K<sub>O</sub>, K<sub>s</sub>, K<sub>m</sub>, K<sub>v</sub>, F, D<sub>P</sub>, I | σ<sub>c</sub> | Step 36 |
| EQ36b | `\sigma_{ac} = \frac{\sigma_c}{Z_N}` | Allowable Contact Stress | σ<sub>c</sub>, Z<sub>N</sub> | σ<sub>ac</sub> | Step 36 |
| EQ38a | `S_{acP} = \sigma_{acP} \times K_R` | Design Bending Strength (Pinion) | σ<sub>acP</sub>, K<sub>R</sub> | S<sub>acP</sub> | Step 38 |
| EQ38b | `S_{acG} = \sigma_{acG} \times K_R` | Design Bending Strength (Gear) | σ<sub>acG</sub>, K<sub>R</sub> | S<sub>acG</sub> | Step 38 |
| EQ38c | `S_{ac} = \sigma_{ac} \times K_R` | Design Contact Strength | σ<sub>ac</sub>, K<sub>R</sub> | S<sub>ac</sub> | Step 38 |
| EQ39a | `HB_{P,bending} = \frac{S_{acP}/1000 - 12.8}{0.0773}` | Required Hardness (Pinion Bending) | S<sub>acP</sub> | HB<sub>P,b</sub> | Step 39 |
| EQ39b | `HB_{G,bending} = \frac{S_{acG}/1000 - 12.8}{0.0773}` | Required Hardness (Gear Bending) | S<sub>acG</sub> | HB<sub>G,b</sub> | Step 39 |
| EQ39c | `HB_{contact} = \frac{S_{ac}/1000 - 29.10}{0.322}` | Required Hardness (Contact) | S<sub>ac</sub> | HB<sub>c</sub> | Step 39 |
| **General Equations Tab** |
| EQG1 | `p_t = \frac{D_P}{N_P} = \frac{D_G}{N_G} = \frac{\pi}{P_d}` | Transverse Circular Pitch | D<sub>P</sub>, N<sub>P</sub>, D<sub>G</sub>, N<sub>G</sub>, P<sub>d</sub> | p<sub>t</sub> | Reference |
| EQG2 | `p_n = p_t \cos \psi` | Normal Circular Pitch | p<sub>t</sub>, ψ | p<sub>n</sub> | Reference |
| EQG3 | `p_x = \frac{p_t}{\tan \psi} = \frac{\pi}{P_d \tan \psi} = m\pi` | Axial Pitch | p<sub>t</sub>, ψ, P<sub>d</sub>, m | p<sub>x</sub> | Reference |
| EQG4 | `P_{nd} = P_d \cos \psi` | Normal Diametral Pitch Relation | P<sub>d</sub>, ψ | P<sub>nd</sub> | Reference |
| EQG5 | `m = \frac{D}{N}` | Metric Module | D, N | m | Reference |
| EQG6 | `m_n = \frac{\cos \psi}{P_d} = \frac{D \cos \psi}{N} = m \cos \psi` | Normal Metric Module | ψ, P<sub>d</sub>, D, N, m | m<sub>n</sub> | Reference |
| EQG7 | `T = \frac{63000P}{n}` | Torque | P, n | T | Reference |
| EQG8 | `v_t = \frac{\pi D n}{12}` | Pitch Line Speed | D, n | v<sub>t</sub> | Reference |
| EQG9 | `W_r = W_t \tan \phi_t` | Radial Force | W<sub>t</sub>, φ<sub>t</sub> | W<sub>r</sub> | Reference |
| EQG10 | `W_x = W_t \tan \psi` | Axial Force | W<sub>t</sub>, ψ | W<sub>x</sub> | Reference |
| EQG11 | `\text{FCR} = \frac{F}{p_x} > 2.0` | Face Contact Ratio | F, p<sub>x</sub> | FCR | Must be > 2.0 |
| EQG12 | `W_t = \frac{33000P}{v_t} = \frac{126000P}{nD}` | Transmitted Load (alt) | P, v<sub>t</sub>, n, D | W<sub>t</sub> | Reference |
| EQG13 | `\tan \phi_n = \tan \phi_t \cos \psi` | Angle Relationship | φ<sub>n</sub>, φ<sub>t</sub>, ψ | - | Reference |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Helical-Specific Steps (1-13)** |
| 1 | Input Basic Parameters | User Input | Enter P [hp], K<sub>O</sub> [-], n<sub>P</sub> [rpm], N<sub>P</sub> [teeth], N<sub>G</sub> [teeth], ψ [deg], P<sub>nd</sub> [teeth/in], F [in] | K<sub>O</sub> from Table 8-1 |
| 2 | Lookup Shock Factor | Table | Look up K<sub>O</sub> from Table 8-1 based on application | Yellow input |
| 3 | Calculate Transverse Diametral Pitch | Equation | P<sub>d</sub> = P<sub>nd</sub> cos(ψ) (EQ3) | Auto-calculated |
| 4 | Calculate Pinion Pitch Diameter | Equation | D<sub>P</sub> = N<sub>P</sub>/P<sub>d</sub> (EQ4) | Auto-calculated |
| 5 | Calculate Gear Pitch Diameter | Equation | D<sub>G</sub> = N<sub>G</sub>/P<sub>d</sub> (EQ5) | Auto-calculated |
| 6 | Calculate Center Distance | Equation | C = (D<sub>P</sub> + D<sub>G</sub>)/2 (EQ6) | Auto-calculated |
| 7 | Calculate Velocity | Equation | V<sub>t</sub> = πD<sub>P</sub>n<sub>P</sub>/12 (EQ7) | Auto-calculated |
| 8 | Select Quality Number | Table | Choose Q<sub>v</sub> from Table 8-3 based on V<sub>t</sub> | Yellow input |
| 9 | Determine Dynamic Factor | Table | Calculate K<sub>v</sub> using Table 8-4 formula | Yellow input |
| 10 | (K<sub>v</sub> Formula) | Note | K<sub>v</sub> = [(A + √V<sub>t</sub>)/A]<sup>B</sup> where A, B from Table 8-4 | Based on Q<sub>v</sub> |
| 11 | Input Normal Pressure Angle | User Input | Enter φ<sub>n</sub> [deg], typically 20° | Default value |
| 12 | Calculate Transverse Pressure Angle | Equation | φ<sub>t</sub> = arctan(tan(φ<sub>n</sub>)/cos(ψ)) (EQ12) | Auto-calculated |
| 13 | Lookup Geometry Factors | Figure | Look up J<sub>P</sub>, J<sub>G</sub>, I from Figures 8-16 to 8-18 | Based on N<sub>P</sub>, N<sub>G</sub>, φ<sub>t</sub> |
| **Transition** |
| - | Continue with Spur Guide | Section Divider | "⬇ Continue with Spur Guide Steps 32-40 ⬇" | Visual separator |
| **Spur Steps (32-40)** |
| 32 | Input Design Life Cycles | User Input | Enter N<sub>c</sub> [cycles] | Calculate or estimate |
| 33 | Lookup Life Factors (Bending) | Figure | Look up Y<sub>N</sub> from Figure 8-14 in spur PDF | Based on N<sub>c</sub> |
| 34a | Lookup Life Factors (Contact) | Figure | Look up Z<sub>N</sub> from Figure 8-15 in spur PDF | Based on N<sub>c</sub> |
| 34b | Input Other Factors | User Input | Enter K<sub>m</sub>, K<sub>s</sub>, K<sub>B</sub>, C<sub>p</sub>, K<sub>R</sub> | Typically K<sub>m</sub>=K<sub>s</sub>=K<sub>B</sub>=K<sub>R</sub>=1.0, C<sub>p</sub>=2300 |
| 35a | Calculate Transmitted Load | Equation | W<sub>t</sub> = 33000 × P<sub>des</sub>/V<sub>t</sub> (EQ35a) | P<sub>des</sub> = P × K<sub>O</sub> |
| 35b | Calculate Pinion Bending Stress | Equation | σ<sub>tP</sub> using EQ35b | Auto-calculated |
| 35c | Calculate Gear Bending Stress | Equation | σ<sub>tG</sub> using EQ35c | Auto-calculated |
| 35d | Calculate Allowable Bending Stress (Pinion) | Equation | σ<sub>acP</sub> = σ<sub>tP</sub>/Y<sub>N</sub> (EQ35d) | Auto-calculated |
| 35e | Calculate Allowable Bending Stress (Gear) | Equation | σ<sub>acG</sub> = σ<sub>tG</sub>/Y<sub>N</sub> (EQ35e) | Auto-calculated |
| 36a | Calculate Contact Stress | Equation | σ<sub>c</sub> using EQ36a | Auto-calculated |
| 36b | Calculate Allowable Contact Stress | Equation | σ<sub>ac</sub> = σ<sub>c</sub>/Z<sub>N</sub> (EQ36b) | Auto-calculated |
| 38a | Calculate Design Bending Strength (Pinion) | Equation | S<sub>acP</sub> = σ<sub>acP</sub> × K<sub>R</sub> (EQ38a) | Auto-calculated |
| 38b | Calculate Design Bending Strength (Gear) | Equation | S<sub>acG</sub> = σ<sub>acG</sub> × K<sub>R</sub> (EQ38b) | Auto-calculated |
| 38c | Calculate Design Contact Strength | Equation | S<sub>ac</sub> = σ<sub>ac</sub> × K<sub>R</sub> (EQ38c) | Auto-calculated |
| 39a | Calculate Required Hardness (Pinion Bending) | Equation | HB<sub>P,b</sub> using EQ39a | Auto-calculated |
| 39b | Calculate Required Hardness (Gear Bending) | Equation | HB<sub>G,b</sub> using EQ39b | Auto-calculated |
| 39c | Calculate Required Hardness (Contact) | Equation | HB<sub>c</sub> using EQ39c | Auto-calculated |
| 40 | Select Material | Table | Choose material from Table 8-6 that meets or exceeds HB values | Manual selection |

---

## 5. Additional Notes

### Validation Rules
- **Integer Teeth**: N<sub>P</sub>, N<sub>G</sub> must be positive integers
- **Minimum Teeth**: N<sub>P</sub> ≥ 17 recommended (can be lower for low-speed applications)
- **Helix Angle Range**: 0° < ψ < 45°, typically 15-30° for standard helical gears
- **Pressure Angle**: φ<sub>n</sub> typically 20° (can be 14.5°, 22.5°, or 25°)
- **Face Contact Ratio**: FCR = F/p<sub>x</sub> should be > 2.0 for smooth operation
- **Positive Values**: All dimensions, forces, and stresses must be positive
- **Dynamic Factor Range**: K<sub>v</sub> typically 0.5-1.5
- **Reliability Factor**: K<sub>R</sub> = 1.0 for 99% reliability, 1.25 for 99.9%, 1.5 for 99.99%

### Standard Values / Constants
- **Normal Pressure Angle**: φ<sub>n</sub> = 20° (default)
- **Elastic Coefficient**: C<sub>p</sub> = 2300 √psi for steel-on-steel
- **Default Factors**: K<sub>m</sub> = K<sub>s</sub> = K<sub>B</sub> = K<sub>R</sub> = 1.0
- **Typical Helix Angles**: 15°, 20°, 23°, 30° (most common: 20-23°)
- **π**: Use Math.PI (3.14159...)

### Dropdown Options

**None explicitly required** - All selections are manual lookups from tables/figures:
- K<sub>O</sub> from Table 8-1
- Q<sub>v</sub> from Table 8-3
- J<sub>P</sub>, J<sub>G</sub>, I from Figures 8-16 to 8-18
- Y<sub>N</sub> from Figure 8-14 (spur PDF)
- Z<sub>N</sub> from Figure 8-15 (spur PDF)
- Material selection from Table 8-6

### Future Enhancement Opportunities
- **Table 8-2 Dropdown**: Implement standard P<sub>nd</sub> values (4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 40, 48)
- **K<sub>v</sub> Calculator**: Auto-calculate from Q<sub>v</sub> and V<sub>t</sub> using Table 8-4 formulas
- **Geometry Factor Interpolation**: Digitize Figures 8-16 to 8-18 for automatic J<sub>P</sub>, J<sub>G</sub>, I lookup
- **Life Factor Curves**: Digitize Figures 8-14 and 8-15 for automatic Y<sub>N</sub> and Z<sub>N</sub> lookup
- **Material Database**: Implement Table 8-6 with searchable material properties
- **Face Contact Ratio Warning**: Auto-calculate and warn if FCR < 2.0
- **Axial Force Display**: Show W<sub>x</sub> = W<sub>t</sub> tan(ψ) to highlight thrust bearing requirement

### Special Considerations
- **Hybrid Procedure**: This calculator uniquely combines helical-specific steps (1-13) with spur stress analysis steps (32-40). The PDF warps from helical to spur methodology.
- **Angle Transformations**: Critical to distinguish between normal (φ<sub>n</sub>) and transverse (φ<sub>t</sub>) pressure angles. The transverse angle is what's used in geometry factor lookups.
- **Pitch Relationships**: P<sub>d</sub> = P<sub>nd</sub> cos(ψ) is fundamental - the transverse pitch is larger than the normal pitch.
- **Axial Thrust**: Helical gears produce axial thrust W<sub>x</sub> = W<sub>t</sub> tan(ψ), requiring thrust bearings.
- **Geometry Factor Lookup**: J and I factors are harder to obtain for helical gears (Figures 8-16 to 8-18) compared to spur gears.
- **Face Contact Ratio**: Unlike spur gears, helical gears have face contact ratio FCR = F/p<sub>x</sub> which should exceed 2.0 for smooth, quiet operation.
- **Design Power**: P<sub>des</sub> = P × K<sub>O</sub> is used in stress calculations (not explicitly shown as separate variable in HTML).
- **Hardness Formula**: Linear formulas for HB assume through-hardened steel. Different formulas apply for case-hardened gears.

### Calculation Order / Dependencies

```
User Inputs: P, KO, nP, NP, NG, ψ, Pnd, F, φn →
EQ3: Pd = Pnd × cos(ψ) →
EQ4: DP = NP / Pd →
EQ5: DG = NG / Pd →
EQ6: C = (DP + DG) / 2 →
EQ7: Vt = π × DP × nP / 12 →
User: Qv (Table 8-3 based on Vt) →
User: Kv (Table 8-4 formula based on Qv and Vt) →
EQ12: φt = arctan(tan(φn) / cos(ψ)) →
User: JP, JG, I (Figures 8-16 to 8-18 based on NP, NG, φt) →

--- Spur Steps Begin ---
User: Nc, YN (Figure 8-14), ZN (Figure 8-15), Km, Ks, KB, Cp, KR →
EQ35a: Wt = 33000 × P × KO / Vt →
EQ35b: σtP = (Wt × Pd) / (F × JP) × KO × Ks × Km × KB × Kv →
EQ35c: σtG = (Wt × Pd) / (F × JG) × KO × Ks × Km × KB × Kv →
EQ35d: σacP = σtP / YN →
EQ35e: σacG = σtG / YN →
EQ36a: σc = Cp × √[(Wt × KO × Ks × Km × Kv) / (F × DP × I)] →
EQ36b: σac = σc / ZN →
EQ38a: SacP = σacP × KR →
EQ38b: SacG = σacG × KR →
EQ38c: Sac = σac × KR →
EQ39a: HB_P,b = (SacP/1000 - 12.8) / 0.0773 →
EQ39b: HB_G,b = (SacG/1000 - 12.8) / 0.0773 →
EQ39c: HB_c = (Sac/1000 - 29.10) / 0.322 →
User: Select material from Table 8-6 that meets HB requirements
```

### General Equations Tab Notes
The General Equations tab contains 13 reference equations covering:
- **Pitch relationships**: Transverse, normal, and axial pitch conversions
- **Module relationships**: Metric module and normal module
- **Force analysis**: Tangential, radial, and axial forces
- **Kinematics**: Torque and velocity relationships
- **Face contact ratio**: Critical design check for helical gears

These equations are for reference and educational purposes. Some may overlap with procedure calculations but provide complete formulas for engineering understanding.

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized (40+ variables)
- [x] Source references noted for all table/figure variables
- [x] All equations extracted and converted to LaTeX (18 procedure + 13 reference)
- [x] Procedure steps documented (13 helical + 9 spur = 22 steps)
- [x] Piecewise equations explained (N/A)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout
- [x] Hybrid procedure (helical → spur) documented

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has high complexity due to:
1. **Hybrid methodology**: Helical steps 1-13 followed by spur steps 32-40
2. **Angle transformations**: Normal vs. transverse pressure angle relationships
3. **Manual geometry factor lookups**: J<sub>P</sub>, J<sub>G</sub>, I from complex figures
4. **Life factor lookups**: Y<sub>N</sub> and Z<sub>N</sub> from logarithmic graphs
5. **Cross-PDF references**: References both helical and spur PDFs

The existing HTML implementation uses:
- Three-tab structure (Procedure, General Equations, Variables)
- Input syncing across tabs via `varInstanceMap`
- Computed field styling (green background)
- Session persistence via LocalStorage
- MathJax for equation rendering

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Based on: fizz_chains_spec.md
