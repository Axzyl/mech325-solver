# Calculator Specification: Worm Gear Design Calculator

---

## 1. Metadata

- **Calculator Name:** Worm Gear Design Calculator
- **Source PDF:** worm.pdf
- **PDF Location:** `documents/gears/worm.pdf`
- **Description:** Calculates worm gear parameters including geometry, forces, efficiency, stress, and pitting resistance. Performs a comprehensive 21-step analysis from lead angle calculation through pitting resistance verification.
- **Complexity:** High (extensive piecewise equations, multiple force calculations, material-specific factors)
- **Tabs to Include:**
  - [x] Procedure (21-step process)
  - [x] General Equations (13 reference equations)
  - [x] Variables (40+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 21-step sequential procedure for worm gear design
- **General Equations Tab**: Contains 13 reference equations for pitch, geometry, and speed calculations
- **Variables Tab**: 40+ variables with various source types require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs (Given)** |
| N<sub>G</sub> | Number of Teeth on Gear | Gear tooth count | teeth | User Input | - | No | Positive integer |
| N<sub>W</sub> | Number of Worm Threads | Worm thread count (1-4 typical) | threads | User Input | - | No | Positive integer |
| D<sub>G</sub> | Pitch Diameter of Gear | Gear pitch diameter | in | User Input | - | No | Must be > 0 |
| D<sub>W</sub> | Pitch Diameter of Worm | Worm pitch diameter | in | User Input | - | No | Must be > 0 |
| P<sub>d</sub> | Diametral Pitch | Number of teeth per inch | teeth/in | User Input | - | No | Must be > 0 |
| φ<sub>n</sub> | Normal Pressure Angle | Normal pressure angle | deg | User Input | - | No | Typically 14.5°, 20°, 25°, or 30° |
| n<sub>W</sub> | Speed of Worm | Worm rotational speed | rpm | User Input | - | No | Must be ≥ 0 |
| n<sub>G</sub> | Speed of Gear | Gear rotational speed | rpm | User Input | - | No | Must be ≥ 0 |
| P<sub>o</sub> | Output Power | Power transmitted | hp | User Input | - | No | Must be ≥ 0 |
| F | Face Width of Wormgear | Wormgear face width | in | User Input | - | No | Must be > 0 |
| **Manual Lookups** |
| Bronze Type | Bronze Material Type | Gear material classification | - | Dropdown | Table 10-5 | No | Sand/Static/Centrifugal |
| y | Lewis Form Factor | Tooth form factor | - | Table | Table 10-5 | Yes | Function of φ<sub>n</sub> |
| **Auto-Calculated (Step 1: Lead and Lead Angle)** |
| p | Circular Pitch | Circumferential pitch | in | Equation | EQ1 | No | p = π / P<sub>d</sub> |
| P<sub>x</sub> | Axial Pitch | Axial pitch (equals p) | in | Equation | EQ2 | No | P<sub>x</sub> = p |
| L | Lead | Lead of worm | in | Equation | EQ3 | No | L = N<sub>W</sub> × P<sub>x</sub> |
| λ | Lead Angle | Lead angle of worm | deg | Equation | EQ4 | No | λ = arctan(L / (π D<sub>W</sub>)) |
| **Auto-Calculated (Step 2: Center Distance)** |
| CD | Center Distance | Distance between centers | in | Equation | EQ5 | No | CD = (D<sub>G</sub> + D<sub>W</sub>) / 2 |
| **Auto-Calculated (Step 3: Pitch Line Speed)** |
| ν<sub>tG</sub> | Pitch Line Speed of Gear | Tangential velocity | ft/min | Equation | EQ6 | No | ν<sub>tG</sub> = π D<sub>G</sub> n<sub>G</sub> / 12 |
| **Auto-Calculated (Step 4: Sliding Speed)** |
| ν<sub>s</sub> | Sliding Speed | Relative sliding velocity | ft/min | Equation | EQ7 | No | ν<sub>s</sub> = ν<sub>tG</sub> / sin(λ) |
| **Auto-Calculated (Step 5: Coefficient of Friction)** |
| μ | Coefficient of Friction | Dynamic friction coefficient | - | Piecewise | EQ8 (3 cases) | No | Depends on ν<sub>s</sub> |
| **Auto-Calculated (Step 6: Forces on Gear)** |
| T<sub>o</sub> | Output Torque | Torque on gear | lb-in | Equation | EQ9 | No | T<sub>o</sub> = 63000 P<sub>o</sub> / n<sub>G</sub> |
| W<sub>tG</sub> | Transmitted Force on Gear | Tangential force | lb | Equation | EQ10 | No | W<sub>tG</sub> = 2 T<sub>o</sub> / D<sub>G</sub> |
| W<sub>xG</sub> | Axial Force on Gear | Axial thrust force | lb | Equation | EQ11 | No | Complex formula (Step 6) |
| W<sub>rG</sub> | Radial Force on Gear | Radial separating force | lb | Equation | EQ12 | No | Complex formula (Step 6) |
| **Auto-Calculated (Step 7: Friction Force)** |
| W<sub>f</sub> | Friction Force | Friction force | lb | Equation | EQ13 | No | Complex formula (Step 7) |
| **Auto-Calculated (Step 8: Power Loss)** |
| P<sub>L</sub> | Power Loss | Power lost to friction | hp | Equation | EQ14 | No | P<sub>L</sub> = ν<sub>s</sub> W<sub>f</sub> / 33000 |
| **Auto-Calculated (Step 9: Input Power)** |
| P<sub>i</sub> | Input Power | Required input power | hp | Equation | EQ15 | No | P<sub>i</sub> = P<sub>o</sub> + P<sub>L</sub> |
| **Auto-Calculated (Step 10: Efficiency)** |
| η | Efficiency | Transmission efficiency | % | Equation | EQ16 | No | η = (P<sub>o</sub> / P<sub>i</sub>) × 100 |
| **Auto-Calculated (Step 12: Normal Circular Pitch)** |
| p<sub>n</sub> | Normal Circular Pitch | Normal pitch | in | Equation | EQ17 | No | p<sub>n</sub> = p cos(λ) |
| **Auto-Calculated (Step 13: Dynamic Factor)** |
| K<sub>v</sub> | Dynamic Factor | Velocity factor | - | Equation | EQ18 | No | K<sub>v</sub> = 1200 / (1200 + ν<sub>tG</sub>) |
| **Auto-Calculated (Step 14: Dynamic Load)** |
| W<sub>d</sub> | Dynamic Load | Dynamic tangential load | lb | Equation | EQ19 | No | W<sub>d</sub> = W<sub>tG</sub> / K<sub>v</sub> |
| **Auto-Calculated (Step 15: Stress)** |
| σ | Stress in Gear Teeth | Bending stress | psi | Equation | EQ20 | No | σ = W<sub>d</sub> / (y F p<sub>n</sub>) |
| **Auto-Calculated (Step 16: Materials Factor)** |
| C<sub>s</sub> | Materials Factor | Material strength coefficient | psi | Piecewise | EQ21 (3 materials × 2 cases) | No | Depends on bronze type and D<sub>G</sub> |
| **Auto-Calculated (Step 17: Ratio Correction Factor)** |
| m<sub>G</sub> | Gear Ratio | Velocity ratio | - | Equation | EQ22 | No | m<sub>G</sub> = N<sub>G</sub> / N<sub>W</sub> |
| C<sub>m</sub> | Ratio Correction Factor | Ratio correction coefficient | - | Piecewise | EQ23 (3 cases) | No | Depends on m<sub>G</sub> |
| **Auto-Calculated (Step 18: Velocity Factor)** |
| C<sub>v</sub> | Velocity Factor | Sliding velocity factor | - | Piecewise | EQ24 (3 cases) | No | Depends on ν<sub>s</sub> |
| **Auto-Calculated (Step 19: Effective Face Width)** |
| F<sub>e</sub> | Effective Face Width | Effective contact width | in | Piecewise | EQ25 (2 cases) | No | min(F, D<sub>W</sub>/3) |
| **Auto-Calculated (Step 20: Rated Tangential Load)** |
| W<sub>tR</sub> | Rated Tangential Load | Allowable pitting load | lb | Equation | EQ26 | No | W<sub>tR</sub> = C<sub>s</sub> D<sub>G</sub><sup>0.8</sup> F<sub>e</sub> C<sub>m</sub> C<sub>v</sub> |
| **General Equations (Reference Variables)** |
| a | Addendum | Tooth addendum | in | Equation | GEN1 | No | a = 1 / P<sub>d</sub> |
| h<sub>t</sub> | Whole Depth | Total tooth depth | in | Equation | GEN2 | No | h<sub>t</sub> = 2.157 / P<sub>d</sub> |
| b | Dedendum | Tooth dedendum | in | Equation | GEN3 | No | b = h<sub>t</sub> - a |
| D<sub>rW</sub> | Worm Root Diameter | Worm root diameter | in | Equation | GEN4 | No | D<sub>rW</sub> = D<sub>W</sub> - 2b |
| D<sub>oW</sub> | Worm Outside Diameter | Worm outside diameter | in | Equation | GEN5 | No | D<sub>oW</sub> = D<sub>W</sub> + 2a |
| D<sub>rG</sub> | Gear Root Diameter | Gear root diameter | in | Equation | GEN6 | No | D<sub>rG</sub> = D<sub>G</sub> - 2b |
| D<sub>t</sub> | Gear Throat Diameter | Gear throat diameter | in | Equation | GEN7 | No | D<sub>t</sub> = D<sub>G</sub> + 2a |
| v<sub>W</sub> | Worm Pitch Line Speed | Worm pitch line velocity | ft/min | Equation | GEN8 | No | v<sub>W</sub> = π D<sub>W</sub> n<sub>W</sub> / 12 |
| v<sub>G</sub> | Gear Pitch Line Speed | Gear pitch line velocity | ft/min | Equation | GEN9 | No | v<sub>G</sub> = π D<sub>G</sub> n<sub>G</sub> / 12 |
| VR | Velocity Ratio | Speed ratio | - | Equation | GEN10 | No | VR = n<sub>W</sub> / n<sub>G</sub> |
| C | Center Distance (alt) | Center distance (alt symbol) | in | Equation | GEN11 | No | C = (D<sub>W</sub> + D<sub>G</sub>) / 2 |

---

## 3. Equations

### Main Procedure Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `p = \frac{\pi}{P_d}` | Circular Pitch | P<sub>d</sub> | p | Step 1 |
| EQ2 | `P_x = p` | Axial Pitch | p | P<sub>x</sub> | Step 1 (equality) |
| EQ3 | `L = N_W \cdot P_x` | Lead | N<sub>W</sub>, P<sub>x</sub> | L | Step 1 |
| EQ4 | `\lambda = \arctan\left(\frac{L}{\pi D_W}\right)` | Lead Angle | L, D<sub>W</sub> | λ | Step 1, result in degrees |
| EQ5 | `CD = \frac{D_G + D_W}{2}` | Center Distance | D<sub>G</sub>, D<sub>W</sub> | CD | Step 2 |
| EQ6 | `\nu_{tG} = \frac{\pi D_G n_G}{12}` | Pitch Line Speed of Gear | D<sub>G</sub>, n<sub>G</sub> | ν<sub>tG</sub> | Step 3 |
| EQ7 | `\nu_s = \frac{\nu_{tG}}{\sin(\lambda)}` | Sliding Speed | ν<sub>tG</sub>, λ | ν<sub>s</sub> | Step 4 |
| EQ8 | `\mu = \begin{cases} 0.15 & \nu_s = 0 \\ 0.124 e^{-0.074 \nu_s^{0.645}} & 0 < \nu_s < 10 \\ 0.103 e^{-0.11 \nu_s^{0.45}} + 0.012 & \nu_s \geq 10 \end{cases}` | Coefficient of Friction | ν<sub>s</sub> | μ | Step 5, piecewise |
| EQ9 | `T_o = \frac{63000 P_o}{n_G}` | Output Torque | P<sub>o</sub>, n<sub>G</sub> | T<sub>o</sub> | Step 6 |
| EQ10 | `W_{tG} = \frac{2 T_o}{D_G}` | Transmitted Force on Gear | T<sub>o</sub>, D<sub>G</sub> | W<sub>tG</sub> | Step 6 |
| EQ11 | `W_{xG} = W_{tG} \frac{\cos(\phi_n)\sin(\lambda) + \mu\cos(\lambda)}{\cos(\phi_n)\cos(\lambda) - \mu\sin(\lambda)}` | Axial Force on Gear | W<sub>tG</sub>, φ<sub>n</sub>, λ, μ | W<sub>xG</sub> | Step 6 |
| EQ12 | `W_{rG} = \frac{W_{tG} \sin(\phi_n)}{\cos(\phi_n)\cos(\lambda) - \mu\sin(\lambda)}` | Radial Force on Gear | W<sub>tG</sub>, φ<sub>n</sub>, λ, μ | W<sub>rG</sub> | Step 6 |
| EQ13 | `W_f = \frac{\mu W_{tG}}{\cos(\lambda)\cos(\phi_n) - \mu\sin(\lambda)}` | Friction Force | μ, W<sub>tG</sub>, λ, φ<sub>n</sub> | W<sub>f</sub> | Step 7 |
| EQ14 | `P_L = \frac{\nu_s W_f}{33000}` | Power Loss | ν<sub>s</sub>, W<sub>f</sub> | P<sub>L</sub> | Step 8 |
| EQ15 | `P_i = P_o + P_L` | Input Power | P<sub>o</sub>, P<sub>L</sub> | P<sub>i</sub> | Step 9 |
| EQ16 | `\eta = \frac{P_o}{P_i} \times 100` | Efficiency | P<sub>o</sub>, P<sub>i</sub> | η | Step 10 |
| EQ17 | `p_n = p \cos(\lambda)` | Normal Circular Pitch | p, λ | p<sub>n</sub> | Step 12 |
| EQ18 | `K_v = \frac{1200}{1200 + \nu_{tG}}` | Dynamic Factor | ν<sub>tG</sub> | K<sub>v</sub> | Step 13 |
| EQ19 | `W_d = \frac{W_{tG}}{K_v}` | Dynamic Load | W<sub>tG</sub>, K<sub>v</sub> | W<sub>d</sub> | Step 14 |
| EQ20 | `\sigma = \frac{W_d}{y F p_n}` | Stress in Gear Teeth | W<sub>d</sub>, y, F, p<sub>n</sub> | σ | Step 15 |
| EQ21 | `C_s = \begin{cases} 1189.636 - 476.545 \log_{10}(D_G) & \text{Sand, } D_G > 2.5 \\ 1000 & \text{Sand, } D_G \leq 2.5 \\ 1411.651 - 455.825 \log_{10}(D_G) & \text{Static, } D_G < 8 \\ 1000 & \text{Static, } D_G \geq 8 \\ 1251.291 - 179.75 \log_{10}(D_G) & \text{Centrifugal, } D_G < 25 \\ 1000 & \text{Centrifugal, } D_G \geq 25 \end{cases}` | Materials Factor | D<sub>G</sub>, Bronze Type | C<sub>s</sub> | Step 16, 6 cases |
| EQ22 | `m_G = \frac{N_G}{N_W}` | Gear Ratio | N<sub>G</sub>, N<sub>W</sub> | m<sub>G</sub> | Step 17 |
| EQ23 | `C_m = \begin{cases} 0.02\sqrt{-m_G^2 + 40m_G - 76} + 0.46 & 6 < m_G < 20 \\ 0.0107\sqrt{m_G^2 + 56m_G + 5146} & 20 \leq m_G < 76 \\ 1.1483 - 0.00658 m_G & m_G \geq 76 \end{cases}` | Ratio Correction Factor | m<sub>G</sub> | C<sub>m</sub> | Step 17, piecewise |
| EQ24 | `C_v = \begin{cases} 0.659 e^{-0.0011 \nu_s} & 0 < \nu_s < 700 \\ 13.31 \nu_s^{-0.571} & 700 \leq \nu_s < 3000 \\ 65.52 \nu_s^{-0.774} & \nu_s \geq 3000 \end{cases}` | Velocity Factor | ν<sub>s</sub> | C<sub>v</sub> | Step 18, piecewise |
| EQ25 | `F_e = \begin{cases} F & F < \frac{D_W}{3} \\ \frac{D_W}{3} & F \geq \frac{D_W}{3} \end{cases}` | Effective Face Width | F, D<sub>W</sub> | F<sub>e</sub> | Step 19, piecewise |
| EQ26 | `W_{tR} = C_s D_G^{0.8} F_e C_m C_v` | Rated Tangential Load | C<sub>s</sub>, D<sub>G</sub>, F<sub>e</sub>, C<sub>m</sub>, C<sub>v</sub> | W<sub>tR</sub> | Step 20 |

### General Reference Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| GEN1 | `a = \frac{1}{P_d}` | Addendum | P<sub>d</sub> | a | General geometry |
| GEN2 | `h_t = \frac{2.157}{P_d}` | Whole Depth | P<sub>d</sub> | h<sub>t</sub> | General geometry |
| GEN3 | `b = h_t - a` | Dedendum | h<sub>t</sub>, a | b | General geometry |
| GEN4 | `D_{rW} = D_W - 2b` | Worm Root Diameter | D<sub>W</sub>, b | D<sub>rW</sub> | General geometry |
| GEN5 | `D_{oW} = D_W + 2a` | Worm Outside Diameter | D<sub>W</sub>, a | D<sub>oW</sub> | General geometry |
| GEN6 | `D_{rG} = D_G - 2b` | Gear Root Diameter | D<sub>G</sub>, b | D<sub>rG</sub> | General geometry |
| GEN7 | `D_t = D_G + 2a` | Gear Throat Diameter | D<sub>G</sub>, a | D<sub>t</sub> | General geometry |
| GEN8 | `v_W = \frac{\pi D_W n_W}{12}` | Worm Pitch Line Speed | D<sub>W</sub>, n<sub>W</sub> | v<sub>W</sub> | General kinematics |
| GEN9 | `v_G = \frac{\pi D_G n_G}{12}` | Gear Pitch Line Speed | D<sub>G</sub>, n<sub>G</sub> | v<sub>G</sub> | General kinematics |
| GEN10 | `VR = \frac{n_W}{n_G}` | Velocity Ratio | n<sub>W</sub>, n<sub>G</sub> | VR | General kinematics |
| GEN11 | `p = \frac{\pi D_G}{N_G}` | Circular Pitch (alt) | D<sub>G</sub>, N<sub>G</sub> | p | Alternative calculation |
| GEN12 | `P_d = \frac{N_G}{D_G} = \frac{\pi}{p}` | Diametral Pitch (alt) | N<sub>G</sub>, D<sub>G</sub>, p | P<sub>d</sub> | Alternative calculation |
| GEN13 | `C = \frac{D_W + D_G}{2}` | Center Distance (alt) | D<sub>W</sub>, D<sub>G</sub> | C | Same as EQ5 |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Given Information** |
| 0a | Input Gear Teeth | User Input | Enter N<sub>G</sub> (teeth) | Positive integer |
| 0b | Input Worm Threads | User Input | Enter N<sub>W</sub> (threads) | Typically 1-4 |
| 0c | Input Gear Pitch Diameter | User Input | Enter D<sub>G</sub> (in) | Must be > 0 |
| 0d | Input Worm Pitch Diameter | User Input | Enter D<sub>W</sub> (in) | Must be > 0 |
| 0e | Input Diametral Pitch | User Input | Enter P<sub>d</sub> (teeth/in) | Must be > 0 |
| 0f | Input Normal Pressure Angle | User Input | Enter φ<sub>n</sub> (deg) | Typically 14.5°, 20°, 25°, 30° |
| 0g | Input Worm Speed | User Input | Enter n<sub>W</sub> (rpm) | Must be ≥ 0 |
| 0h | Input Gear Speed | User Input | Enter n<sub>G</sub> (rpm) | Must be ≥ 0 |
| 0i | Input Output Power | User Input | Enter P<sub>o</sub> (hp) | Must be ≥ 0 |
| 0j | Input Face Width | User Input | Enter F (in) | Must be > 0 |
| 0k | Select Bronze Type | Dropdown | Select material: Sand-cast / Static-chill-cast or Forged / Centrifugally cast | Affects C<sub>s</sub> calculation |
| **Step 1: Calculate Lead and Lead Angle** |
| 1a | Calculate Circular Pitch | Equation | p = π / P<sub>d</sub> (EQ1) | Auto-calculated |
| 1b | Set Axial Pitch | Equation | P<sub>x</sub> = p (EQ2) | Auto-calculated |
| 1c | Calculate Lead | Equation | L = N<sub>W</sub> × P<sub>x</sub> (EQ3) | Auto-calculated |
| 1d | Calculate Lead Angle | Equation | λ = arctan(L / (π D<sub>W</sub>)) (EQ4) | Auto-calculated, result in degrees |
| **Step 2: Calculate Center Distance** |
| 2 | Calculate Center Distance | Equation | CD = (D<sub>G</sub> + D<sub>W</sub>) / 2 (EQ5) | Auto-calculated |
| **Step 3: Calculate Pitch Line Speed of Gear** |
| 3 | Calculate Pitch Line Speed | Equation | ν<sub>tG</sub> = π D<sub>G</sub> n<sub>G</sub> / 12 (EQ6) | Auto-calculated, result in ft/min |
| **Step 4: Calculate Sliding Speed** |
| 4 | Calculate Sliding Speed | Equation | ν<sub>s</sub> = ν<sub>tG</sub> / sin(λ) (EQ7) | Auto-calculated |
| **Step 5: Calculate Coefficient of Friction** |
| 5 | Calculate Coefficient of Friction | Piecewise Equation | μ based on ν<sub>s</sub> (EQ8) | 3 cases: ν<sub>s</sub>=0, 0<ν<sub>s</sub><10, ν<sub>s</sub>≥10 |
| **Step 6: Calculate Forces on Gear** |
| 6a | Calculate Output Torque | Equation | T<sub>o</sub> = 63000 P<sub>o</sub> / n<sub>G</sub> (EQ9) | Auto-calculated |
| 6b | Calculate Transmitted Force | Equation | W<sub>tG</sub> = 2 T<sub>o</sub> / D<sub>G</sub> (EQ10) | Auto-calculated |
| 6c | Calculate Axial Force | Equation | W<sub>xG</sub> using EQ11 | Complex formula with φ<sub>n</sub>, λ, μ |
| 6d | Calculate Radial Force | Equation | W<sub>rG</sub> using EQ12 | Complex formula with φ<sub>n</sub>, λ, μ |
| **Step 7: Calculate Friction Force** |
| 7 | Calculate Friction Force | Equation | W<sub>f</sub> using EQ13 | Complex formula with μ, W<sub>tG</sub>, λ, φ<sub>n</sub> |
| **Step 8: Calculate Power Loss** |
| 8 | Calculate Power Loss | Equation | P<sub>L</sub> = ν<sub>s</sub> W<sub>f</sub> / 33000 (EQ14) | Auto-calculated |
| **Step 9: Calculate Input Power** |
| 9 | Calculate Input Power | Equation | P<sub>i</sub> = P<sub>o</sub> + P<sub>L</sub> (EQ15) | Auto-calculated |
| **Step 10: Calculate Efficiency** |
| 10 | Calculate Efficiency | Equation | η = (P<sub>o</sub> / P<sub>i</sub>) × 100 (EQ16) | Auto-calculated, result in % |
| **Step 11: Find Lewis Form Factor** |
| 11 | Look Up Lewis Form Factor | Table | Find y from Table 10-5 based on φ<sub>n</sub> | Manual lookup or user input |
| **Step 12: Calculate Normal Circular Pitch** |
| 12 | Calculate Normal Pitch | Equation | p<sub>n</sub> = p cos(λ) (EQ17) | Auto-calculated |
| **Step 13: Calculate Dynamic Factor** |
| 13 | Calculate Dynamic Factor | Equation | K<sub>v</sub> = 1200 / (1200 + ν<sub>tG</sub>) (EQ18) | Auto-calculated |
| **Step 14: Calculate Dynamic Load** |
| 14 | Calculate Dynamic Load | Equation | W<sub>d</sub> = W<sub>tG</sub> / K<sub>v</sub> (EQ19) | Auto-calculated |
| **Step 15: Calculate Stress** |
| 15 | Calculate Gear Stress | Equation | σ = W<sub>d</sub> / (y F p<sub>n</sub>) (EQ20) | Auto-calculated |
| **Step 16: Calculate Materials Factor** |
| 16 | Calculate Materials Factor | Piecewise Equation | C<sub>s</sub> based on bronze type and D<sub>G</sub> (EQ21) | 6 cases (3 materials × 2 ranges each) |
| **Step 17: Calculate Ratio Correction Factor** |
| 17a | Calculate Gear Ratio | Equation | m<sub>G</sub> = N<sub>G</sub> / N<sub>W</sub> (EQ22) | Auto-calculated |
| 17b | Calculate Ratio Correction | Piecewise Equation | C<sub>m</sub> based on m<sub>G</sub> (EQ23) | 3 cases based on m<sub>G</sub> range |
| **Step 18: Calculate Velocity Factor** |
| 18 | Calculate Velocity Factor | Piecewise Equation | C<sub>v</sub> based on ν<sub>s</sub> (EQ24) | 3 cases based on ν<sub>s</sub> range |
| **Step 19: Calculate Effective Face Width** |
| 19 | Calculate Effective Face Width | Piecewise Equation | F<sub>e</sub> = min(F, D<sub>W</sub>/3) (EQ25) | 2 cases |
| **Step 20: Calculate Rated Tangential Load** |
| 20 | Calculate Rated Load | Equation | W<sub>tR</sub> = C<sub>s</sub> D<sub>G</sub><sup>0.8</sup> F<sub>e</sub> C<sub>m</sub> C<sub>v</sub> (EQ26) | Auto-calculated |
| **Step 21: Check Pitting Resistance** |
| 21 | Validate Pitting Resistance | Validation | Check: W<sub>tR</sub> > W<sub>tG</sub> | Pass if true, fail if false |

---

## 5. Additional Notes

### Validation Rules
- **Positive Integers**: N<sub>G</sub>, N<sub>W</sub> must be positive integers
- **Positive Values**: D<sub>G</sub>, D<sub>W</sub>, P<sub>d</sub>, n<sub>W</sub>, n<sub>G</sub>, P<sub>o</sub>, F must be > 0
- **Angle Range**: φ<sub>n</sub> typically 14.5°, 20°, 25°, or 30°
- **Thread Count**: N<sub>W</sub> typically 1-4 (higher = lower efficiency but smoother operation)
- **Pitting Check**: Design is satisfactory if W<sub>tR</sub> > W<sub>tG</sub>
- **Efficiency**: η typically ranges from 50-98% depending on lead angle and sliding speed
- **Division by Zero**: Ensure λ ≠ 0° for sliding speed calculation

### Standard Values / Constants
- **π**: Use Math.PI (3.14159265...)
- **Torque Conversion**: 63000 = conversion factor for hp and rpm to lb-in
- **Power Conversion**: 33000 = ft-lb/min per hp
- **Common Pressure Angles**: 14.5°, 20°, 25°, 30°
- **Typical Worm Threads**: 1, 2, 3, or 4

### Dropdown Options

**Bronze Type:**
- Sand-cast
- Static-chill-cast or Forged
- Centrifugally cast

**Lewis Form Factor (Table 10-5):**

| φ<sub>n</sub> | y |
|---------------|---|
| 14.5° | 0.100 |
| 20° | 0.125 |
| 25° | 0.150 |
| 30° | 0.175 |

### Piecewise Equations Summary

**EQ8: Coefficient of Friction (μ)**
- ν<sub>s</sub> = 0: μ = 0.15
- 0 < ν<sub>s</sub> < 10: μ = 0.124 e<sup>-0.074 ν<sub>s</sub><sup>0.645</sup></sup>
- ν<sub>s</sub> ≥ 10: μ = 0.103 e<sup>-0.11 ν<sub>s</sub><sup>0.45</sup></sup> + 0.012

**EQ21: Materials Factor (C<sub>s</sub>)**

Sand-cast bronze:
- D<sub>G</sub> > 2.5: C<sub>s</sub> = 1189.636 - 476.545 log₁₀(D<sub>G</sub>)
- D<sub>G</sub> ≤ 2.5: C<sub>s</sub> = 1000

Static-chill-cast or Forged bronze:
- D<sub>G</sub> < 8: C<sub>s</sub> = 1411.651 - 455.825 log₁₀(D<sub>G</sub>)
- D<sub>G</sub> ≥ 8: C<sub>s</sub> = 1000

Centrifugally cast bronze:
- D<sub>G</sub> < 25: C<sub>s</sub> = 1251.291 - 179.75 log₁₀(D<sub>G</sub>)
- D<sub>G</sub> ≥ 25: C<sub>s</sub> = 1000

**EQ23: Ratio Correction Factor (C<sub>m</sub>)**
- 6 < m<sub>G</sub> < 20: C<sub>m</sub> = 0.02√(-m<sub>G</sub>² + 40m<sub>G</sub> - 76) + 0.46
- 20 ≤ m<sub>G</sub> < 76: C<sub>m</sub> = 0.0107√(m<sub>G</sub>² + 56m<sub>G</sub> + 5146)
- m<sub>G</sub> ≥ 76: C<sub>m</sub> = 1.1483 - 0.00658 m<sub>G</sub>

**EQ24: Velocity Factor (C<sub>v</sub>)**
- 0 < ν<sub>s</sub> < 700: C<sub>v</sub> = 0.659 e<sup>-0.0011 ν<sub>s</sub></sup>
- 700 ≤ ν<sub>s</sub> < 3000: C<sub>v</sub> = 13.31 ν<sub>s</sub><sup>-0.571</sup>
- ν<sub>s</sub> ≥ 3000: C<sub>v</sub> = 65.52 ν<sub>s</sub><sup>-0.774</sup>

**EQ25: Effective Face Width (F<sub>e</sub>)**
- F < D<sub>W</sub>/3: F<sub>e</sub> = F
- F ≥ D<sub>W</sub>/3: F<sub>e</sub> = D<sub>W</sub>/3

### Future Enhancement Opportunities
- **Table 10-5**: Implement automatic lookup or interpolation for Lewis form factor y based on φ<sub>n</sub>
- **Material Database**: Add more bronze types and their properties
- **Efficiency Curves**: Plot efficiency vs. lead angle or gear ratio
- **Force Diagrams**: Visual representation of W<sub>tG</sub>, W<sub>xG</sub>, W<sub>rG</sub>, W<sub>f</sub>
- **Geometry Visualization**: 3D rendering of worm and gear mesh
- **Design Optimization**: Suggest optimal N<sub>W</sub>, D<sub>W</sub>, D<sub>G</sub> for target efficiency or size
- **Heat Dissipation**: Calculate thermal effects from power loss P<sub>L</sub>

### Special Considerations
- **Self-Locking**: Worm gears with low lead angles (λ < ~5-7°) may be self-locking (irreversible)
- **Efficiency**: Single-thread worms (N<sub>W</sub>=1) have lower efficiency but higher ratios
- **Material Pairing**: Worm typically hardened steel, gear typically bronze for wear resistance
- **Lubrication**: Critical for worm gears due to high sliding action
- **Heat Generation**: P<sub>L</sub> is dissipated as heat; adequate cooling required
- **Direction of Forces**: Forces on worm are opposite to forces on gear
- **Piecewise Indicators**: Calculator shows which formula branch is active with indicator boxes
- **Complex Denominators**: Several equations share denominator: cos(φ<sub>n</sub>)cos(λ) - μsin(λ)
  - Ensure this denominator ≠ 0 to avoid division errors

### Calculation Order / Dependencies

```
User Inputs: NG, NW, DG, DW, Pd, φn, nW, nG, Po, F, Bronze Type, y →

Step 1:
  p = π / Pd (EQ1) →
  Px = p (EQ2) →
  L = NW × Px (EQ3) →
  λ = arctan(L / (π DW)) (EQ4) →

Step 2:
  CD = (DG + DW) / 2 (EQ5) →

Step 3:
  νtG = π DG nG / 12 (EQ6) →

Step 4:
  νs = νtG / sin(λ) (EQ7) →

Step 5:
  μ = f(νs) (EQ8, piecewise) →

Step 6:
  To = 63000 Po / nG (EQ9) →
  WtG = 2 To / DG (EQ10) →
  WxG = f(WtG, φn, λ, μ) (EQ11) →
  WrG = f(WtG, φn, λ, μ) (EQ12) →

Step 7:
  Wf = f(μ, WtG, λ, φn) (EQ13) →

Step 8:
  PL = νs Wf / 33000 (EQ14) →

Step 9:
  Pi = Po + PL (EQ15) →

Step 10:
  η = (Po / Pi) × 100 (EQ16) →

Step 11:
  y (user input or table lookup) →

Step 12:
  pn = p cos(λ) (EQ17) →

Step 13:
  Kv = 1200 / (1200 + νtG) (EQ18) →

Step 14:
  Wd = WtG / Kv (EQ19) →

Step 15:
  σ = Wd / (y F pn) (EQ20) →

Step 16:
  Cs = f(DG, Bronze Type) (EQ21, piecewise) →

Step 17:
  mG = NG / NW (EQ22) →
  Cm = f(mG) (EQ23, piecewise) →

Step 18:
  Cv = f(νs) (EQ24, piecewise) →

Step 19:
  Fe = min(F, DW/3) (EQ25, piecewise) →

Step 20:
  WtR = Cs DG^0.8 Fe Cm Cv (EQ26) →

Step 21:
  Check: WtR > WtG (validation)

General Equations (reference only):
  a = 1 / Pd (GEN1)
  ht = 2.157 / Pd (GEN2)
  b = ht - a (GEN3)
  DrW = DW - 2b (GEN4)
  DoW = DW + 2a (GEN5)
  DrG = DG - 2b (GEN6)
  Dt = DG + 2a (GEN7)
  vW = π DW nW / 12 (GEN8)
  vG = π DG nG / 12 (GEN9)
  VR = nW / nG (GEN10)
```

### Implementation Notes
- **Tab System**: Three tabs (Procedure, General Equations, Variables) for organization
- **Variable Syncing**: All instances of same variable (across tabs) sync automatically
- **Input Types**: White = user input, Green = auto-calculated (but editable)
- **Piecewise Indicators**: Show which formula branch is active for EQ8, EQ21, EQ23, EQ24, EQ25
- **Final Validation**: Step 21 shows pass/fail with color coding (green = satisfactory, red = not satisfactory)
- **Summary Panel**: Shows key results (λ, η, Pi, WtG, σ, WtR)
- **Session Persistence**: LocalStorage saves all inputs and calculated values
- **MathJax**: Used for LaTeX equation rendering
- **Reset Function**: Clears all values with confirmation dialog

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table/lookup variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (21 steps)
- [x] Piecewise equations explained (5 piecewise equations with all cases)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Dropdown options documented
- [x] Lewis form factor table included
- [x] Calculation dependency chain documented
- [x] Special considerations for worm gears noted
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Implementation

**Implementation Note:** This calculator has high complexity due to:
1. Five piecewise equations with 15 total cases
2. Complex force calculations with shared denominators
3. Material-specific factors
4. Multiple interdependent steps
5. Final pass/fail validation

The calculator is already implemented and functional. This specification serves as documentation for maintenance, updates, or reimplementation.

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
