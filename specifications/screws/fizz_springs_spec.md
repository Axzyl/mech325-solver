# Calculator Specification: Helical Compression Springs Design

---

## 1. Metadata

- **Calculator Name:** Helical Compression Springs - Static Design Calculator
- **Source PDF:** fizz_springs.pdf
- **PDF Location:** `documents/screws/fizz_springs.pdf`
- **Description:** Comprehensive helical compression spring design calculator for static applications. Covers spring anatomy, stress calculations, wire diameter selection, spring constant determination, and complete design iteration following the Shigley methodology. Includes multiple design paths (iterating wire diameter vs. calculating based on spring index) and extensive material property lookups.
- **Complexity:** Complex
- **Tabs to Include:**
  - [x] Procedure (Two main design procedures: Iterate Wire Diameter and Calculate Based on C)
  - [x] General Equations (30+ equations for stress, deflection, stability)
  - [x] Variables (Essential for 60+ variables)

**Rationale for Tab Structure:**
The PDF contains two complete design procedures (sections 5.3.3 and 5.3.4), plus fundamental equations for spring anatomy, stress analysis, and fatigue. With 60+ variables, 30+ equations, and 6 major table lookups, all three tabs are necessary for effective use.

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Geometry - Basic** | | | | | | | |
| D | Mean Diameter | Distance to center of wire | in or mm | User Input/Equation | Page 2 | No | Can be input or calculated |
| d | Wire Diameter | Diameter of spring wire | in or mm | User Input/Table | Page 2, Table A-28 | Maybe | Selected from standard sizes |
| OD | Outer Diameter | Outside diameter of spring | in or mm | Equation | EQ1 | No | OD = D + d |
| ID | Inner Diameter | Inside diameter of spring | in or mm | Equation | EQ2 | No | ID = D - d |
| C | Spring Index | Ratio of mean to wire diameter | - | User Input/Equation | Page 2 | No | Best between 4 and 12 |
| **Coil Counts** | | | | | | | |
| N | Number of Coils | Total coils (same as Na per PDF) | - | Equation | Page 3 | No | N = Na per document |
| N<sub>a</sub> | Active Coils | Number of active coils | - | Equation | Page 3, Table 10-1 | No | 3 ≤ Na ≤ 15 requirement |
| N<sub>e</sub> | End Coils | Number of end coils | - | Table | Table 10-1 | No | Depends on end type |
| N<sub>t</sub> | Total Coils | Total number of coils | - | Table/Equation | Table 10-1 | No | Nt = Na + Ne |
| **Spring Type** | | | | | | | |
| spring_type | Spring Configuration | Over-a-rod, Free, In-a-hole | - | Dropdown | Page 9, flowchart | No | Affects D calculation |
| end_type | End Condition | Plain, Ground, Squared, etc. | - | Dropdown | Table 10-1 | No | 4 options, affects Ne, Nt, L0, Ls, p |
| set_removed | Set Removed | Before or After set removal | - | Dropdown | Table 10-6 | No | Affects allowable stress % |
| **Lengths** | | | | | | | |
| L<sub>0</sub> | Free Length | Unloaded spring length | in or mm | Table/Equation | Table 10-1 | No | Depends on end_type |
| L<sub>s</sub> | Solid Length | Fully compressed length | in or mm | Table | Table 10-1 | No | Depends on end_type |
| p | Pitch | Distance between coils | in or mm | Table | Table 10-1 | No | Depends on end_type |
| **Loading** | | | | | | | |
| F | Applied Load | Force on spring | lbf or N | User Input | Page 2 | No | General force |
| F<sub>max</sub> | Maximum Load | Peak load on spring | lbf or N | User Input | Page 2, fatigue | No | For design |
| F<sub>min</sub> | Minimum Load | Minimum cyclic load | lbf or N | User Input | Page 8, fatigue | No | For fatigue analysis |
| F<sub>a</sub> | Alternating Load | Amplitude of cyclic load | lbf or N | Equation | EQ-FAT1 | No | (Fmax - Fmin) / 2 |
| F<sub>m</sub> | Mean Load | Average cyclic load | lbf or N | Equation | EQ-FAT2 | No | (Fmax + Fmin) / 2 |
| F<sub>s</sub> | Load at Solid | Force when fully compressed | lbf or N | Equation | Page 9 | No | Fs = (1 + ξ) × Fmax |
| **Deflection** | | | | | | | |
| y | Deflection | Spring compression distance | in or mm | Equation | EQ-DEFL1 | No | Total deflection |
| y<sub>s</sub> | Deflection at Solid | Compression to solid length | in or mm | Equation | EQ-YS | No | ys = Fmax / k |
| y<sub>cr</sub> | Critical Deflection | Buckling deflection limit | in or mm | Equation | EQ-YCR | No | Stability check |
| ξ | Fractional Overrun | Clash allowance fraction | - | User Input | Page 9 | No | Typically 0.15, ξ ≥ 0.15 |
| **Stress Correction Factors** | | | | | | | |
| K<sub>s</sub> | Stress Correction Factor | Shear stress correction | - | Equation | EQ-KS | No | Ks = (2C + 1) / (2C) |
| K<sub>W</sub> | Wahl Factor | Curvature and shear correction | - | Equation | EQ-KW | No | KW = (4C - 1)/(4C - 4) + 0.615/C |
| K<sub>B</sub> | Bergsträsser Factor | Preferred stress factor | - | Equation | EQ-KB | No | KB = (4C + 2) / (4C - 3) |
| K<sub>C</sub> | Curvature Factor | Ratio of KB to Ks | - | Equation | EQ-KC | No | KC = KB / Ks |
| **Stresses** | | | | | | | |
| τ | Shear Stress | Actual shear stress in wire | psi or MPa | Equation | EQ-TAU | No | Use with KB |
| τ<sub>max</sub> | Maximum Shear Stress | Peak shear stress | psi or MPa | Equation | EQ-TAUMAX | No | Basic formula |
| τ<sub>a</sub> | Alternating Shear Stress | Stress amplitude (fatigue) | psi or MPa | Equation | EQ-TAUA | No | For fatigue |
| τ<sub>m</sub> | Mean Shear Stress | Mean stress (fatigue) | psi or MPa | Equation | EQ-TAUM | No | For fatigue |
| τ<sub>all</sub> | Allowable Shear Stress | Design limit stress | psi or MPa | Equation | EQ-TAUALL | No | τall = 0.56 Sut |
| **Material Strength Properties** | | | | | | | |
| material | Material Type | Spring wire material | - | Dropdown | Table 10-3 | No | Music wire, OQ&T, HD, Chrome-V, Chrome-Si, Stainless |
| A | Tensile Strength Constant | Coefficient for Sut formula | kpsi·in^m or MPa·mm^m | Table | Table 10-4 | Maybe | Based on material and diameter range |
| m | Tensile Strength Exponent | Exponent for Sut formula | - | Table | Table 10-4 | Maybe | Based on material and diameter range |
| S<sub>ut</sub> | Ultimate Tensile Strength | Minimum tensile strength | kpsi or MPa | Equation/Table | EQ-SUT | Maybe | Sut = A / d^m |
| S<sub>y</sub> | Yield Strength | Yield strength in tension | kpsi or MPa | Equation | Page 5 | No | Sy from Sut |
| S<sub>sy</sub> | Torsional Yield Strength | Yield strength in shear | kpsi or MPa | Equation | EQ-SSY | No | Ssy = 0.577 Sy or range |
| **Fatigue Strength** | | | | | | | |
| peened | Peening Treatment | Unpeened or Peened | - | Dropdown | Page 8 | No | Affects Ssa and Ssm |
| S<sub>sa</sub> | Alternating Shear Strength | Fatigue strength amplitude | kpsi or MPa | User Input/Table | Page 8 | No | 35 kpsi (unpeened) or 57.5 kpsi (peened) |
| S<sub>sm</sub> | Mean Shear Strength | Fatigue strength mean | kpsi or MPa | User Input/Table | Page 8 | No | 55 kpsi (unpeened) or 77.5 kpsi (peened) |
| S<sub>su</sub> | Ultimate Shear Strength | Shear ultimate strength | kpsi or MPa | Equation | EQ-SSU | No | Ssu = 0.67 Sut (Sines) |
| S<sub>se</sub> | Endurance Strength | Corrected endurance limit | kpsi or MPa | Equation | EQ-GOODMAN or EQ-GERBER | No | Goodman or Gerber |
| **Elastic Properties** | | | | | | | |
| E | Young's Modulus (Tension) | Elastic modulus in tension | Mpsi or GPa | Table | Table 10-5 | Maybe | Material and diameter dependent |
| G | Shear Modulus | Shear modulus | Mpsi or GPa | Table | Table 10-5 | Maybe | Material and diameter dependent |
| **Spring Constant** | | | | | | | |
| k | Spring Rate | Force per unit deflection | lbf/in or N/mm | Equation | EQ-K | No | k = F / y |
| **Stability** | | | | | | | |
| α | End Condition Constant | Stability end condition factor | - | Table | Table 10-2 | Maybe | 0.5, 0.707, 1, or 2 |
| λ<sub>eff</sub> | Effective Slenderness Ratio | Buckling parameter | - | Equation | EQ-LAMBDA | No | λeff = α L0 / D |
| C<sub>1</sub>' | Elastic Constant 1 | Random elastic constant | - | Equation | EQ-C1 | No | C1' = E / (2(E - G)) |
| C<sub>2</sub>' | Elastic Constant 2 | Random elastic constant | - | Equation | EQ-C2 | No | C2' = 2π² (E - G) / (2G + E) |
| **Safety Factors** | | | | | | | |
| n<sub>s</sub> | Safety Factor at Solid | FOS at solid height | - | Equation/User Input | Page 9 | No | ns ≥ 1.2 requirement |
| **Design Variables (Procedure Specific)** | | | | | | | |
| d<sub>rod</sub> | Rod Diameter | Diameter of central rod | in or mm | User Input | Page 9, flowchart | No | Only for Over-a-rod |
| d<sub>allow</sub> | Rod Allowance | Clearance for rod | in or mm | User Input | Page 9, flowchart | No | Only for Over-a-rod |
| d<sub>hole</sub> | Hole Diameter | Diameter of hole | in or mm | User Input | Page 9, flowchart | No | Only for In-a-hole |
| percent_sut | Percent of Sut Allowed | Allowable stress percentage | % | Table | Table 10-6 | Maybe | 45-70% depending on material and set |
| wire_gauge | Wire Gauge | American/Brown & Sharpe gauge | - | Table | Table A-28 | Maybe | For selecting d |
| rel_cost | Relative Material Cost | Cost factor for material | - | Table | Table 10-4 | Maybe | For figure of merit |
| γ | Specific Weight | Weight per unit volume | lb/in³ or N/mm³ | Table/User Input | Page 8 | No | For weight calculation |
| fom | Figure of Merit | Cost optimization metric | - | Equation | EQ-FOM | No | fom = -(rel_cost × γ × π² × d² × Nt × D) / 4 |
| **Intermediate Calculation Variables** | | | | | | | |
| α_calc | Alpha for C Calculation | Intermediate for spring index | psi or MPa | Equation | Page 10 | No | α = Ssy / ns |
| β | Beta for C Calculation | Intermediate for spring index | psi or MPa | Equation | Page 10 | No | β = 8(1 + ξ)Fmax / (π d²) |
| **Dynamic/Frequency** | | | | | | | |
| W | Spring Weight | Weight of spring | lb or N | Equation | EQ-W | No | W = π²d²DNaγ / 4 |
| f | Natural Frequency | Fundamental frequency | Hz | Equation | EQ-FREQ1 or EQ-FREQ2 | No | Depends on end condition |
| f<sub>cr</sub> | Critical Frequency | Operating frequency limit | Hz | User Input | Page 8 | No | fcr should be 15-20× operating freq |
| ω | Angular Frequency | Fundamental angular frequency | rad/s | Equation | EQ-OMEGA | No | ω = mπ √(kg/W), m = 1,2,3... |

**Notes:**
- Wire diameter d is typically selected from Table A-28 (standard wire gauges)
- Many material properties (A, m, E, G) require table lookups from Tables 10-4 and 10-5
- End condition constant α from Table 10-2 affects stability calculations
- The PDF uses both exact formulas and approximations (e.g., deflection with and without 1/(2C²) term)

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Geometry** | | | | | |
| EQ1 | `OD = D + d` | Outer diameter | D, d | OD | - |
| EQ2 | `ID = D - d` | Inner diameter | D, d | ID | - |
| EQ3 | `C = \frac{D}{d}` | Spring index | D, d | C | Best: 4 ≤ C ≤ 12 |
| **Stress Factors** | | | | | |
| EQ-KS | `K_s = \frac{2C + 1}{2C}` | Stress correction factor | C | Ks | - |
| EQ-KW | `K_W = \frac{4C - 1}{4C - 4} + \frac{0.615}{C}` | Wahl factor | C | KW | - |
| EQ-KB | `K_B = \frac{4C + 2}{4C - 3}` | Bergsträsser factor | C | KB | Preferred |
| EQ-KC | `K_C = \frac{K_B}{K_s} = \frac{2C(4C + 2)}{(4C - 3)(2C + 1)}` | Curvature factor | C, KB, Ks | KC | - |
| **Stress Calculations** | | | | | |
| EQ-TAUMAX | `\tau_{max} = \frac{Tr}{J} + \frac{F}{A} = \frac{8FD}{\pi d^3} + \frac{4F}{\pi d^2}` | Max shear stress (basic) | F, D, d | τmax | Not typically used |
| EQ-TAU | `\tau = K_B \frac{8FD}{\pi d^3}` | Shear stress with Bergsträsser | F, D, d, KB | τ | Preferred formula |
| **Deflection and Spring Rate** | | | | | |
| EQ-DEFL1 | `y = \frac{8FD^3N}{d^4G}(1 + \frac{1}{2C^2}) \approx \frac{8FD^3N}{d^4G}` | Total deflection | F, D, N, d, G, C | y | Approximation usually sufficient |
| EQ-K | `k = \frac{F}{y} \approx \frac{d^4G}{8D^3N}` | Spring rate | F, y (or d, G, D, N) | k | - |
| EQ-YS | `y_s = \frac{F_{max}}{k}` | Deflection at solid | Fmax, k | ys | - |
| **Material Strength** | | | | | |
| EQ-SUT | `S_{ut} = \frac{A}{d^m}` | Ultimate tensile strength | A, m, d | Sut | A and m from Table 10-4 |
| EQ-SSY | `S_{sy} = 0.577S_y` or `0.35S_{ut} \leq S_{sy} \leq 0.52S_{ut}` | Torsional yield strength | Sy or Sut | Ssy | Range formula |
| EQ-TAUALL | `\tau_{all} = 0.56S_{ut}` | Allowable shear stress | Sut | τall | - |
| **Fatigue** | | | | | |
| EQ-FAT1 | `F_a = \frac{F_{max} - F_{min}}{2}` | Alternating load | Fmax, Fmin | Fa | - |
| EQ-FAT2 | `F_m = \frac{F_{max} + F_{min}}{2}` | Mean load | Fmax, Fmin | Fm | - |
| EQ-TAUA | `\tau_a = K_B \frac{8F_aD}{\pi d^3}` | Alternating shear stress | Fa, D, d, KB | τa | - |
| EQ-TAUM | `\tau_m = K_B \frac{8F_mD}{\pi d^3}` | Mean shear stress | Fm, D, d, KB | τm | - |
| EQ-SSU | `S_{su} = 0.67S_{ut}` | Ultimate shear (Sines) | Sut | Ssu | - |
| EQ-GOODMAN | `S_{se} = \frac{S_{sa}}{1 - \frac{S_{sm}}{S_{su}}}` | Endurance (Goodman) | Ssa, Ssm, Ssu | Sse | - |
| EQ-GERBER | `S_{se} = \frac{S_{sa}}{1 - (\frac{S_{sm}}{S_{su}})^2}` | Endurance (Gerber) | Ssa, Ssm, Ssu | Sse | - |
| **Stability** | | | | | |
| EQ-LAMBDA | `\lambda_{eff} = \frac{\alpha L_0}{D}` | Effective slenderness ratio | α, L0, D | λeff | - |
| EQ-C1 | `C_1' = \frac{E}{2(E - G)}` | Elastic constant 1 | E, G | C1' | - |
| EQ-C2 | `C_2' = \frac{2\pi^2(E - G)}{2G + E}` | Elastic constant 2 | E, G | C2' | - |
| EQ-YCR | `y_{cr} = L_0 C_1'[1 - (1 - \frac{C_2'}{\lambda_{eff}^2})^{1/2}]` | Critical deflection | L0, C1', C2', λeff | ycr | - |
| EQ-STAB | `L_0 < \frac{\pi D}{\alpha}[\frac{2(E-G)}{2G+E}]^{1/2}` | Absolute stability condition | D, α, E, G | - | For steels: L0 < 2.63 D/α |
| **Coil and Length Formulas (from Table 10-1)** | | | | | |
| EQ-NE-PLAIN | `N_e = 0` | End coils for plain ends | - | Ne | - |
| EQ-NE-GROUND | `N_e = 1` | End coils for plain & ground | - | Ne | - |
| EQ-NE-SQUARED | `N_e = 2` | End coils for squared/closed | - | Ne | - |
| EQ-NT-PLAIN | `N_t = N_a` | Total coils (plain) | Na | Nt | - |
| EQ-NT-GROUND | `N_t = N_a + 1` | Total coils (plain & ground) | Na | Nt | - |
| EQ-NT-SQUARED | `N_t = N_a + 2` | Total coils (squared or ground) | Na | Nt | - |
| EQ-L0-PLAIN | `L_0 = pN_a + d` | Free length (plain) | p, Na, d | L0 | - |
| EQ-L0-GROUND | `L_0 = p(N_a + 1)` | Free length (plain & ground) | p, Na | L0 | - |
| EQ-L0-SQUARED | `L_0 = pN_a + 3d` | Free length (squared/closed) | p, Na, d | L0 | - |
| EQ-L0-SQ-GR | `L_0 = pN_a + 2d` | Free length (squared & ground) | p, Na, d | L0 | - |
| EQ-LS-PLAIN | `L_s = d(N_t + 1)` | Solid length (plain) | d, Nt | Ls | - |
| EQ-LS-OTHER | `L_s = dN_t` | Solid length (ground/squared) | d, Nt | Ls | - |
| EQ-PITCH-PLAIN | `p = \frac{L_0 - d}{N_a}` | Pitch (plain) | L0, d, Na | p | - |
| EQ-PITCH-GROUND | `p = \frac{L_0}{N_a + 1}` | Pitch (plain & ground) | L0, Na | p | - |
| EQ-PITCH-SQUARED | `p = \frac{L_0 - 3d}{N_a}` | Pitch (squared/closed) | L0, d, Na | p | - |
| EQ-PITCH-SQ-GR | `p = \frac{L_0 - 2d}{N_a}` | Pitch (squared & ground) | L0, d, Na | p | - |
| **Design Procedure Equations** | | | | | |
| EQ-FS | `F_s = (1 + \xi)F_{max}` | Force at solid height | Fmax, ξ | Fs | ξ typically 0.15 |
| EQ-NS-CHECK | `n_s = \frac{S_{sy}}{n_s} = K_B \frac{8F_sD}{\pi d^3}` | Safety factor at solid | Ssy, ns, KB, Fs, D, d | - | Rearranged for checking |
| EQ-ALPHA | `\alpha_{calc} = \frac{S_{sy}}{n_s}` | Alpha for C calculation | Ssy, ns | α_calc | Design procedure variable |
| EQ-BETA | `\beta = \frac{8(1 + \xi)F_{max}}{\pi d^2}` | Beta for C calculation | Fmax, ξ, d | β | Design procedure variable |
| EQ-C-SOLVE | `C = \frac{2\alpha - \beta}{4\beta} + \sqrt{(\frac{2\alpha - \beta}{4\beta})^2 - \frac{3\alpha}{4\beta}}` | Solve for spring index | α_calc, β | C | Pick larger of two solutions |
| EQ-D-OVERROD | `D = d_{rod} + d + d_{allow}` | Mean diameter (over-a-rod) | drod, d, dallow | D | spring_type = Over-a-rod |
| EQ-D-FREE-A | `S_{sy} = \text{const}(A)/d^m` | Free as-wound (const formula) | A, m, d | Ssy | spring_type = Free, as-wound |
| EQ-D-FREE-SR | `S_{sy} = 0.65A/d^m` | Free set-removed | A, m, d | Ssy | spring_type = Free, set removed |
| EQ-D-FREE-CALC | `D = \frac{S_{sy}\pi d^3}{8n_s(1 + \xi)F_{max}}` | Mean diameter (free, set removed) | Ssy, d, ns, ξ, Fmax | D | spring_type = Free |
| EQ-D-HOLE | `D = d_{hole} - d - d_{allow}` | Mean diameter (in-a-hole) | dhole, d, dallow | D | spring_type = In-a-hole |
| EQ-D-FROM-C | `D = Cd` | Mean diameter from index | C, d | D | - |
| EQ-NA-CALC | `N_a = \frac{dG}{8kC^3}` | Active coils calculation | d, G, k, C | Na | Design procedure |
| EQ-FOM | `\text{fom} = -(\text{rel\_cost}) \frac{\gamma \pi^2 d^2 N_t D}{4}` | Figure of merit | rel_cost, γ, d, Nt, D | fom | Maximize (less negative) |
| **Wire Diameter Iteration (Procedure 5.3.3)** | | | | | |
| EQ-D-ITER | `d = (0.163K_B \frac{C}{A})^{1/(2-m)}` | Wire diameter guess | KB, C, A, m | d | Use C = 10 initially |
| EQ-NS-CALC | `n_s = \frac{7.363Ad^{2-m}}{K_BC}` | Safety factor check | A, d, m, KB, C | ns | Should be ≈ 1.2 |
| **Frequency** | | | | | |
| EQ-W | `W = AL\gamma = \frac{\pi^2 d^2 DN_a\gamma}{4}` | Spring weight | d, D, Na, γ | W | - |
| EQ-OMEGA | `\omega = m\pi\sqrt{\frac{kg}{W}}` | Angular frequency | k, g, W, m | ω | m = 1, 2, 3, ... |
| EQ-FREQ1 | `f = \frac{1}{2}\sqrt{\frac{kg}{W}}` | Frequency (both ends contact) | k, g, W | f | - |
| EQ-FREQ2 | `f = \frac{1}{4}\sqrt{\frac{kg}{W}}` | Frequency (one end contacts) | k, g, W | f | - |

**Notes:**
- Many equations are piecewise based on end_type (from Table 10-1)
- spring_type affects D calculation (EQ-D-OVERROD, EQ-D-FREE-CALC, EQ-D-HOLE)
- The C-solve equation (EQ-C-SOLVE) gives two solutions; always pick the larger one
- Stability check for steels simplifies to L0 < 2.63 D / α

---

## 4. Procedure Steps

### 5.3.2 Design for Static Service (Overview)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 0 | Choose wire diameter approach | Dropdown | Iterate d (5.3.3) or Calculate based on C (5.3.4) | Two complete procedures |

### Procedure 5.3.3: Iterate Wire Diameter

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Setup** | | | | |
| 1 | Input design requirements | User Input | Enter Fmax, desired ξ (0.15), y or k | Starting parameters |
| 2 | Select material type | Dropdown | Music wire, OQ&T, Hard-drawn, Chrome-V, Chrome-Si, Stainless | Sets material |
| 3 | Select A, m, E, G for material | Table Lookup | Table 10-4 (A, m), Table 10-5 (E, G) | Yellow input, may have multiple rows per material |
| 4 | Set initial constants | User Input | ns = 1.2, ξ = 0.15 | Starting values |
| 5 | Get allowable % of Sut | Table Lookup | Table 10-6, based on material and set_removed | Yellow input |
| 6 | Choose range of wire diameters | Table Lookup | Table A-28, estimate range | Yellow input, multiple selections |
| **For Each Wire Diameter d** | | | | |
| 7-1 | Calculate Sut for this d | Equation | Use EQ-SUT: Sut = A / d^m | - |
| 7-2 | Calculate Ssy | Equation | Ssy = percent × Sut | percent from Table 10-6 |
| 7-3 | Calculate α | Equation | α = Ssy / ns | Intermediate variable |
| 7-4 | Calculate β | Equation | β = 8(1 + ξ)Fmax / (π d²) | Intermediate variable |
| 7-5 | Solve for C | Equation | Use EQ-C-SOLVE, pick larger solution | 4 ≤ C ≤ 12 required |
| 7-6 | Check C range | Validation | Is 4 ≤ C ≤ 12? | If not, mark as infeasible |
| 7-7 | Calculate D | Equation | D = C × d | - |
| 7-8 | Calculate KB | Equation | KB = (4C + 2) / (4C - 3) | - |
| 7-9 | Calculate τ | Equation | τ = KB × 8Fs × D / (π d³) where Fs = (1+ξ)Fmax | - |
| 7-10 | Calculate ns (actual) | Equation | ns = Ssy / τ | Should be close to 1.2 |
| 7-11 | Select spring type | Dropdown | Over-a-rod, Free, In-a-hole | Affects next steps |
| 7-12a | If Over-a-rod: input drod, dallow | User Input | Enter rod diameter and allowance | - |
| 7-12b | If Over-a-rod: calculate D | Equation | D = drod + d + dallow | Override previous D |
| 7-12c | If Over-a-rod: recalculate C | Equation | C = D / d | Override previous C |
| 7-13a | If In-a-hole: input dhole, dallow | User Input | Enter hole diameter and allowance | - |
| 7-13b | If In-a-hole: calculate D | Equation | D = dhole - d - dallow | Override previous D |
| 7-13c | If In-a-hole: recalculate C | Equation | C = D / d | Override previous C |
| 7-14 | Calculate OD and ID | Equation | OD = D + d, ID = D - d | - |
| 7-15 | Calculate k (if y given) or y (if k given) | Equation | k = Fmax / y or y = Fmax / k | One must be input |
| 7-16 | Calculate Na | Equation | Na = dG / (8kC³) | - |
| 7-17 | Check Na range | Validation | Is 3 ≤ Na ≤ 15? | If not, mark as infeasible |
| 7-18 | Select end type | Dropdown | Plain, Plain & Ground, Squared/Closed, Squared & Ground | From Table 10-1 |
| 7-19 | Get Ne from table | Table | Table 10-1 based on end_type | Ne = 0, 1, or 2 |
| 7-20 | Calculate Nt | Equation | Nt = Na + Ne (from Table 10-1) | - |
| 7-21 | Calculate Ls | Equation | Use Table 10-1 formula based on end_type | Ls = dNt or d(Nt+1) |
| 7-22 | Calculate L0 | Equation | Use Table 10-1 formula based on end_type | Various formulas |
| 7-23 | Calculate p | Equation | Use Table 10-1 formula based on end_type | Various formulas |
| 7-24 | Calculate ξ (actual) | Equation | ξ = (L0 - Ls) / y - 1 | Check if ξ ≥ 0.15 |
| 7-25 | Check ξ requirement | Validation | Is ξ ≥ 0.15? | If not, mark as infeasible |
| 7-26 | Select end condition for stability | Dropdown | Fixed ends, One fixed/one pivoted, Both pivoted, One clamped/one free | From Table 10-2 |
| 7-27 | Get α (stability) from table | Table | Table 10-2 based on end condition | α = 0.5, 0.707, 1, or 2 |
| 7-28 | Calculate stability check | Equation | Check if L0 < 2.63 D / α (for steels) | If fails, mark as infeasible |
| 7-29 | Calculate figure of merit | Equation | fom = -(rel_cost) × γ × π² × d² × Nt × D / 4 | From Table 10-4 |
| 7-30 | Store results for this d | Data Storage | Save all calculated values | - |
| **After All Diameters Tested** | | | | |
| 8 | Eliminate infeasible designs | Filter | Remove any with failed validation checks | C, Na, ξ, stability |
| 9 | Select best design | Comparison | Choose design with highest (least negative) fom | Maximize fom |
| 10 | Display final design | Output | Show d, D, C, OD, ID, Na, Nt, Ls, L0, p, ns, ξ, fom | - |

### Procedure 5.3.4: Calculate Based on C (Alternative Method)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **Setup** | | | | |
| 1 | Input design requirements | User Input | Enter Fmax, y or k, ξ = 0.15, ns = 1.2 | Starting parameters |
| 2 | Select material type | Dropdown | Music wire, OQ&T, Hard-drawn, Chrome-V, Chrome-Si, Stainless | Sets material |
| 3 | Select A, m values | Table Lookup | Table 10-4 based on material | Yellow input |
| 4 | Get allowable % of Sut | Table Lookup | Table 10-6 based on material and set_removed | Yellow input |
| 5 | Calculate Ssy formula | Equation | Ssy = percent × A / d^m | Function of d |
| 6 | Choose initial C | User Input | Use C = 10 as starting guess | Per example in PDF |
| 7 | Calculate KB for C = 10 | Equation | KB = (4C + 2) / (4C - 3) | - |
| 8 | Solve for d | Equation | d = (0.163 KB C / A)^(1/(2-m)) | EQ-D-ITER |
| 9 | Find closest standard d | Table Lookup | Table A-28, select nearest wire diameter | Yellow input |
| 10 | Check ns for this d | Equation | ns = 7.363 A d^(2-m) / (KB C) | Should be ≈ 1.2 |
| 11 | Adjust d if needed | Iteration | If ns is way off, try adjacent standard d | Optional |
| 12 | Input or calculate k | User Input/Equation | k = F / y or input k directly | - |
| 13 | Select E and G | Table Lookup | Table 10-5 based on material and d range | Yellow input |
| 14 | Calculate Na | Equation | Na = dG / (8kC³) | - |
| 15 | Check Na range | Validation | Is 3 ≤ Na ≤ 15? | If not, adjust C |
| 16 | Adjust C if Na out of range | Iteration | Try different C within 4 ≤ C ≤ 12 | Iterative |
| 17 | Select end type | Dropdown | Plain, Plain & Ground, Squared/Closed, Squared & Ground | From Table 10-1 |
| 18 | Get formulas from Table 10-1 | Table | Ne, Nt, L0, Ls, p formulas | Based on end_type |
| 19 | Calculate all dimensional parameters | Equation | Calculate Ne, Nt, D, OD, ID, L0, Ls, p | Use Table 10-1 formulas |
| 20 | Check stability | Equation | Verify L0 < 2.63 D / α | From Table 10-2 |
| 21 | Display final design | Output | Show all design parameters | - |

### Additional Analysis (Optional)

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| FAT-1 | Input cyclic loads | User Input | Enter Fmax, Fmin | For fatigue analysis |
| FAT-2 | Calculate Fa and Fm | Equation | Fa = (Fmax - Fmin)/2, Fm = (Fmax + Fmin)/2 | - |
| FAT-3 | Calculate τa and τm | Equation | Use EQ-TAUA and EQ-TAUM | - |
| FAT-4 | Select peening status | Dropdown | Unpeened or Peened | Affects Ssa, Ssm |
| FAT-5 | Get Ssa and Ssm | Table/User Input | Page 8: Unpeened (35, 55 kpsi) or Peened (57.5, 77.5 kpsi) | Yellow input |
| FAT-6 | Calculate Ssu | Equation | Ssu = 0.67 Sut | Sines criterion |
| FAT-7 | Calculate Sse (Goodman) | Equation | Sse = Ssa / (1 - Ssm/Ssu) | - |
| FAT-8 | Calculate Sse (Gerber) | Equation | Sse = Ssa / (1 - (Ssm/Ssu)²) | Alternative |
| FAT-9 | Compare τa to Sse | Comparison | Check if τa < Sse | Fatigue safety check |
| FREQ-1 | Calculate spring weight W | Equation | W = π²d²DNaγ / 4 | - |
| FREQ-2 | Select frequency boundary | Dropdown | Both ends contact or One end contacts | - |
| FREQ-3 | Calculate natural frequency | Equation | f = (1/2 or 1/4) × √(kg/W) | Depends on boundary |
| FREQ-4 | Input operating frequency | User Input | Enter actual operating frequency | - |
| FREQ-5 | Check frequency ratio | Validation | fcr should be 15-20× operating frequency | Avoid resonance |

**Notes:**
- Two main design procedures are provided; users should choose one
- Procedure 5.3.3 (Iterate Wire Diameter) is more comprehensive but requires testing multiple diameters
- Procedure 5.3.4 (Calculate Based on C) is faster but may require iteration if Na is out of range
- Both procedures require extensive table lookups (6 tables total)
- Stability, fatigue, and frequency checks are optional but recommended

---

## 5. Additional Notes

### Piecewise Equations

**End Type Formulas (from Table 10-1):**
All formulas for Ne, Nt, L0, Ls, and p depend on end_type:
- **Plain:** Ne = 0, Nt = Na, L0 = pNa + d, Ls = d(Nt + 1), p = (L0 - d)/Na
- **Plain & Ground:** Ne = 1, Nt = Na + 1, L0 = p(Na + 1), Ls = dNt, p = L0/(Na + 1)
- **Squared or Closed:** Ne = 2, Nt = Na + 2, L0 = pNa + 3d, Ls = dNt, p = (L0 - 3d)/Na
- **Squared & Ground:** Ne = 2, Nt = Na + 2, L0 = pNa + 2d, Ls = dNt, p = (L0 - 2d)/Na

**Mean Diameter D (from flowchart page 9):**
Depends on spring_type:
- **Over-a-rod:** D = drod + d + dallow
- **Free (as-wound):** Ssy = const(A)/d^m, then solve for D from stress equation
- **Free (set removed):** Ssy = 0.65A/d^m, then D = Ssyπd³ / [8ns(1+ξ)Fmax]
- **In-a-hole:** D = dhole - d - dallow

**Torsional Yield Strength Ssy:**
Multiple formulas:
- Ssy = 0.577 Sy (von Mises)
- 0.35 Sut ≤ Ssy ≤ 0.52 Sut (range)
- Ssy = 0.56 Sut (used in one location)
- Ssy = percent × Sut (from Table 10-6)

**Frequency Formula:**
Depends on end contact:
- Both ends always contact: f = (1/2)√(kg/W)
- One end contacts: f = (1/4)√(kg/W)

### Validation Rules

**Design Requirements (from page 9):**
- 4 ≤ C ≤ 12 (spring index)
- 3 ≤ Na ≤ 15 (active coils)
- ξ ≥ 0.15 (fractional overrun)
- ns ≥ 1.2 (safety factor at solid height)

**Stability (for steels, page 4):**
- L0 < 2.63 D / α

**Frequency (page 8):**
- fcr should be 15-20 times higher than operating frequency to avoid resonance

**Positive Values:**
- All dimensions (D, d, L0, Ls, p) > 0
- All loads (F, Fmax, Fmin) ≥ 0
- All stresses > 0
- k > 0
- For fatigue: Fmax > Fmin ≥ 0

### Standard Values / Constants

**Typical Design Values:**
- ns = 1.2 (safety factor at solid)
- ξ = 0.15 (fractional overrun)
- C = 10 (initial guess for wire diameter iteration)
- g = 386.4 in/s² or 9810 mm/s² (acceleration due to gravity)

**Material Properties (Tables 10-4, 10-5):**
- Music wire A228: A = 201 kpsi·in^m (2211 MPa·mm^m), m = 0.145, E = 29.5 Mpsi (203.4 GPa), G = 12.0 Mpsi (82.7 GPa)
- OQ&T wire A229: A = 147 kpsi·in^m, m = 0.187, E = 29.0 Mpsi, G = 11.85 Mpsi
- Hard-drawn A227: A = 140 kpsi·in^m, m = 0.190, E = 28.8 Mpsi, G = 11.7 Mpsi
- Chrome-vanadium A232: A = 169 kpsi·in^m, m = 0.168, E = 29.5 Mpsi, G = 11.2 Mpsi
- Chrome-silicon A401: A = 202 kpsi·in^m, m = 0.108, E = 29.5 Mpsi, G = 11.2 Mpsi
- 302 Stainless A313: A = 169 kpsi·in^m, m = 0.146, E = 28 Mpsi, G = 10 Mpsi

**Fatigue Strength (page 8):**
- Unpeened: Ssa = 35 kpsi (241 MPa), Ssm = 55 kpsi (379 MPa)
- Peened: Ssa = 57.5 kpsi (398 MPa), Ssm = 77.5 kpsi (534 MPa)

**End Condition Constants (Table 10-2):**
- Fixed ends (parallel surfaces): α = 0.5
- One fixed, one pivoted: α = 0.707
- Both ends pivoted: α = 1
- One clamped, one free: α = 2

### Dropdown Options

**material (Material Type):**
- Music wire (0.80-0.95C) - Best, toughest, highest tensile strength, small springs
- Oil-tempered wire (0.60-0.70C) - General purpose, larger sizes
- Hard-drawn wire (0.60-0.70C) - Cheapest, low accuracy
- Chrome-vanadium - High stress, fatigue resistance, good for shock
- Chrome-silicon - Highly stressed, long life, shock loading
- 302 Stainless steel - Corrosion resistance

**spring_type (Spring Configuration):**
- Over-a-rod - Spring around a central rod
- Free (as-wound) - Unconstrained spring, as manufactured
- Free (set removed) - Unconstrained spring, after set removal
- In-a-hole - Spring inside a cylindrical hole

**end_type (End Condition):**
- Plain - No special end treatment
- Plain and Ground - Plain ends with ground surfaces
- Squared or Closed - Ends squared off or closed
- Squared and Ground - Squared ends with ground surfaces

**set_removed (Set Treatment):**
- Before Set Removed - Includes KW or KB factors
- After Set Removed - Includes Ks factor (higher allowable stress)

**peened (Peening Treatment):**
- Unpeened - No shot peening
- Peened - Shot peened for improved fatigue life

**end_condition_stability (from Table 10-2):**
- Spring supported between flat parallel surfaces (fixed ends)
- One end supported by flat surface perpendicular to spring axis (fixed); other end pivoted (hinged)
- Both ends pivoted (hinged)
- One end clamped; other end free

### Future Enhancement Opportunities

**Interactive Table Lookups:**
- A and m from Table 10-4 (could auto-fill based on material and d range)
- E and G from Table 10-5 (could auto-fill based on material and d range)
- Wire diameter d from Table A-28 (dropdown of standard wire gauges)
- Percent of Sut from Table 10-6 (could auto-fill based on material and set_removed)
- End condition constant α from Table 10-2 (could auto-fill based on end_condition_stability)
- Fatigue strengths Ssa and Ssm (could auto-fill based on peened status)

**Automated Design Iteration:**
- Run procedure 5.3.3 for multiple wire diameters automatically
- Display results in sortable table
- Auto-highlight feasible designs
- Recommend best design based on fom

**Flowchart Integration:**
- Interactive flowchart from page 9 to guide users through spring_type selection
- Auto-populate formulas based on path taken

**Visualization:**
- Draw spring with actual proportions (D, d, L0, Ls, coils)
- Show buckling limit graphically
- Plot fatigue diagram (Goodman/Gerber)

### Special Considerations

**Two Complete Design Procedures:**
- Procedure 5.3.3 (Iterate Wire Diameter): More comprehensive, tests multiple diameters, uses figure of merit for optimization
- Procedure 5.3.4 (Calculate Based on C): Faster, assumes C = 10 initially, requires less iteration

**User Must Choose:**
- spring_type affects D calculation significantly
- end_type affects 5 different formulas (Ne, Nt, L0, Ls, p)
- Material selection affects A, m, E, G, allowable stress %, and cost

**Table Complexity:**
- Table A-28 has multiple columns (American Wire Gauge, Music Wire diameter, different standards)
- Table 10-4 has multiple rows per material (different diameter ranges)
- Table 10-5 has different E/G values for different wire diameter ranges
- All table lookups should have clear references to page numbers and table/figure numbers

**Approximations:**
- Deflection formula: exact version includes 1/(2C²) term, but approximation (without it) is usually sufficient
- For N: document states "N = Na idk why"
- Some equations shown but marked as "Ignore this one apparently"

**Informal Language:**
- PDF contains informal comments ("This shit is driving me up the Wahl factor", "you're fuck try another d maybe idk")
- Calculator should use professional terminology

### Calculation Order / Dependencies

**Procedure 5.3.3 (Iterate Wire Diameter):**
```
User Inputs: Fmax, ξ, material →
Table 10-4 → A, m
Table 10-5 → E, G
Table 10-6 → percent_sut →
Choose range of d from Table A-28 →

For each d:
  Sut = A / d^m →
  Ssy = percent × Sut →
  α = Ssy / ns →
  β = 8(1+ξ)Fmax / (πd²) →
  C = [2α-β]/[4β] + √([2α-β]/[4β])² - 3α/[4β] →
  D = Cd →
  KB = (4C+2)/(4C-3) →
  Check constraints (C, Na, ξ, stability) →
  Calculate fom →

Select d with best (highest) fom →
Output final design
```

**Procedure 5.3.4 (Calculate Based on C):**
```
User Inputs: Fmax, k or y, material →
Table 10-4 → A, m
Table 10-5 → E, G
Table 10-6 → percent_sut →

C = 10 (initial guess) →
KB = (4C+2)/(4C-3) →
d = (0.163 KB C / A)^(1/(2-m)) →
Select nearest d from Table A-28 →
ns = 7.363 A d^(2-m) / (KB C) →

Calculate k or y (whichever not given) →
Na = dG / (8kC³) →
Adjust C if Na out of range →

Calculate all other parameters →
Output final design
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized (60+ variables)
- [x] Source references noted for all table/graph variables (6 tables)
- [x] All equations extracted and converted to LaTeX (40+ equations)
- [x] Procedure steps documented (2 complete procedures + optional analyses)
- [x] Piecewise equations explained (end type, spring type, frequency, Ssy)
- [x] Validation rules documented (C, Na, ξ, ns, stability, frequency)
- [x] Future enhancements noted (interactive tables, automation, visualization)
- [x] Tab structure decision justified (3 tabs for complex procedures + many equations + many variables)
- [x] Specification reviewed against PDF (pages 1-18 covered, section 5.3)
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [ ] Ready for Implementation

**Specification Author:** Claude Code
**Date Created:** 2025-01-09
**Last Updated:** 2025-01-09

**Implementation Notes for HTML Calculator:**

**Major Design Decisions:**
1. **Two-Path Design:** Consider implementing both procedures (5.3.3 and 5.3.4) as separate tabs or modes within the Procedure tab
2. **Procedure 5.3.3 Approach:** Could create a multi-row table where user tests multiple d values side-by-side, with automatic feasibility flagging and fom comparison
3. **Flowchart Integration:** The flowchart on page 9 is critical - consider making it interactive or at minimum providing clear decision tree

**Table Lookup Complexity:**
- 6 major tables need yellow input boxes (10-1, 10-2, 10-3, 10-4, 10-5, 10-6, A-28)
- Table 10-4 has multiple rows per material (different diameter ranges) - may need sub-selection
- Table 10-5 has diameter-dependent E and G values
- Table A-28 is very large (wire gauge conversion) - consider searchable dropdown

**Conditional Visibility:**
- Many variables only apply for specific spring_type selections
- drod, dallow only for Over-a-rod
- dhole, dallow only for In-a-hole
- Ne, Nt, L0, Ls, p formulas all depend on end_type
- Fmin only needed for fatigue analysis
- Peening status only for fatigue

**Calculation Modes:**
- User must specify either y or k (one is input, other is calculated)
- Multiple paths through design procedure depending on spring_type
- Stability check is steel-specific (simplified formula)

**Informal Language Cleanup:**
- PDF has casual/humorous comments - use professional terminology in calculator
- Some equations marked "Ignore this one" - exclude from implementation

**Recommended Calculator Structure:**
1. **Procedure Tab:**
   - Mode selector: "Iterate Wire Diameter" vs. "Calculate Based on C"
   - Guided workflow with clear step numbers
   - Table for testing multiple d values (if Iterate mode)
   - Feasibility indicators (green checkmark / red X)
   - Figure of merit comparison

2. **General Equations Tab:**
   - Grouped by category (Geometry, Stress, Deflection, Material, Fatigue, Stability, Frequency)
   - All equations editable for what-if analysis

3. **Variables Tab:**
   - Organized by usage (Input, Calculated, Table Lookup)
   - Clear indicators of which are required vs. optional
   - Conditional visibility for spring_type-specific variables

**Testing Priority:**
- Validate both procedures against worked examples if available
- Test all piecewise equations (end_type variations)
- Verify table lookup values
- Check constraint validation (C, Na, ξ, ns ranges)
- Test figure of merit calculation and comparison

---

## Template Version

Template Version: 1.0
Specification Version: 1.0
