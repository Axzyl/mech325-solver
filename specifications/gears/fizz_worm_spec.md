# Calculator Specification: Worm Gear Design (Fizz Guide)

---

## 1. Metadata

- **Calculator Name:** Worm Gear Design Calculator (Fizz Guide)
- **Source PDF:** fizz_worm.pdf
- **PDF Location:** `documents/gears/fizz_worm.pdf`
- **Description:** Calculates worm gear drive parameters using a 21-step comprehensive methodology, including velocity ratio, pitch diameters, lead angles, efficiency, forces, stresses, wear rating, and thermal analysis.
- **Complexity:** High (extensive calculations, friction analysis, thermal modeling, multi-factor stress analysis)
- **Tabs to Include:**
  - [x] Procedure (21-step process)
  - [x] General Equations (key equations reference)
  - [x] Variables (30+ variables)

**Rationale for Tab Structure:**
- **Procedure Tab**: PDF has clear 21-step sequential procedure for worm gear design
- **General Equations Tab**: Contains key worm gear equations for reference
- **Variables Tab**: 30+ variables organized by category require central management

---

## 2. Variables

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| **User Inputs** |
| P<sub>out</sub> | Output Power | Power delivered by gear | hp | User Input | - | No | Given parameter |
| n<sub>w</sub> | Worm Speed | Rotational speed of worm | rpm | User Input | - | No | Input speed |
| n<sub>g</sub> | Gear Speed | Rotational speed of gear | rpm | User Input | - | No | Output speed |
| N<sub>w</sub> | Number of Worm Threads | Thread count on worm | threads | User Input | - | No | Typically 1-4 |
| N<sub>g</sub> | Number of Gear Teeth | Tooth count on gear | teeth | User Input | - | No | Positive integer |
| d | Worm Pitch Diameter | Pitch diameter of worm | in | User Input | - | No | Design choice |
| C | Center Distance | Distance between centers | in | User Input | - | No | Design constraint |
| L | Lead | Axial advance per revolution | in | User Input | - | No | Standard value |
| F | Face Width | Width of gear face | in | User Input | - | No | Typically ≤0.67d |
| φ<sub>n</sub> | Normal Pressure Angle | Normal pressure angle | deg | User Input | - | No | Typically 20° |
| C<sub>s</sub> | Service Factor | Service/application factor | - | User Input | - | No | Typically 1.0 |
| t<sub>g</sub> | Max Gear Temperature | Maximum operating temperature | °F | User Input | - | No | Typically 180°F |
| t<sub>a</sub> | Ambient Temperature | Environmental temperature | °F | User Input | - | No | Typically 70°F |
| **Manual Lookups** |
| μ | Coefficient of Friction | Friction coefficient | - | Chart/Graph | Friction charts | Maybe | Based on V<sub>w</sub> and materials |
| y | Lewis Form Factor | Tooth form factor | - | Table | Lewis factor table | Maybe | Based on N<sub>g</sub> |
| C<sub>m</sub> | Materials Factor | Material strength factor | - | Table | Material tables | Maybe | Material dependent |
| C<sub>v</sub> | Velocity Factor | Speed correction factor | - | Table | Velocity tables | Maybe | Velocity dependent |
| K | Wear Factor | Wear resistance factor | - | Table | Wear factor table | Maybe | Material combination |
| h | Heat Transfer Coefficient | Thermal transfer rate | BTU/min·ft²·°F | Table/Graph | Heat transfer charts | Maybe | Depends on cooling |
| A | Surface Area | Heat dissipation area | ft² | Calculation/Geometry | - | No | Housing geometry |
| **Auto-Calculated** |
| m<sub>G</sub> | Velocity Ratio | Gear ratio | - | Equation | EQ1 | No | n<sub>w</sub>/n<sub>g</sub> |
| λ | Lead Angle | Helix angle of worm | deg | Equation | EQ2 | No | arctan(L/(πd)) |
| p<sub>x</sub> | Axial Pitch | Pitch along worm axis | in | Equation | EQ3 | No | πd/tan(λ) |
| D | Gear Pitch Diameter | Pitch diameter of gear | in | Equation | EQ4 | No | 2C - d |
| V<sub>w</sub> | Worm Velocity | Pitch line velocity of worm | ft/min | Equation | EQ5 | No | πdn<sub>w</sub>/12 |
| η | Efficiency | Mechanical efficiency | - | Equation | EQ6 | No | Function of φ<sub>n</sub>, μ, λ |
| P<sub>in</sub> | Input Power | Power required at worm | hp | Equation | EQ7 | No | P<sub>out</sub>/η |
| V<sub>g</sub> | Gear Velocity | Pitch line velocity of gear | ft/min | Equation | EQ8 | No | πDn<sub>g</sub>/12 |
| W<sub>t</sub> | Tangential Force | Tangential force on gear | lb | Equation | EQ9 | No | 33000P<sub>out</sub>/V<sub>g</sub> |
| W<sub>tw</sub> | Worm Tangential Force | Tangential force on worm | lb | Equation | EQ10 | No | Complex friction formula |
| W<sub>rw</sub> | Worm Radial Force | Radial force on worm | lb | Equation | EQ11 | No | W<sub>t</sub>tan(φ<sub>n</sub>) |
| P<sub>d</sub> | Diametral Pitch | Diametral pitch of gear | teeth/in | Equation | EQ12 | No | N<sub>g</sub>/D |
| σ<sub>t</sub> | Bending Stress | Tooth bending stress | psi | Equation | EQ13 | No | W<sub>t</sub>P<sub>d</sub>/(Fy) |
| σ<sub>all</sub> | Allowable Stress | Allowable bending stress | psi | Equation | EQ14 | No | 1000C<sub>s</sub>C<sub>m</sub>C<sub>v</sub> |
| FS | Factor of Safety | Stress safety factor | - | Equation | EQ15 | No | σ<sub>all</sub>/σ<sub>t</sub> |
| W<sub>wear</sub> | Wear Load Rating | Wear capacity | lb | Equation | EQ16 | No | DFK |
| P<sub>thermal</sub> | Thermal Power Capacity | Heat dissipation capacity | hp | Equation | EQ17 | No | hA(t<sub>g</sub>-t<sub>a</sub>)/33000 |
| P<sub>loss</sub> | Power Loss | Heat generated | hp | Equation | EQ18 | No | P<sub>in</sub>(1-η) |
| TM | Thermal Margin | Thermal safety margin | - | Equation | EQ19 | No | P<sub>thermal</sub>/P<sub>loss</sub> |

---

## 3. Equations

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `m_G = \frac{n_w}{n_g}` | Velocity Ratio | n<sub>w</sub>, n<sub>g</sub> | m<sub>G</sub> | Should equal N<sub>g</sub>/N<sub>w</sub> |
| EQ2 | `\lambda = \arctan\left(\frac{L}{\pi d}\right)` | Lead Angle | L, d | λ | Result in degrees |
| EQ3 | `p_x = \frac{\pi d}{\tan(\lambda)}` | Axial Pitch | d, λ | p<sub>x</sub> | Related to L by L=N<sub>w</sub>p<sub>x</sub> |
| EQ4 | `D = 2C - d` | Gear Pitch Diameter | C, d | D | Center distance relation |
| EQ5 | `V_w = \frac{\pi d n_w}{12}` | Worm Velocity | d, n<sub>w</sub> | V<sub>w</sub> | Convert in/min to ft/min |
| EQ6 | `\eta = \frac{\cos(\phi_n) - \mu\tan(\lambda)}{\cos(\phi_n) + \mu\cot(\lambda)}` | Efficiency | φ<sub>n</sub>, μ, λ | η | Friction efficiency |
| EQ7 | `P_{in} = \frac{P_{out}}{\eta}` | Input Power | P<sub>out</sub>, η | P<sub>in</sub> | Accounts for losses |
| EQ8 | `V_g = \frac{\pi D n_g}{12}` | Gear Velocity | D, n<sub>g</sub> | V<sub>g</sub> | Convert in/min to ft/min |
| EQ9 | `W_t = \frac{33000 \times P_{out}}{V_g}` | Tangential Force on Gear | P<sub>out</sub>, V<sub>g</sub> | W<sub>t</sub> | Power-force relation |
| EQ10 | `W_{tw} = \frac{W_t(\cos(\phi_n)\tan(\lambda) + \mu)}{\cos(\phi_n) - \mu\tan(\lambda)}` | Worm Tangential Force | W<sub>t</sub>, φ<sub>n</sub>, λ, μ | W<sub>tw</sub> | Friction-based force |
| EQ11 | `W_{rw} = W_t \tan(\phi_n)` | Worm Radial Force | W<sub>t</sub>, φ<sub>n</sub> | W<sub>rw</sub> | Radial component |
| EQ12 | `P_d = \frac{N_g}{D}` | Diametral Pitch | N<sub>g</sub>, D | P<sub>d</sub> | Teeth per inch |
| EQ13 | `\sigma_t = \frac{W_t P_d}{F y}` | Bending Stress | W<sub>t</sub>, P<sub>d</sub>, F, y | σ<sub>t</sub> | Lewis equation |
| EQ14 | `\sigma_{all} = 1000 \times C_s \times C_m \times C_v` | Allowable Stress | C<sub>s</sub>, C<sub>m</sub>, C<sub>v</sub> | σ<sub>all</sub> | Material strength |
| EQ15 | `FS = \frac{\sigma_{all}}{\sigma_t}` | Factor of Safety | σ<sub>all</sub>, σ<sub>t</sub> | FS | Should be > 1.5 |
| EQ16 | `W_{wear} = D \times F \times K` | Wear Load Rating | D, F, K | W<sub>wear</sub> | Wear capacity |
| EQ17 | `P_{thermal} = \frac{h \times A \times (t_g - t_a)}{33000}` | Thermal Power Capacity | h, A, t<sub>g</sub>, t<sub>a</sub> | P<sub>thermal</sub> | Heat dissipation |
| EQ18 | `P_{loss} = P_{in} \times (1 - \eta)` | Power Loss | P<sub>in</sub>, η | P<sub>loss</sub> | Heat generated |
| EQ19 | `TM = \frac{P_{thermal}}{P_{loss}}` | Thermal Margin | P<sub>thermal</sub>, P<sub>loss</sub> | TM | Should be > 1.0 |

---

## 4. Procedure Steps

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 0 | Given Information | User Input | Enter P<sub>out</sub>, n<sub>w</sub>, n<sub>g</sub>, N<sub>w</sub>, N<sub>g</sub> | Initial parameters |
| 1 | Calculate Velocity Ratio | Equation | m<sub>G</sub> = n<sub>w</sub>/n<sub>g</sub> (EQ1) | Verify against N<sub>g</sub>/N<sub>w</sub> |
| 2-3 | Select Pitch Diameters | User Input | Choose d (worm) and C (center distance) | Design constraints |
| 4 | Select Lead | User Input | Choose L from standard values | L = N<sub>w</sub> × p<sub>x</sub> |
| 5 | Calculate Lead Angle | Equation | λ = arctan(L/(πd)) (EQ2) | Auto-calculated |
| 6 | Calculate Gear Pitch Diameter | Equation | D = 2C - d (EQ4) | Auto-calculated |
| 7 | Calculate Worm Velocity | Equation | V<sub>w</sub> = πdn<sub>w</sub>/12 (EQ5) | Auto-calculated |
| 8 | Look Up Friction Coefficient | Chart/Graph | Find μ based on V<sub>w</sub> and material pair | Manual lookup |
| 9 | Calculate Efficiency | Equation | η using friction formula (EQ6) | Auto-calculated |
| 10 | Calculate Input Power | Equation | P<sub>in</sub> = P<sub>out</sub>/η (EQ7) | Auto-calculated |
| 11 | Calculate Tangential Force | Equation | V<sub>g</sub> then W<sub>t</sub> (EQ8, EQ9) | Auto-calculated |
| 12 | Calculate Forces on Worm | Equation | W<sub>tw</sub> and W<sub>rw</sub> (EQ10, EQ11) | Auto-calculated |
| 13 | Select Face Width | User Input | Choose F (typically ≤ 0.67d) | Design constraint |
| 14-15 | Calculate Stress | Equation | P<sub>d</sub>, then σ<sub>t</sub> (EQ12, EQ13) | Look up y from table |
| 16 | Input Strength Factors | User Input/Table | Enter C<sub>s</sub>, look up C<sub>m</sub>, C<sub>v</sub> | C<sub>s</sub> typically 1.0 |
| 17-18 | Calculate Allowable Stress | Equation | σ<sub>all</sub> then FS (EQ14, EQ15) | FS should be > 1.5 |
| 19 | Calculate Wear Load | Equation | W<sub>wear</sub> = DFK (EQ16) | Look up K from table |
| 20 | Calculate Thermal Capacity | Equation | P<sub>thermal</sub> (EQ17) | Input h, A, temperatures |
| 21 | Check Heat Dissipation | Equation | P<sub>loss</sub>, TM (EQ18, EQ19) | TM should be > 1.0 |

---

## 5. Additional Notes

### Validation Rules
- **Positive Values**: All dimensions, speeds, powers must be > 0
- **Integer Values**: N<sub>w</sub>, N<sub>g</sub> must be positive integers
- **Thread Range**: N<sub>w</sub> typically 1-4
- **Angle Range**: 0° < φ<sub>n</sub> < 90° (typically 20°)
- **Face Width**: F ≤ 0.67d (recommended)
- **Factor of Safety**: FS should be > 1.5 for reliable operation
- **Thermal Margin**: TM should be > 1.0 (add cooling if < 1.0)
- **Efficiency Range**: 0 < η < 1
- **Temperature**: t<sub>g</sub> > t<sub>a</sub>

### Standard Values / Constants
- **Normal Pressure Angle**: φ<sub>n</sub> = 20° (typical)
- **Service Factor**: C<sub>s</sub> = 1.0 (typical)
- **Max Gear Temperature**: t<sub>g</sub> = 180°F (typical)
- **Ambient Temperature**: t<sub>a</sub> = 70°F (typical)
- **Face Width Limit**: F ≤ 0.67d
- **π**: Use Math.PI = 3.14159265359

### Dropdown Options

**Not explicitly implemented in current calculator, but could add:**

**Worm Material:**
- Hardened Steel
- Bronze
- Cast Iron

**Gear Material:**
- Bronze (most common)
- Cast Iron
- Aluminum Bronze

**Lubrication Type:**
- Type A (Bath or disc)
- Type B (Forced feed)
- Type C (Forced feed with cooling)

**Service Factor (C<sub>s</sub>):**
- Light duty: 1.0
- Medium duty: 1.25
- Heavy duty: 1.5

### Future Enhancement Opportunities
- **Friction Chart**: Digitize friction vs. velocity curves for automatic μ lookup
- **Lewis Factor Table**: Implement automatic y lookup based on N<sub>g</sub>
- **Material Tables**: Auto-populate C<sub>m</sub> based on material selection
- **Velocity Factor**: Auto-calculate C<sub>v</sub> based on V<sub>w</sub>
- **Wear Factor Table**: Auto-lookup K based on material combination
- **Heat Transfer**: Auto-estimate h based on cooling method
- **Standard Lead Values**: Provide dropdown of standard lead values
- **Self-Locking Check**: Flag when lead angle is small enough for self-locking (λ < friction angle)
- **Geometry Visualization**: Show worm-gear mesh diagram
- **Material Database**: Full material property database

### Special Considerations
- **Self-Locking**: Worm drives can be self-locking when tan(λ) < μ/cos(φ<sub>n</sub>)
- **Efficiency**: Worm gears typically have lower efficiency (50-98%) compared to other gear types
- **Heat Generation**: Significant heat generation due to sliding contact requires thermal analysis
- **Friction Critical**: Friction coefficient heavily influences efficiency and forces
- **Material Pairing**: Worm typically hardened steel, gear typically bronze for wear resistance
- **Lubrication Essential**: Proper lubrication critical for friction control and heat dissipation
- **Direction of Power**: Equations assume power flows from worm to gear (reducing)
- **Thrust Loads**: Worm experiences significant axial thrust (W<sub>tw</sub>)
- **Cooling Requirements**: May need forced cooling or fins for high power applications

### Calculation Order / Dependencies

```
Given: P_out, n_w, n_g, N_w, N_g

Step 1: m_G = n_w / n_g

Steps 2-3: Choose d, C

Step 4: Choose L

Step 5: λ = arctan(L/(πd))
        p_x = πd/tan(λ)

Step 6: D = 2C - d

Step 7: V_w = πdn_w/12

Step 8: Look up μ (friction chart, based on V_w and materials)

Step 9: η = [cos(φ_n) - μtan(λ)] / [cos(φ_n) + μcot(λ)]

Step 10: P_in = P_out / η

Step 11: V_g = πDn_g/12
         W_t = 33000P_out / V_g

Step 12: W_tw = W_t[cos(φ_n)tan(λ) + μ] / [cos(φ_n) - μtan(λ)]
         W_rw = W_t tan(φ_n)

Step 13: Choose F

Steps 14-15: P_d = N_g / D
             Look up y (Lewis factor table, based on N_g)
             σ_t = W_t P_d / (F y)

Step 16: Enter C_s
         Look up C_m (material table)
         Look up C_v (velocity table)

Steps 17-18: σ_all = 1000 C_s C_m C_v
             FS = σ_all / σ_t

Step 19: Look up K (wear factor table)
         W_wear = D F K

Step 20: Enter h, A, t_g, t_a
         P_thermal = h A (t_g - t_a) / 33000

Step 21: P_loss = P_in (1 - η)
         TM = P_thermal / P_loss
```

### Variable Categories (for Variables Tab)

**Basic Parameters:**
- N<sub>w</sub> - Number of Worm Threads
- N<sub>g</sub> - Number of Gear Teeth
- P<sub>d</sub> - Diametral Pitch
- φ<sub>n</sub> - Normal Pressure Angle
- n<sub>w</sub> - Worm Speed
- n<sub>g</sub> - Gear Speed
- m<sub>G</sub> - Gear Ratio

**Geometry:**
- d - Worm Pitch Diameter
- D - Gear Pitch Diameter
- C - Center Distance
- L - Lead
- λ - Lead Angle
- p<sub>x</sub> - Axial Pitch
- A - Addendum (not used in current calculations)
- h - Whole Depth (not used in current calculations)
- F - Face Width

**Power & Torque:**
- P<sub>out</sub> - Output Power
- P<sub>in</sub> - Input Power
- P<sub>loss</sub> - Power Loss
- P<sub>thermal</sub> - Thermal Power Capacity
- η - Efficiency
- TM - Thermal Margin
- t<sub>g</sub> - Gear Torque (reused as max temperature in current code)
- t<sub>a</sub> - Axial Torque (reused as ambient temperature in current code)

**Forces:**
- W<sub>t</sub> - Tangential Force
- W<sub>tw</sub> - Worm Tangential Force
- W<sub>rw</sub> - Worm Radial Force
- W<sub>wear</sub> - Wear Load

**Velocities:**
- V<sub>w</sub> - Worm Pitch Line Speed
- V<sub>g</sub> - Gear Pitch Line Speed

**Stress & Factors:**
- σ<sub>t</sub> - Bending Stress
- σ<sub>all</sub> - Allowable Stress
- y - Lewis Form Factor
- K - Geometry Factor (Wear Factor)
- C<sub>m</sub> - Ratio Correction Factor (Materials Factor)
- C<sub>s</sub> - Material Factor (Service Factor)
- C<sub>v</sub> - Velocity Factor
- μ - Coefficient of Friction
- FS - Factor of Safety

### Implementation Notes

**Variable Name Conflicts:**
The current HTML implementation has some variable naming conflicts:
- `h` is used for heat transfer coefficient, but also listed as "Whole Depth" in variables table
- `A` is used for surface area, but also listed as "Addendum" in variables table
- `tg` is used for max gear temperature (°F), conflicting with typical torque notation
- `ta` is used for ambient temperature (°F), conflicting with typical torque notation

**Recommended Resolution:**
- Keep `h` as heat transfer coefficient (BTU/min·ft²·°F)
- Keep `A` as surface area (ft²)
- Use `t_g` for max temperature (°F)
- Use `t_a` for ambient temperature (°F)
- Add separate variables if addendum/depth calculations are needed

**Sync Groups:**
The calculator implements extensive variable synchronization across tabs:
- All instances of the same variable sync bidirectionally
- Variables with subscript notation have multiple IDs (e.g., `nw`, `var_nw`, `nw_eq1`, `nw_eq7`)
- Changes in any tab update all other tabs immediately

---

## 6. Review Checklist

- [x] Metadata section complete
- [x] All variables from HTML listed and categorized
- [x] Source references noted for all table/graph variables
- [x] All equations extracted and converted to LaTeX
- [x] Procedure steps documented (21 steps)
- [x] Piecewise equations explained (N/A for this calculator)
- [x] Validation rules documented
- [x] Future enhancements noted
- [x] Tab structure decision justified
- [x] Specification reviewed against HTML implementation
- [x] Consistent naming and formatting throughout
- [x] Variable name conflicts identified

---

## 7. Implementation Ready

**Specification Status:** [x] Complete  [x] Reviewed  [x] Ready for Reference

**Implementation Note:** This calculator has high complexity due to:
1. Extensive friction and efficiency calculations
2. Thermal analysis requirements
3. Multiple table lookups (friction, Lewis factor, material properties, wear factor)
4. Force analysis with friction coupling
5. Self-locking potential analysis

The current implementation handles most calculations automatically but requires manual lookups for:
- μ (friction coefficient from charts)
- y (Lewis form factor from tables)
- C<sub>m</sub> (materials factor from tables)
- C<sub>v</sub> (velocity factor from tables)
- K (wear factor from tables)
- h (heat transfer coefficient)
- A (surface area estimation)

**Specification Author:** Claude Code
**Date Created:** 2025-10-22
**Last Updated:** 2025-10-22

---

## Template Version

Template Version: 1.0 (based on fizz_chains_spec.md template)
Last Updated: 2025-10-22
