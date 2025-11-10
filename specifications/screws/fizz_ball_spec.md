# CALCULATOR SPECIFICATION: Ball Screw Design

**Calculator Name**: fizz_ball
**Source Document**: `documents/screws/fizz_ball.pdf`
**Pages**: 2 (pages 127-128 in original document)
**Date Created**: 2025-11-09
**Specification Version**: 1.0

---

## 1. CALCULATOR METADATA

### 1.1 Purpose
This calculator provides a complete 4-step design procedure for ball screw selection and analysis. It calculates travel life based on usage parameters, assists in selecting an appropriate ball screw from a graph, computes required driving torque, and determines power requirements for a given travel speed.

### 1.2 Scope
- **Inputs**: Load, travel distance per cycle, operating frequency, lifetime, travel speed
- **Outputs**: Travel life, ball screw selection (nominal diameter, threads per inch, lead), driving torque, required power
- **Target Users**: Mechanical engineers designing precision linear motion systems

### 1.3 Limitations
- Graph-based selection (Step 2) requires visual interpolation
- Graph shows 8 screw sizes only (nominal diameters 3/8" to 3")
- Logarithmic scales require careful reading
- Torque formula (T = 0.177FL) is simplified/empirical

### 1.4 Assumptions
- Torque coefficient: 0.177 (built into formula)
- Travel life calculation assumes continuous annual operation (365 days/year)
- Graph assumes standard load ratings for shown screw sizes

---

## 2. VARIABLES

### 2.1 Variable Definitions Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|---------------------|-------|
| **Input Variables - Usage Parameters** |
| distance_per_cycle | Distance per cycle | Travel distance per cycle | in | User Input | Step 1 | Yes | - |
| cycles_per_hour | Cycles per hour | Operating cycles per hour | cycles/hr | User Input | Step 1 | Yes | - |
| hours_per_day | Hours per day | Operating hours per day | hr/day | User Input | Step 1 | Yes | - |
| days_per_year | Days per year | Operating days per year | day/yr | User Input | Step 1 | Yes | Typically 365 |
| years_lifetime | Lifetime in years | Expected lifetime | years | User Input | Step 1 | Yes | - |
| **Calculated - Travel Life** |
| travel_life | Travel life | Total travel distance over lifetime | in | Calculated | Step 1: Formula | Yes | - |
| **Input Variables - Load** |
| F | Applied load | Load on ball screw | lbf | User Input | Step 2, 3 | Yes | - |
| **Graph Lookup Variables - Screw Selection** |
| nominal_diameter | Nominal diameter | Screw nominal diameter | in | Graph Lookup | Step 2 | Yes | Yellow input, 3/8" to 3" |
| threads_per_inch | Threads per inch | Thread count | tpi | Graph Lookup | Step 2 | Yes | Yellow input from graph table |
| L | Lead | Lead (pitch × starts) | in | Graph Lookup | Step 2, 3, 4 | Yes | Yellow input from graph table |
| **Calculated - Torque** |
| T_u | Driving torque | Torque required to drive screw | lbf·in | Calculated | Step 3: T_u = 0.177FL | Yes | - |
| **Input Variables - Speed** |
| V | Travel speed | Linear travel speed | in/s | User Input | Step 4a | Yes | - |
| **Calculated - Rotational Speed** |
| n | Rotational speed | Screw rotation speed | rpm | Calculated | Step 4a: n = V·(1/L)·60 | Yes | - |
| **Calculated - Power** |
| P | Required power | Power required to drive screw | hp | Calculated | Step 4b: P = Tn/63000 | Yes | - |

### 2.2 Variable Categories by Source Type

**User Inputs (White)**: distance_per_cycle, cycles_per_hour, hours_per_day, days_per_year, years_lifetime, F, V

**Graph Lookup (Yellow)**: nominal_diameter, threads_per_inch, L

**Calculated (Green)**: travel_life, T_u, n, P

---

## 3. EQUATIONS

### 3.1 Core Equations

**Step 1: Travel Life**
```latex
\text{Travel Life} = \left(\frac{\text{distance (in)}}{\text{cycle}}\right) \left(\frac{\# \text{ of cycles}}{\text{hour}}\right) \left(\frac{24 \text{ hours}}{\text{day}}\right) \left(\frac{365 \text{ days}}{\text{year}}\right) (\# \text{ of years})
```

Simplified:
```latex
\text{Travel Life} = \text{distance\_per\_cycle} \times \text{cycles\_per\_hour} \times \text{hours\_per\_day} \times \text{days\_per\_year} \times \text{years\_lifetime}
```

**Step 3: Driving Torque**
```latex
T_u = 0.177 F L
```
Where: F in lbf, L in inches, T_u in lbf·in

**Step 4a: Rotational Speed**
```latex
n = (V) \left(\frac{1 \text{ rev}}{L}\right) \left(\frac{60 \text{ sec}}{\text{min}}\right)
```

Simplified:
```latex
n = \frac{60V}{L}
```
Where: V in in/s, L in inches, n in rpm

**Step 4b: Required Power**
```latex
P = \frac{T n}{63000}
```
Where: T in lbf·in, n in rpm, P in hp

---

## 4. PROCEDURE STEPS

### 4.1 Ball Screw Design Procedure (4 Steps)

| Step | Action Type | Description | Inputs Required | Outputs Generated | Notes |
|------|-------------|-------------|-----------------|-------------------|-------|
| 1 | Calculate | Compute total travel life in inches | distance_per_cycle, cycles_per_hour, hours_per_day, days_per_year, years_lifetime | travel_life | Full lifetime calculation |
| 2 | Graph Lookup | Select ball screw based on load and travel life | F, travel_life | nominal_diameter, threads_per_inch, L | Yellow input, use graph on page 2 |
| 3 | Calculate | Compute driving torque | F, L | T_u | T_u = 0.177FL |
| 4a | Calculate | Calculate required rotational speed | V, L | n | n = 60V/L |
| 4b | Calculate | Compute required power | T_u, n | P | P = Tn/63000 |

---

## 5. ADDITIONAL NOTES

### 5.1 Graph Reference

**Ball Screw Selection Graph** (page 2):

**Axes:**
- X-axis: Travel life (in) - logarithmic scale from 10⁵ to 10⁹
- Y-axis: Load (lbf) - logarithmic scale from 100 to 100,000

**Screw Design Data Table (embedded in graph):**

| Nominal dia. | Threads per inch | Lead |
|--------------|------------------|------|
| 3 | 1½ | 0.667 |
| 2½ | 1 | 1.00 |
| 2 | 2 | 0.50 |
| 1½ | 2 | 0.50 |
| ¾ | 2 | 0.50 |
| ½ | 5 | 0.20 |
| ⅜ | 8 | 0.125 |

**Graph Curves:**
- 8 diagonal lines representing the 8 screw sizes
- Each line shows load capacity vs travel life
- Lines slope downward from left to right (higher loads = shorter life)
- Selection: Find intersection of calculated load and travel life, choose screw curve above that point

**Usage:**
1. Calculate travel life (Step 1)
2. Locate travel life on X-axis
3. Locate applied load F on Y-axis
4. Find intersection point
5. Select the screw curve that is above and closest to the intersection
6. Read nominal diameter, threads per inch, and lead from the table

### 5.2 Special Considerations

**Torque Formula**:
- T_u = 0.177FL is an empirical/simplified formula
- The constant 0.177 accounts for ball screw efficiency, friction, and geometry
- Much simpler than power screw torque equations (no thread angle, friction coefficient)
- Ball screws are highly efficient (~90-95%), hence low torque coefficient

**Lead vs Pitch**:
- Lead L = pitch × number of starts
- For single-start threads: L = pitch
- For multi-start threads: L > pitch
- Graph provides lead directly, not pitch

**Graph Interpolation**:
- Both axes are logarithmic
- Interpolation should be done on log scale
- May be challenging to read precisely
- Consider providing formula approximations or digitized table

**Selection Margin**:
- Choose screw curve **above** the calculated point for safety margin
- Do not select screw curve below the point (insufficient capacity)
- Closest curve above provides most economical design

**Power Calculation Context**:
- Step 4 determines motor power requirements
- Useful for selecting drive motor
- Linear speed V would typically come from application requirements
- Power scales linearly with both load and speed

**Comparison to Power Screws**:
- Ball screws: Much higher efficiency (~90% vs ~25-60% for power screws)
- Ball screws: Lower friction, higher speeds possible
- Ball screws: Higher cost, more precise
- Ball screws: Not self-locking (requires brake if needed)
- Torque formula much simpler (0.177FL vs complex friction equations)

---

## 6. IMPLEMENTATION READY CHECKLIST

### 6.1 Data Requirements
- [x] All equations documented in LaTeX format
- [x] All variables defined with units
- [x] Input/output relationships mapped
- [x] Graph lookup requirements identified
- [x] Screw data table documented

### 6.2 UI/UX Requirements
- [ ] Three-tier input system (white/yellow/green) implemented
- [ ] MathJax rendering for all equations
- [ ] PDF viewer with page navigation (2 pages)
- [ ] Session save/load/reset functionality
- [ ] Graph visualization or interactive tool
- [ ] Screw data table for selection
- [ ] Visual indication of selected screw on graph
- [ ] Logarithmic scale handling for graph

### 6.3 Calculation Logic
- [ ] Travel life calculation (multi-factor multiplication)
- [ ] Graph interpolation logic or table lookup
- [ ] Torque calculation (simple multiplication)
- [ ] Rotational speed conversion
- [ ] Power calculation
- [ ] Logarithmic scale conversions if graph displayed

### 6.4 Validation Requirements
- [ ] Constraint checks:
  - All usage parameters > 0
  - Load F > 0
  - Travel speed V > 0
  - Lead L > 0
- [ ] Graph bounds checking (10⁵ ≤ travel_life ≤ 10⁹)
- [ ] Load bounds checking (100 ≤ F ≤ 100,000)
- [ ] Warning if point falls outside graph range

### 6.5 Output Requirements
- [ ] Travel life (inches)
- [ ] Selected screw specifications:
  - Nominal diameter
  - Threads per inch
  - Lead
- [ ] Driving torque T_u
- [ ] Rotational speed n
- [ ] Required power P
- [ ] All intermediate calculations

### 6.6 Known Challenges
- **Graph-based selection**: Requires either:
  - Interactive graph tool with clickable selection
  - Formula approximation of graph curves
  - Digitized table of load vs travel life for each screw
  - Manual user input from visual graph reading
- **Logarithmic scales**: Difficult to interpolate accurately
- **Limited screw options**: Only 8 standard sizes shown
- **Empirical formula**: 0.177 coefficient may vary by manufacturer
- **No safety factor guidance**: User must determine appropriate margins

---

## 7. REVIEW CHECKLIST

- [x] All pages of PDF reviewed (2 pages)
- [x] All equations extracted and formatted
- [x] All variables catalogued with units
- [x] Graph identified and documented
- [x] Screw data table extracted
- [x] Procedure steps sequenced correctly
- [x] Special cases noted
- [x] Implementation challenges identified
- [x] Cross-references validated

---

**Specification Status**: ✅ COMPLETE - Ready for HTML implementation

**Estimated Implementation Complexity**: MEDIUM
- 10 variables
- 4 equations
- 4-step straightforward procedure
- 1 graph lookup (challenging)
- Small data table (8 screws)
- Logarithmic scale handling

**Recommended Implementation Approach**:
1. Implement travel life calculation (Step 1)
2. Create digitized table or formula approximation of graph curves
3. Implement screw selection logic:
   - Option A: Interactive graph visualization
   - Option B: Table-based lookup with interpolation
   - Option C: Manual selection from dropdown (user reads graph)
4. Implement torque calculation (Step 3)
5. Implement speed and power calculations (Step 4)
6. Provide clear visual feedback for selected screw
7. Add warning if calculated point falls outside graph range
8. Display both log and linear values where appropriate
9. Consider providing formula approximations for each curve to avoid manual graph reading

**Graph Digitization Strategy**:
For each screw size, the relationship between load F and travel life L follows approximately:
```
F × L^k = constant
```
Where k ≈ 0.3 (based on typical ball screw life equations)

Could digitize by sampling several points from each curve and fitting a power law relationship. This would eliminate need for manual graph reading.
