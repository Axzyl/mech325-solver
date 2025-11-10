# Calculator Specification: Fasteners/Bolts Design and Analysis

---

## 1. Metadata

- **Calculator Name:** Fasteners/Bolts Design and Factor of Safety Calculator
- **Source PDF:** fizz_fasteners.pdf
- **PDF Location:** `documents/screws/fizz_fasteners.pdf`
- **Description:** Comprehensive bolt/fastener design calculator covering design selection (bolt length, stiffness, member stiffness) and safety factor calculations (preload, overload, yield, joint separation, and Goodman fatigue analysis). Includes extensive table lookups for bolt properties and multiple piecewise equations based on bolt configuration and units.
- **Complexity:** Complex
- **Tabs to Include:**
  - [x] Procedure (PDF has numbered steps in two main procedures)
  - [x] General Equations (>20 equations total)
  - [x] Variables (Essential for this many variables)

**Rationale for Tab Structure:**
The PDF contains two major procedures (5.4.1 Design Selection with 3 main steps, and 5.4.2 Factor of Safety with 6 calculation types), plus 20+ equations and 50+ variables. The Procedure tab will organize the workflow, General Equations will provide quick reference to all formulas, and Variables tab will manage the extensive variable list. This three-tab structure is essential for usability given the complexity.

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Configuration** | | | | | | | |
| config | Bolt Arrangement | Figure (a) or (b) configuration | - | Dropdown | Page 1, diagrams | No | Affects grip length calculation |
| unit_system | Unit System | Inches or mm | - | Dropdown | Page 2 | No | Affects LT calculation |
| **Geometric Dimensions** | | | | | | | |
| h | Recess Depth | Depth of countersunk recess (fig b) | in or mm | User Input | Page 1, diagram (b) | No | Only for config (b) |
| t<sub>1</sub> | Top Plate Thickness | Thickness of material on bolt head side | in or mm | User Input | Page 1, diagrams | No | For config (a) |
| t<sub>2</sub> | Bottom Plate Thickness | Thickness of material on nut side | in or mm | User Input | Page 1, diagrams | No | All configs |
| d | Nominal Bolt Diameter | Standard bolt diameter | in or mm | User Input | Throughout | No | Key parameter |
| H | Nut Height | Height of the nut | in or mm | User Input | Page 2, EQ for L | No | Only for config (a) |
| **Calculated Lengths** | | | | | | | |
| l | Grip Length | Material squeezed between bolt and nut | in or mm | Equation | EQ1-EQ2 | No | Piecewise on config |
| L | Bolt Length | Total fastener length | in or mm | Equation | EQ3-EQ4 | No | Piecewise, round up to 1/4" |
| L<sub>T</sub> | Threaded Length | Length of threaded portion | in or mm | Equation | EQ5-EQ6 | No | Piecewise on units and L |
| l<sub>d</sub> | Unthreaded in Grip | Unthreaded portion within grip | in or mm | Equation | EQ7 | No | - |
| l<sub>t</sub> | Threaded in Grip | Threaded portion within grip | in or mm | Equation | EQ8 | No | - |
| **Areas** | | | | | | | |
| A<sub>d</sub> | Unthreaded Area | Cross-section of unthreaded portion | in² or mm² | Equation | EQ9 | No | - |
| A<sub>t</sub> | Tensile Stress Area | Effective area of threaded portion | in² or mm² | Table | Table 8-1 (metric) or 8-2 (UNC/UNF) | Maybe | Based on d and thread type |
| **Material Properties** | | | | | | | |
| E<sub>b</sub> | Bolt Young's Modulus | Elastic modulus of bolt material | psi or MPa | User Input | Page 2, EQ10 | No | Typically 30e6 psi (steel) |
| E<sub>m</sub> | Member Young's Modulus | Elastic modulus of member material | psi or MPa | User Input | Page 4 | No | Can differ from bolt |
| **Stiffness Values** | | | | | | | |
| k<sub>b</sub> | Bolt Stiffness | Spring rate of the bolt | lbf/in or N/mm | Equation | EQ10 | No | - |
| k<sub>i</sub> | Member Section Stiffness | Spring rate of individual section | lbf/in or N/mm | Equation | EQ11 | No | Calculated for each section |
| d<sub>inner</sub> | Section Inner Diameter | Inner diameter for member section | in or mm | User Input | Page 5, EQ11 | No | d for plates, washer ID for washers |
| D<sub>section</sub> | Section Outer Diameter | Shortest distance between lines | in or mm | User Input | Page 5, diagram & EQ11 | No | From frustum diagram |
| t<sub>section</sub> | Section Thickness | Thickness of member section | in or mm | User Input | Page 5 | No | Individual or combined |
| k<sub>top</sub> | Top Half Stiffness | Combined stiffness of top sections | lbf/in or N/mm | Equation | EQ12 | No | If symmetric |
| k<sub>t</sub> | Total Member Stiffness | Combined stiffness of all members | lbf/in or N/mm | Equation | EQ13-EQ14 | No | Piecewise on symmetry |
| k<sub>m</sub> | Member Stiffness (alias) | Same as kt | lbf/in or N/mm | Equation | = kt | No | Used in later equations |
| **Loading** | | | | | | | |
| P<sub>tot</sub> | Total Load | Total load on tension joint | lbf or N | User Input | Page 6, EQ15 | No | - |
| N | Number of Bolts | Count of bolts in joint | - | User Input | Page 6, EQ15 | No | Integer ≥ 1 |
| P | Load Per Bolt | Load carried by each bolt | lbf or N | Equation | EQ15 | No | - |
| **Bolt Strength Properties** | | | | | | | |
| bolt_std | Bolt Standard | SAE, ASTM, or ISO/Metric | - | Dropdown | Tables 8-9, 8-10, 8-11 | No | Determines which table |
| bolt_grade | Bolt Grade | Grade/class of bolt | - | Dropdown/Table | Tables 8-9, 8-10, 8-11 | Maybe | e.g., SAE 5, ASTM A325, ISO 8.8 |
| S<sub>p</sub> | Proof Strength | Minimum proof load strength | kpsi or MPa | Table | Tables 8-9, 8-10, 8-11 | Maybe | Based on grade and size |
| S<sub>ut</sub> | Tensile Strength | Minimum ultimate tensile strength | kpsi or MPa | Table | Tables 8-9, 8-10, 8-11 | Maybe | Based on grade and size |
| S<sub>y</sub> | Yield Strength | Minimum yield strength | kpsi or MPa | Table | Tables 8-9, 8-10, 8-11 | Maybe | Based on grade and size |
| S<sub>e</sub> | Endurance Strength | Fatigue endurance limit | kpsi or MPa | Table | Table 8-17 | Maybe | Based on grade and size |
| **Preload** | | | | | | | |
| preload_type | Preload Specification | Given, Percentage, or Shigley | - | Dropdown | Page 8, step 1c | No | How Fi is determined |
| F<sub>i</sub> | Preload Force | Initial bolt tension | lbf or N | User Input/Equation | EQ16-EQ18 | No | Piecewise on preload_type |
| preload_pct | Preload Percentage | Percent of proof strength | % | User Input | Page 8, EQ17 | No | Only if preload_type = Percentage |
| connection_type | Connection Type | Permanent or Nonpermanent | - | Dropdown | Page 8, EQ18 | No | Only if preload_type = Shigley |
| **Torque** | | | | | | | |
| bolt_condition | Bolt Surface Condition | Plating/lubrication state | - | Dropdown | Page 8, K table | No | Determines K value |
| K | Torque Factor | Torque coefficient | - | Table | Page 8, K table | Maybe | Based on bolt_condition |
| T | Torque | Required tightening torque | lb-in or N-mm | Equation | EQ19 | No | - |
| **Stress and Safety Factors** | | | | | | | |
| C | Joint Stiffness Constant | Fraction of load on bolt | - | Equation | EQ20 | No | 0 < C < 1 |
| n<sub>L</sub> | Load Factor of Safety | Overload safety factor | - | Equation | EQ21 | No | Should be > 1 |
| σ<sub>b</sub> | Bolt Tensile Stress | Stress in bolt | psi or MPa | Equation | EQ22 | No | - |
| n<sub>p</sub> | Proof/Yield Factor | Yielding safety factor | - | Equation | EQ23 | No | Should be > 1 |
| n<sub>0</sub> | Joint Separation Factor | Safety against separation | - | Equation | EQ24 | No | Should be > 1 |
| **Fatigue Analysis** | | | | | | | |
| load_type | Load Type | Fully Reversed or Repeated | - | Dropdown | Page 9-10 | No | Affects σa, σm equations |
| P<sub>max</sub> | Maximum Load | Peak load per bolt | lbf or N | User Input | Page 9, EQ25 | No | Only if load_type = Fully Reversed |
| P<sub>min</sub> | Minimum Load | Minimum load per bolt | lbf or N | User Input | Page 9, EQ26 | No | Only if load_type = Fully Reversed |
| σ<sub>a</sub> | Alternating Stress | Amplitude of stress cycle | psi or MPa | Equation | EQ25 or EQ28 | No | Piecewise on load_type |
| σ<sub>m</sub> | Midrange Stress | Mean stress in cycle | psi or MPa | Equation | EQ26 or EQ29 | No | Piecewise on load_type |
| σ<sub>i</sub> | Initial Stress | Stress from preload | psi or MPa | Equation | EQ27 | No | - |
| n<sub>f</sub> | Fatigue Factor of Safety | Goodman criteria FOS | - | Equation | EQ30 or EQ31 | No | Piecewise on load_type |
| S<sub>a</sub> | Alternating Strength (plot) | Strength amplitude for diagram | psi or MPa | Equation | EQ32-EQ33 | No | For fatigue diagram |
| S<sub>m</sub> | Midrange Strength (plot) | Mean strength for diagram | psi or MPa | Equation | - | No | For fatigue diagram |

**Notes:**
- Many table lookups (At, Sp, Sut, Sy, Se, K) are marked "Maybe" for future interactivity - initially yellow manual inputs
- Piecewise equations are extensive in this calculator (config, units, preload type, load type)
- Unit conversions: mm² to m² (×10⁶), kpsi to psi (×1000), inches to 1/4" increments
- Some variables only apply in certain configurations (conditional visibility)

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| **Design Selection - Bolt Length** | | | | | |
| EQ1 | `l = t_1 + t_2` (if config = a) | Grip length for figure (a) | t₁, t₂ | l | Piecewise: config (a) |
| EQ2 | `l = \begin{cases} h + \frac{t_2}{2} & t_2 < d \\ h + \frac{d}{2} & t_2 \geq d \end{cases}` | Grip length for figure (b) | h, t₂, d | l | Piecewise: config (b) and t₂ vs d |
| EQ3 | `L > l + H` | Bolt length for figure (a) | l, H | L | Round up to nearest 1/4" |
| EQ4 | `L > h + 1.5d` | Bolt length for figure (b) | h, d | L | Round up to nearest 1/4" |
| **Design Selection - Bolt Stiffness** | | | | | |
| EQ5 | `L_T = \begin{cases} 2d + \frac{1}{4} & L \leq 6 \\ 2d + \frac{1}{2} & L > 6 \end{cases}` | Threaded length (inches) | L, d | LT | Piecewise: unit_system = inches |
| EQ6 | `L_T = \begin{cases} 2d + 6 & L \leq 125, d \leq 48 \\ 2d + 12 & 125 < L \leq 200 \\ 2d + 25 & L > 200 \end{cases}` | Threaded length (mm) | L, d | LT | Piecewise: unit_system = mm |
| EQ7 | `l_d = L - L_T` | Unthreaded length in grip | L, LT | ld | - |
| EQ8 | `l_t = l - l_d` | Threaded length in grip | l, ld | lt | - |
| EQ9 | `A_d = \frac{\pi d^2}{4}` | Unthreaded area | d | Ad | - |
| EQ10 | `k_b = \frac{A_d A_t E_b}{A_d l_t + A_t l_d}` | Bolt stiffness | Ad, At, Eb, ld, lt | kb | - |
| **Design Selection - Member Stiffness** | | | | | |
| EQ11 | `k = \frac{0.5774 \pi E_m d}{\ln\left[\frac{(1.155t + D - d)(D + d)}{(1.155t + D + d)(D - d)}\right]}` | Member section stiffness | Em, d, D, t | ki | Calculate for each section |
| EQ12 | `\frac{1}{k_{top}} = \frac{1}{k_1} + \cdots + \frac{1}{k_n}` | Top half stiffness | k₁, k₂, ... | ktop | Series springs |
| EQ13 | `\frac{1}{k_t} = \frac{1}{k_1} + \cdots + \frac{1}{k_n}` | Total member stiffness | k₁, k₂, ... | kt | All sections |
| EQ14 | `k_t = \frac{k_{top}}{2}` | Total if symmetric | ktop | kt | Only if symmetric |
| **Preload and Torque** | | | | | |
| EQ15 | `P = \frac{P_{tot}}{N}` | Load per bolt | Ptot, N | P | - |
| EQ16 | `F_i = F_i` | Preload if given | - | Fi | preload_type = Given |
| EQ17 | `F_i = \frac{X}{100} A_t S_p` | Preload from percentage | preload_pct, At, Sp | Fi | preload_type = Percentage |
| EQ18a | `F_i = 0.75 A_t S_p` | Nonpermanent preload | At, Sp | Fi | preload_type = Shigley, connection_type = Nonpermanent |
| EQ18b | `F_i = 0.90 A_t S_p` | Permanent preload | At, Sp | Fi | preload_type = Shigley, connection_type = Permanent |
| EQ19 | `T = K F_i d` | Required torque | K, Fi, d | T | - |
| **Safety Factors** | | | | | |
| EQ20 | `C = \frac{k_b}{k_b + k_m}` | Joint stiffness constant | kb, km | C | km = kt |
| EQ21 | `n_L = \frac{S_p A_t - F_i}{C P}` | Load factor of safety | Sp, At, Fi, C, P | nL | - |
| EQ22 | `\sigma_b = \frac{F_b}{A_t} = \frac{C P + F_i}{A_t}` | Bolt tensile stress | C, P, Fi, At | σb | - |
| EQ23 | `n_p = \frac{S_p}{\sigma_b} = \frac{S_p A_t}{C P + F_i}` | Proof/yield factor | Sp, σb, At, C, P, Fi | np | - |
| EQ24 | `n_0 = \frac{F_i}{P(1 - C)}` | Joint separation factor | Fi, P, C | n0 | - |
| **Fatigue Analysis - Fully Reversed** | | | | | |
| EQ25 | `\sigma_a = \frac{C(P_{max} - P_{min})}{2 A_t}` | Alternating stress | C, Pmax, Pmin, At | σa | load_type = Fully Reversed |
| EQ26 | `\sigma_m = \frac{C(P_{max} + P_{min})}{2 A_t} + \frac{F_i}{A_t}` | Midrange stress | C, Pmax, Pmin, At, Fi | σm | load_type = Fully Reversed |
| EQ27 | `\sigma_i = \frac{F_i}{A_t}` | Initial stress | Fi, At | σi | - |
| EQ30 | `n_f = \frac{S_e(S_{ut} - \sigma_i)}{S_{ut} \sigma_a + S_e(\sigma_m - \sigma_i)}` | Fatigue FOS | Se, Sut, σi, σa, σm | nf | load_type = Fully Reversed |
| **Fatigue Analysis - Repeated Load** | | | | | |
| EQ28 | `\sigma_a = \frac{C P}{2 A_t}` | Alternating stress (0 to P) | C, P, At | σa | load_type = Repeated |
| EQ29 | `\sigma_m = \frac{C P}{2 A_t} + \frac{F_i}{A_t} = \sigma_a + \sigma_i` | Midrange stress (0 to P) | C, P, At, Fi, σa, σi | σm | load_type = Repeated |
| EQ31 | `n_f = \frac{S_e(S_{ut} - \sigma_i)}{\sigma_a(S_{ut} + S_e)}` | Fatigue FOS (repeated) | Se, Sut, σi, σa | nf | load_type = Repeated |
| **Fatigue Diagram** | | | | | |
| EQ32 | `S_a = \frac{\sigma_a}{\sigma_m - \sigma_i}(S_m - \sigma_i)` | Load line | σa, σm, σi, Sm | Sa | For plotting |
| EQ33 | `S_a = S_e - \frac{S_e}{S_{ut}} S_m` | Goodman line | Se, Sut, Sm | Sa | For plotting |

**Notes:**
- Piecewise equations: EQ1/EQ2 (config), EQ5/EQ6 (units), EQ16/EQ17/EQ18a/EQ18b (preload type), EQ25-26-30 vs EQ28-29-31 (load type), EQ13 vs EQ14 (symmetry)
- Round L to nearest 1/4" (0.25 in) in EQ3/EQ4
- Note conversion: Table 8-17 specifies endurance in kpsi, may need × 1000 for psi
- Member stiffness (EQ11-13) requires iterative calculation for multiple sections

---

## 4. Procedure Steps

### 5.4.1 Design Selection

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **1** | **Determine suitable bolt length** | | | |
| 1a | Select bolt configuration | Dropdown | Choose figure (a) or (b) | Sets config variable |
| 1a-i | Input dimensions for config (a) | User Input | Enter t₁, t₂, H | Only if config = (a) |
| 1a-ii | Input dimensions for config (b) | User Input | Enter h, t₂ | Only if config = (b) |
| 1a-iii | Input nominal bolt diameter | User Input | Enter d | Required for all |
| 1b-i | Compute grip length | Equation | Use EQ1 or EQ2 | Piecewise on config |
| 1b-ii | Compute bolt length | Equation | Use EQ3 or EQ4, round up to 1/4" | Piecewise on config |
| **2** | **Compute bolt stiffness kb** | | | |
| 2a | Select unit system | Dropdown | Inches or mm | Sets unit_system |
| 2a-i | Determine threaded length LT | Equation | Use EQ5 or EQ6 | Piecewise on units and L |
| 2b | Compute unthreaded in grip ld | Equation | Use EQ7 | - |
| 2c | Compute threaded in grip lt | Equation | Use EQ8 | - |
| 2d | Compute unthreaded area Ad | Equation | Use EQ9 | - |
| 2e | Find tensile stress area At | Table Lookup | Table 8-1 (metric) or 8-2 (UNC/UNF) | Yellow input, based on d and thread type |
| 2f | Input bolt Young's modulus | User Input | Enter Eb (typically 30e6 psi for steel) | - |
| 2g | Compute bolt stiffness kb | Equation | Use EQ10 | - |
| **3** | **Determine member stiffness km** | | | |
| 3a | Create frustum diagram | Diagram/Guide | 30° angle, starts at 1.5d, converge/diverge | Visual aid (not calculated) |
| 3b-i | Identify member sections | User Input | List all sections (washers, plates) | Count and name |
| 3b-ii | For each section, input properties | User Input | Enter dᵢₙₙₑᵣ, D, t, E for each | Multiple iterations |
| 3b-iii | For each section, compute ki | Equation | Use EQ11 for each section | Multiple calculations |
| 3c | Check if symmetric | Dropdown | Top/bottom same? | Yes or No |
| 3c-i | If symmetric, compute ktop | Equation | Use EQ12, then EQ14 for kt | Only if symmetric |
| 3c-ii | If not symmetric, compute kt | Equation | Use EQ13 for all sections | If not symmetric |
| 3d | Set km = kt | Equation | Alias for clarity | - |

### 5.4.2 Factor of Safety and Preload Calculations

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| **1** | **Determine torque to reach preload** | | | |
| 1a | Input total load and bolt count | User Input | Enter Ptot, N | - |
| 1a-i | Calculate load per bolt P | Equation | Use EQ15 | - |
| 1b-i | Select bolt standard | Dropdown | SAE, ASTM, or ISO/Metric | Sets bolt_std |
| 1b-ii | Select bolt grade/class | Dropdown/Table | Based on standard | Sets bolt_grade |
| 1b-iii | Find Sp from table | Table Lookup | Table 8-9, 8-10, or 8-11 | Yellow input, based on std, grade, size |
| 1b-iv | Find Sut from table | Table Lookup | Table 8-9, 8-10, or 8-11 | Yellow input, same table |
| 1b-v | Find Sy from table | Table Lookup | Table 8-9, 8-10, or 8-11 | Yellow input, same table |
| 1b-vi | Find Se from table | Table Lookup | Table 8-17 | Yellow input, based on grade and size |
| 1c | Select preload specification method | Dropdown | Given, Percentage, or Shigley | Sets preload_type |
| 1c-i | If Given, input Fi | User Input | Enter Fi directly | Only if preload_type = Given |
| 1c-ii | If Percentage, input % and compute | User Input + Equation | Enter %, use EQ17 | Only if preload_type = Percentage |
| 1c-iii | If Shigley, select connection type | Dropdown | Permanent or Nonpermanent | Only if preload_type = Shigley |
| 1c-iv | If Shigley, compute Fi | Equation | Use EQ18a or EQ18b | Based on connection_type |
| 1d | Select bolt surface condition | Dropdown | Nonplated, Zinc, Lubricated, etc. | Sets bolt_condition |
| 1d-i | Find torque factor K | Table Lookup | K table on page 8 | Yellow input, based on bolt_condition |
| 1e | Compute required torque T | Equation | Use EQ19 | Note: 1000 lb-in is "VERY high" |
| **2** | **Compute overload/load factor nL** | | | |
| 2a | Compute joint stiffness constant C | Equation | Use EQ20 | - |
| 2b | Compute load factor of safety nL | Equation | Use EQ21 | Should be > 1 |
| **3** | **Compute bolt yield factor np** | | | |
| 3a | Compute bolt tensile stress σb | Equation | Use EQ22 | - |
| 3b | Compute proof/yield factor np | Equation | Use EQ23 | Should be > 1 |
| **4** | **Compute joint separation factor n0** | | | |
| 4a | Compute separation factor n0 | Equation | Use EQ24 | Should be > 1 |
| **5** | **Compute Goodman fatigue factor nf** | | | |
| 5a | Select load type | Dropdown | Fully Reversed or Repeated | Sets load_type |
| 5a-i | If Fully Reversed, input Pmax, Pmin | User Input | Enter Pmax, Pmin | Only if Fully Reversed |
| 5a-ii | Compute alternating stress σa | Equation | Use EQ25 (Fully Reversed) or EQ28 (Repeated) | Piecewise on load_type |
| 5b | Compute midrange stress σm | Equation | Use EQ26 (Fully Reversed) or EQ29 (Repeated) | Piecewise on load_type |
| 5c | Compute initial stress σi | Equation | Use EQ27 | - |
| 5d | Compute fatigue factor of safety nf | Equation | Use EQ30 (Fully Reversed) or EQ31 (Repeated) | Piecewise on load_type |
| 5e | (Optional) Plot fatigue diagram | Graph | Use EQ32 (load line) and EQ33 (Goodman line) | Visual aid |
| **6** | **Adjust design if needed** | | | |
| 6a | Review all safety factors | Check | nL, np, n0, nf all > 1? | Display warnings if < 1 |
| 6b | If low, increase grade/size/quantity | Iteration | Modify bolt_grade, d, or N | User decision |

**Notes:**
- Procedure has 2 main sections: Design Selection (steps 1-3) and Safety Analysis (steps 1-6)
- Many conditional steps based on dropdowns (config, preload_type, load_type, symmetry)
- Table lookups are critical and marked as yellow inputs
- Frustum diagram (step 3a) is for user understanding, not calculated
- Multiple iterations possible in step 3b (one per member section)
- Final step 6 is iterative design improvement

---

## 5. Additional Notes

### Piecewise Equations

**Grip Length (l):**
- Config (a): Simple sum of plate thicknesses
- Config (b): Depends on whether t₂ < d or t₂ ≥ d

**Bolt Length (L):**
- Config (a): l + H + margin
- Config (b): h + 1.5d + margin
- Always round UP to nearest 1/4 inch (0.25 in)

**Threaded Length (LT):**
- Inches: 2d + 1/4" (if L ≤ 6") or 2d + 1/2" (if L > 6")
- Millimeters: 2d + 6mm (if L ≤ 125mm and d ≤ 48mm), 2d + 12mm (if 125 < L ≤ 200mm), or 2d + 25mm (if L > 200mm)

**Preload Force (Fi):**
- Given: User enters directly
- Percentage: Fi = (X/100) × At × Sp
- Shigley Nonpermanent: Fi = 0.75 × At × Sp
- Shigley Permanent: Fi = 0.90 × At × Sp

**Fatigue Stresses:**
- Fully Reversed: Uses Pmax and Pmin in EQ25, EQ26, EQ30
- Repeated (0 to P): Uses simplified EQ28, EQ29, EQ31

**Member Stiffness:**
- Symmetric: Calculate top half, then kt = ktop / 2
- Not symmetric: Calculate all sections, combine with EQ13

### Validation Rules

**Integer Requirements:**
- N (number of bolts) must be integer ≥ 1

**Positive Values:**
- All dimensions (d, l, L, t, h, H) > 0
- All loads (P, Ptot, Pmax, Pmin) ≥ 0
- All stiffnesses (kb, km, ki) > 0
- For Fully Reversed: Pmax > Pmin ≥ 0

**Rounding:**
- L must be rounded UP to nearest 1/4" (0.25 in) or equivalent in mm

**Safety Factor Warnings:**
- nL < 1: Warning "Overload risk - bolt may fail"
- np < 1: Warning "Yield risk - bolt will plastically deform"
- n0 < 1: Warning "Joint separation risk - members will separate"
- nf < 1: Warning "Fatigue failure risk - bolt will fail over time"

**Torque Warning:**
- T > 1000 lb-in: Warning "Torque is VERY high for manual wrench"

**Range Checks:**
- 0 < C < 1 (stiffness constant should be fractional)
- At should match d and thread type from tables

### Standard Values / Constants

**Material Properties:**
- Steel Eb = 30e6 psi = 207 GPa
- Aluminum Eb = 10e6 psi = 69 GPa
- Cast Iron Eb = 14.5e6 psi = 100 GPa

**Typical Preloads:**
- Nonpermanent: 75% of proof strength
- Permanent: 90% of proof strength

**Torque Factors (K):**
- Nonplated, black finish: 0.30
- Zinc-plated: 0.20
- Lubricated: 0.18
- Cadmium-plated: 0.16
- With Bowman Anti-Seize: 0.12
- With Bowman-Grip nuts: 0.09

### Dropdown Options

**config (Bolt Arrangement):**
- (a) - Through bolt with nut
- (b) - Countersunk/recessed bolt

**unit_system (Unit System):**
- inches - U.S. customary units
- mm - Metric units

**bolt_std (Bolt Standard):**
- SAE - Society of Automotive Engineers (Table 8-9)
- ASTM - American Society for Testing and Materials (Table 8-10)
- ISO/Metric - International Organization for Standardization (Table 8-11)

**bolt_grade (depends on bolt_std):**
- SAE: 1, 2, 4, 5, 5.2, 7, 8, 8.2
- ASTM: A307, A325 type 1/2/3, A354 grade BC/BD, A449, A490 type 1/3
- ISO: 4.6, 4.8, 5.8, 8.8, 9.8, 10.9, 12.9

**preload_type (Preload Specification):**
- Given - User specifies Fi directly
- Percentage - Fi as % of proof strength
- Shigley - Use Shigley's recommendation

**connection_type (only if preload_type = Shigley):**
- Nonpermanent - Reused fasteners (75% of proof)
- Permanent - Permanent assembly (90% of proof)

**bolt_condition (Bolt Surface Condition):**
- Nonplated, black finish
- Zinc-plated
- Lubricated
- Cadmium-plated
- With Bowman Anti-Seize
- With Bowman-Grip nuts

**load_type (Load Type for Fatigue):**
- Fully Reversed - Load varies from Pmin to Pmax
- Repeated - Load varies from 0 to P

**symmetry (for member stiffness):**
- Yes - Top and bottom halves identical
- No - Calculate all sections separately

### Future Enhancement Opportunities

**Interactive Table Lookups:**
- At from Table 8-1 or 8-2 (could auto-fill based on d and thread type)
- Sp, Sut, Sy from Tables 8-9, 8-10, 8-11 (could auto-fill based on standard, grade, and size range)
- Se from Table 8-17 (could auto-fill based on grade and size range)
- K from torque factor table (could auto-fill based on bolt_condition)

**Frustum Diagram Generator:**
- Auto-generate the 30° frustum diagram based on bolt arrangement
- Calculate D values for each section automatically
- Suggest section divisions

**Graph Plotting:**
- Auto-plot fatigue failure diagram with load line and Goodman line
- Show operating point and safety margin visually

**Multi-Section Member Entry:**
- Dynamic form to add/remove member sections
- Automatic calculation of all ki values and series combination

### Special Considerations

**Table Usage:**
- 5 tables requiring manual lookup (8-1, 8-2, 8-9/8-10/8-11, 8-17, K table)
- Tables span multiple pages in PDF - provide page references in info boxes
- Metric vs. UNC/UNF threads require different tables for At

**Unit Conversions:**
- mm² to m² for SI calculations: multiply by 10⁶
- kpsi to psi: multiply by 1000
- inches to mm: multiply by 25.4
- lb to N: multiply by 4.448
- lb-in to N-mm: multiply by 113

**Conditional Visibility:**
- h only visible if config = (b)
- t₁, H only visible if config = (a)
- preload_pct only visible if preload_type = Percentage
- connection_type only visible if preload_type = Shigley
- Pmax, Pmin only visible if load_type = Fully Reversed

**Iterative Calculations:**
- Member stiffness (step 3) requires user to calculate ki for each section, then combine
- If safety factors are insufficient, user must iterate by changing bolt properties (step 6)

**Fatigue Diagram Notes:**
- X-axis: Steady stress σm (or Sm)
- Y-axis: Alternating stress σa (or Sa)
- Load line: Passes through (σi, 0) with slope σa/(σm - σi)
- Goodman line: Passes through (0, Se) and (Sut, 0)
- Intersection point B represents actual stress state
- Goodman line intersection C represents failure

### Calculation Order / Dependencies

```
User Inputs: config, d, t1/t2/h/H →
  EQ1/EQ2: l →
  EQ3/EQ4: L →

unit_system, L, d →
  EQ5/EQ6: LT →
  EQ7: ld →
  EQ8: lt →

d →
  EQ9: Ad →

Table 8-1/8-2 (d, thread type) → At (manual lookup)

Eb, Ad, At, ld, lt →
  EQ10: kb →

User Inputs: Em, member sections (d, D, t for each) →
  EQ11: ki for each section →
  EQ12/EQ13/EQ14: kt = km →

Ptot, N →
  EQ15: P →

Tables 8-9/8-10/8-11 (standard, grade, size) → Sp, Sut, Sy (manual lookup)
Table 8-17 (grade, size) → Se (manual lookup)

preload_type, [Fi or preload_pct or connection_type], At, Sp →
  EQ16/EQ17/EQ18a/EQ18b: Fi →

bolt_condition → K (manual lookup from table)

K, Fi, d →
  EQ19: T →

kb, km →
  EQ20: C →

Sp, At, Fi, C, P →
  EQ21: nL →

C, P, Fi, At →
  EQ22: σb →

Sp, σb →
  EQ23: np →

Fi, P, C →
  EQ24: n0 →

load_type, [P or Pmax/Pmin], C, At, Fi →
  EQ25/EQ28: σa →
  EQ26/EQ29: σm →
  EQ27: σi →

Se, Sut, σi, σa, σm →
  EQ30/EQ31: nf →

(Optional) σa, σm, σi, Se, Sut →
  EQ32: Load line Sa →
  EQ33: Goodman line Sa →
  Plot fatigue diagram
```

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from PDF listed and categorized (50+ variables)
- [x] Source references noted for all table/graph variables (5 tables)
- [x] All equations extracted and converted to LaTeX (33 equations)
- [x] Procedure steps documented (2 procedures with 30+ steps total)
- [x] Piecewise equations explained (6 piecewise equation sets)
- [x] Validation rules documented (safety factors, rounding, ranges)
- [x] Future enhancements noted (interactive tables, diagram generator, plotting)
- [x] Tab structure decision justified (3 tabs for complex procedure + many equations)
- [x] Specification reviewed against PDF (pages 1-11 covered)
- [x] Consistent naming and formatting throughout

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [ ] Ready for Implementation

**Specification Author:** Claude Code
**Date Created:** 2025-01-09
**Last Updated:** 2025-01-09

**Implementation Notes for HTML Calculator:**
- This is a very complex calculator - consider splitting into two pages (Design Selection vs. Safety Analysis) or use clear section headers
- Extensive conditional visibility required for dropdown-dependent inputs
- 5 tables require yellow input boxes with clear info boxes directing to specific pages/tables
- Member stiffness section (Design Selection step 3) may need dynamic form for adding multiple sections
- Consider providing a "Quick Start" mode vs. "Full Analysis" mode
- Fatigue diagram plotting is optional but adds value
- All 4 safety factors should be prominently displayed with color-coding (green if >1, red if <1)

---

## Template Version

Template Version: 1.0
Specification Version: 1.0
