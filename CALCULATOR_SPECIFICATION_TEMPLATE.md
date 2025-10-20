# Calculator Specification: [Calculator Name]

> **Instructions:** Copy this template to create a new specification. Replace all `[placeholders]` and fill in all sections completely before implementing the HTML calculator. See `CALCULATOR_SPECIFICATION_GUIDE.md` for detailed instructions.

---

## 1. Metadata

- **Calculator Name:** [Full descriptive name]
- **Source PDF:** [Filename]
- **PDF Location:** `documents/[filename].pdf`
- **Description:** [Brief description of what this calculator does, what it calculates, and what methods it uses]
- **Complexity:** [Simple / Medium / Complex]
- **Tabs to Include:**
  - [ ] Procedure (Include if PDF has numbered steps)
  - [ ] General Equations (Include if more than 5 equations)
  - [ ] Variables (Almost always include)

**Rationale for Tab Structure:**
[Explain why you included/excluded each tab based on PDF characteristics]

---

## 2. Variables

**Instructions:** List ALL variables from the PDF. Categorize each by Source Type.

### Variable Categories

**Source Types:**
- `User Input` - User enters value directly (white background)
- `Equation` - Calculated from equation (green background)
- `Table` - Manual lookup from PDF table (yellow background, no calculation)
- `Graph` - Manual lookup from PDF graph (yellow background, no calculation)
- `Dropdown` - User selects from predefined options (white background)

### Variables Table

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| [Var] | [Variable Name] | [What it represents] | [unit] | [User Input/Equation/Table/Graph/Dropdown] | [Table/Figure # or EQ#] | [Yes/No/Maybe] | [Constraints, ranges, etc.] |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |

**Example Row:**
```
| P<sub>d</sub> | Diametral Pitch | Gear tooth density | teeth/in | Graph | Figure 9-11 | Maybe | Round to standard value |
```

**Tips:**
- Use HTML for subscripts: `P<sub>d</sub>`, superscripts: `x<sup>2</sup>`
- For Greek letters, write them out: phi, sigma, gamma, etc. (rendered in calculator)
- Sort logically: by usage order, by category, or alphabetically
- Mark all table/graph variables clearly
- Note any variables that might become interactive in the future

---

## 3. Equations (if applicable)

**Instructions:** List ALL equations from the PDF. Assign each a unique ID. Skip this section if no equations exist.

### Equations Table

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ# | `[LaTeX]` | [What it calculates] | [Var1, Var2, ...] | [Result] | [Conditions, constraints] |
| | | | | | |
| | | | | | |

**Example Row:**
```
| EQ1 | `D = \frac{N}{P_d}` | Pitch Diameter | N, Pd | D | - |
```

**LaTeX Conversion Guide:**
- Fractions: `a / b` → `\frac{a}{b}`
- Multiplication: `×` → `\times` or `\cdot`
- Subscripts: `P_d` (stays the same)
- Superscripts: `x^2` (stays the same)
- Greek: `φ` → `\phi`, `σ` → `\sigma`
- Square root: `√x` → `\sqrt{x}`
- Pi: `π` → `\pi`

**Special Cases:**

### Piecewise Equations
If an equation changes based on conditions, document both versions:

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ# | `[Formula1]` if [condition] <br> `[Formula2]` if [condition] | [Description] | [Vars] | [Result] | Piecewise: [explain conditions] |

**Example:**
```
| EQ5 | `h = \frac{2.188}{P_d}` if Pd < 20 <br> `h = \frac{2}{P_d} + 0.002` if Pd ≥ 20 | Whole Depth | Pd | h | Piecewise: coarse vs fine pitch |
```

---

## 4. Procedure Steps (if applicable)

**Instructions:** Document the step-by-step procedure from the PDF. Skip this section if no procedure exists.

### Procedure Table

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| [#] | [What to do] | [Type] | [Specifics] | [Constraints, dependencies] |
| | | | | |
| | | | | |

**Action Types:**
- `User Input` - User enters a value
- `Equation` - Calculate using equation (reference EQ#)
- `Table Lookup` - Manual lookup from PDF table (specify Table #)
- `Graph Lookup` - Manual lookup from PDF graph (specify Figure #)
- `Dropdown` - User selects from options
- `Range Selection` - User chooses within recommended range
- `Iteration` - Try multiple values until condition met

**Example Rows:**
```
| 1-2 | Overload Factor | Table Lookup | Ko from Table 9-1 based on power source and driven machine | Yellow input, manual only |
| 3 | Design Power | Equation | Pdesign = P × Ko (EQ1) | Auto-calculated |
| 4 | Diametral Pitch | Graph Lookup | Pd from Figure 9-11 using design power and pinion speed | Yellow input, round to standard |
| 5 | Pinion Teeth | User Input | Choose NP in recommended range | Typically 17 < NP < 20 |
```

---

## 5. Additional Notes

**Instructions:** Document any special considerations, validation rules, future plans, or anything that doesn't fit in the tables above.

### Piecewise Equations
[List any equations that change based on conditions and explain when each version applies]

Example:
- Whole depth (h) and clearance (c) have different formulas for coarse (Pd < 20) vs fine (Pd ≥ 20) pitch
- Calculator should display which formula is currently active

### Validation Rules
[List any constraints, ranges, or validation requirements]

Example:
- NP must be an integer
- NP typically in range 17 < NP < 20
- NG must be rounded to nearest integer
- F (face width) must satisfy: Fnom ≤ F ≤ Fmax (warning if outside range)

### Standard Values / Constants
[List any typical or standard values that should be pre-filled or suggested]

Example:
- Pressure angle φ typically 20° or 25°
- Elastic coefficient Cp = 2300 psi^0.5 for steel/steel gears (allow manual override)
- Reliability factor KR typically 1.0

### Dropdown Options
[If any variables use dropdowns, list all options]

Example:
- Mounting Factor (Kmb):
  - 1.0 - Both gears straddle-mounted
  - 1.1 - One gear straddle-mounted
  - 1.25 - Neither gear straddle-mounted

### Future Enhancement Opportunities
[Note which manual lookups might become interactive in the future]

Example:
- Table 9-1 (Ko) could become interactive dropdown or calculator
- Figure 9-11 (Pd) could use automatic graph interpolation
- Figure 14-2 (J) could use automatic graph interpolation based on inputs

### Special Considerations
[Any other important information for implementation]

Example:
- Some values need rounding to standard numbers (Pd, NG)
- Warning messages needed for out-of-range values
- Certain steps are iterative (may need trial and error)
- Units conversions needed (psi ↔ ksi, HP ↔ watts)

### Calculation Order / Dependencies
[If calculation order is critical, document the dependency chain]

Example:
```
P (user) →
Ko (table) →
Pdesign = P × Ko →
Pd (graph) →
D = N / Pd →
vt = π × D × n / 12 →
...
```

---

## 6. Review Checklist

Before proceeding to implementation, verify:

- [ ] Metadata section complete
- [ ] All variables from PDF listed and categorized
- [ ] Source references noted for all table/graph variables
- [ ] All equations extracted and converted to LaTeX
- [ ] Procedure steps documented (if applicable)
- [ ] Piecewise equations explained
- [ ] Validation rules documented
- [ ] Future enhancements noted
- [ ] Tab structure decision justified
- [ ] Specification reviewed against PDF (nothing missed)
- [ ] Consistent naming and formatting throughout

---

## 7. Implementation Ready

Once this checklist is complete, this specification is ready to be used with `HTML_IMPLEMENTATION_GUIDE.md` to build the HTML calculator.

**Specification Status:** [ ] Draft  [ ] Complete  [ ] Reviewed  [ ] Ready for Implementation

**Specification Author:** [Your Name]
**Date Created:** [YYYY-MM-DD]
**Last Updated:** [YYYY-MM-DD]

---

## Template Version

Template Version: 1.0
Last Updated: 2025-01-XX
