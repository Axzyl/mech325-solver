# Calculator Specification Guide

This guide teaches you how to analyze a PDF document and create a complete specification for an HTML calculator. The specification serves as a blueprint for implementation, separating "what to build" from "how to build it."

## Table of Contents
1. [Why Create a Specification First?](#why-create-a-specification-first)
2. [Overview of the Specification Process](#overview-of-the-specification-process)
3. [Step 1: Analyze the Source PDF](#step-1-analyze-the-source-pdf)
4. [Step 2: Categorize Variables](#step-2-categorize-variables)
5. [Step 3: Extract and Document Equations](#step-3-extract-and-document-equations)
6. [Step 4: Map the Procedure Flow](#step-4-map-the-procedure-flow)
7. [Step 5: Decide on Tab Structure](#step-5-decide-on-tab-structure)
8. [Step 6: Fill Out the Specification Template](#step-6-fill-out-the-specification-template)
9. [Example: Spur Gear Stress Calculator Specification](#example-spur-gear-stress-calculator-specification)
10. [Specification Checklist](#specification-checklist)

---

## Why Create a Specification First?

Creating a specification document before writing code provides several benefits:

### 1. **Clear Planning**
- Forces you to understand the PDF completely before implementation
- Identifies all variables, equations, and procedure steps upfront
- Prevents missed requirements or last-minute changes

### 2. **Design Documentation**
- Creates a permanent record of design decisions
- Explains why certain approaches were chosen
- Helps onboard new developers or revisit the calculator later

### 3. **Reduced Errors**
- Catch missing variables or equations before coding
- Identify inconsistencies in the PDF
- Plan for edge cases and validations early

### 4. **Easier Implementation**
- HTML implementation becomes straightforward with a complete spec
- No need to constantly refer back to the PDF while coding
- Clear mapping from specification to code

### 5. **Future Planning**
- Documents which tables/graphs might become interactive later
- Identifies enhancement opportunities
- Makes it easier to add features without breaking existing functionality

---

## Overview of the Specification Process

The specification creation process follows these steps:

```
PDF Document
    ↓
Step 1: Analyze PDF (identify variables, equations, procedure)
    ↓
Step 2: Categorize Variables (input, equation, table, graph, dropdown)
    ↓
Step 3: Extract Equations (convert to LaTeX, note dependencies)
    ↓
Step 4: Map Procedure (sequential steps, what to do at each step)
    ↓
Step 5: Decide Tab Structure (which tabs are needed)
    ↓
Step 6: Fill Out Specification Template
    ↓
Completed Specification Document
    ↓
[Ready for HTML Implementation]
```

**Time Investment:** 1-3 hours for a complete specification (saves 3-5 hours during implementation)

**Output:** A structured markdown document using `CALCULATOR_SPECIFICATION_TEMPLATE.md`

**Save Location:**
- Gear calculators: `specifications/gears/calculator_name_spec.md`
- Belt calculators: `specifications/belts/calculator_name_spec.md`

---

## Step 1: Analyze the Source PDF

### 1.1 Initial Read-Through

**Read the entire PDF once** to get a high-level understanding:
- What is being calculated?
- Is there a step-by-step procedure or just equations?
- How complex is it? (simple vs complex calculator)
- What are the inputs and outputs?

**Example:** For a spur gear stress analysis PDF:
- Calculates bending stress and contact stress
- Has a 40-step procedure
- Complex calculator with many variables
- Inputs: gear parameters, material properties; Outputs: stress values, safety factors

### 1.2 Identify All Variables

Create a list of **every variable** mentioned in the PDF.

**For each variable, note:**
- Symbol (e.g., `Pd`, `Ko`, `σb`)
- Full name (e.g., "Diametral Pitch", "Overload Factor", "Bending Stress")
- Description (what it represents)
- Unit (e.g., teeth/in, psi, degrees)
- Where it appears (which pages/steps)

**Tools to help:**
- Create a spreadsheet or table as you go
- Mark each variable when you encounter it in the PDF
- Group related variables together

**Example Variable List (partial):**
```
Pd - Diametral Pitch - Gear tooth density - teeth/in - Pages 2,5,8
Ko - Overload Factor - Accounts for shock loads - dimensionless - Page 3
NP - Number of Pinion Teeth - Pinion tooth count - teeth - Pages 2,4,6
NG - Number of Gear Teeth - Gear tooth count - teeth - Pages 2,4,6
σb - Bending Stress - Stress at tooth root - psi - Pages 10,12
```

**Tip:** Use a consistent naming convention for variable symbols. For subscripts, use `NP` for N<sub>P</sub>, `Pd` for P<sub>d</sub>, etc.

### 1.3 Identify All Equations

List **every equation** in the PDF.

**For each equation, note:**
- What it calculates (output variable)
- Formula (in mathematical notation)
- Input variables required
- Any conditions or restrictions
- Location in PDF (page/step number)

**Example Equation List (partial):**
```
1. D = N / Pd (Pitch Diameter from teeth and pitch)
2. VR = nP / nG (Velocity Ratio from speeds)
3. vt = π × D × n / 12 (Pitch Line Velocity)
4. σb = Wt × Ko × Kv / (F × Pd × J) (Bending Stress)
```

**Special Cases to Note:**
- **Piecewise equations**: Formulas that change based on conditions
  - Example: `h = 2.188/Pd` if Pd < 20, else `h = 2/Pd + 0.002`
- **Ranges**: Variables with min/max constraints
  - Example: `Fnom ≤ F ≤ Fmax`
- **Iterations**: Steps that require trying multiple values
  - Example: "Choose NP between 17 and 20"

### 1.4 Identify Tables and Graphs

List **every table and graph** that requires manual lookup.

**For each table/graph, note:**
- Table/Figure number (e.g., "Table 9-1", "Figure 14-2")
- What it's used to find (which variables)
- Input parameters (what you need to know to use the table/graph)
- Location in PDF

**Example Table/Graph List:**
```
Table 9-1: Overload Factor Ko
  - Inputs: Power source type, driven machine type
  - Output: Ko value
  - Page 3

Figure 9-11: Diametral Pitch Selection
  - Inputs: Design power, pinion speed
  - Output: Pd value
  - Page 5

Figure 14-2: Geometry Factor J
  - Inputs: Number of teeth, pressure angle
  - Output: J value
  - Page 8
```

**🔴 Important:** Tables and graphs will become **yellow input fields** (manual lookup only). Mark these clearly in your specification.

### 1.5 Identify the Procedure (if applicable)

If the PDF has numbered steps, document the procedure flow.

**For each step, note:**
- Step number
- What to do (description)
- What type of action (equation, table lookup, user input, etc.)
- Dependencies (what must be known before this step)

**Example Procedure (partial):**
```
Step 1-2: Look up Ko from Table 9-1
Step 3: Calculate Pdesign = P × Ko
Step 4: Look up Pd from Figure 9-11
Step 5: Choose NP in range 17-20
Step 6: Choose target gear speed nG
Step 7-10: Calculate VR, NG (round to integer), actual nG
```

**If NO procedure exists:**
- Note that it's just a collection of equations
- May not need a Procedure tab
- Focus on documenting equations only

---

## Step 2: Categorize Variables

For **each variable** identified in Step 1, determine its **Source Type**.

### Source Types

| Source Type | Description | Example | Notes |
|-------------|-------------|---------|-------|
| **User Input** | User enters directly | Power P, Speed n | Starting values provided by user |
| **Equation** | Calculated from equation | Pitch diameter D = N/Pd | Auto-calculated but editable |
| **Table** | Manual lookup from PDF table | Overload factor Ko from Table 9-1 | User must look up from PDF |
| **Graph** | Manual lookup from PDF graph | Geometry factor J from Figure 14-2 | User must look up from PDF |
| **Dropdown** | User selects from options | Mounting factor: 1.0, 1.1, or 1.25 | Predefined choices |

**Note:** All input fields in the calculator are editable regardless of source type. The source type is for documentation purposes to understand where values come from.

### How to Categorize

**Ask these questions for each variable:**

1. **Is it a starting value?**
   - If YES → `User Input`
   - Example: Number of teeth, power, speed

2. **Is there an equation to calculate it?**
   - If YES → `Equation` (note the equation)
   - Example: D = N/Pd → D is from equation

3. **Does the PDF say "look up from Table X"?**
   - If YES → `Table` (note table number)
   - Example: "Ko from Table 9-1" → Ko is from table

4. **Does the PDF say "read from Figure X" or show a graph?**
   - If YES → `Graph` (note figure number)
   - Example: "J from Figure 14-2" → J is from graph

5. **Are there discrete options to choose from?**
   - If YES → `Dropdown` (list the options)
   - Example: Mounting: "1.0 (both straddle), 1.1 (one straddle), 1.25 (neither)"

**Special Case:** Some variables can be **both** user input and calculated
- Example: You can either enter a value OR calculate it
- Mark primary source type, note the alternative in "Notes"

### Example Categorization

| Variable | Source Type | Source Reference | Notes |
|----------|-------------|------------------|-------|
| P | User Input | - | Input power |
| nP | User Input | - | Pinion speed |
| Ko | Table | Table 9-1 | Overload factor |
| Pdesign | Equation | Pdesign = P × Ko | Design power |
| Pd | Graph | Figure 9-11 | Round to standard value |
| NP | User Input | - | Choose in range 17-20 |
| D | Equation | D = N / Pd | Pitch diameter |
| J | Graph | Figure 14-2 | Geometry factor |

---

## Step 3: Extract and Document Equations

Convert all equations from the PDF into a structured format.

### 3.1 Assign Equation IDs

Give each equation a unique identifier (EQ1, EQ2, etc.)

**Numbering Convention:**
- Number sequentially as they appear in the PDF
- Or group by category (EQ-GEOM-1, EQ-STRESS-1, etc.)

**Example:**
```
EQ1: D = N / Pd
EQ2: VR = nP / nG
EQ3: vt = π × D × n / 12
```

### 3.2 Convert to LaTeX

Convert each equation to LaTeX for MathJax rendering.

**Common LaTeX conversions:**
- Division: `a / b` → `\frac{a}{b}`
- Multiplication: `a × b` → `a \times b` or `a \cdot b`
- Subscripts: `N_P` → `N_P` (stays the same)
- Superscripts: `x^2` → `x^2` (stays the same)
- Greek letters: `φ` → `\phi`, `σ` → `\sigma`
- Square root: `√x` → `\sqrt{x}`
- Pi: `π` → `\pi`

**Example LaTeX conversions:**
```
PDF: D = N / Pd
LaTeX: D = \frac{N}{P_d}

PDF: vt = π × D × n / 12
LaTeX: v_t = \frac{\pi D n}{12}

PDF: σb = (Wt × Ko × Kv) / (F × Pd × J)
LaTeX: \sigma_b = \frac{W_t \times K_o \times K_v}{F \times P_d \times J}
```

**Tool:** You can use an online LaTeX editor to test your formulas: https://latex.codecogs.com/eqneditor/editor.php

### 3.3 Document Equation Details

For each equation, create an entry in your specification:

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `D = \frac{N}{P_d}` | Pitch Diameter | N, Pd | D | - |
| EQ2 | `VR = \frac{n_P}{n_G}` | Velocity Ratio | nP, nG | VR | - |
| EQ3 | `v_t = \frac{\pi D n}{12}` | Pitch Line Velocity | D, n | vt | - |

**For piecewise equations**, document all conditions:

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ5 | `h = \frac{2.188}{P_d}` (Pd < 20) <br> `h = \frac{2}{P_d} + 0.002` (Pd ≥ 20) | Whole Depth | Pd | h | Piecewise: coarse vs fine pitch |

---

## Step 4: Map the Procedure Flow

If the PDF has a sequential procedure, map out the step-by-step flow.

### 4.1 Document Each Step

For each procedure step, create an entry:

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1-2 | Overload Factor | Table Lookup | Ko from Table 9-1 | Yellow input |
| 3 | Design Power | Equation | Pdesign = P × Ko (EQ1) | - |
| 4 | Diametral Pitch | Graph Lookup | Pd from Figure 9-11 | Yellow input, round to standard |
| 5 | Pinion Teeth | User Input | Choose NP in range 17-20 | Recommended range |
| 6 | Target Gear Speed | User Input | Choose nG (typically middle of range) | - |
| 7 | Velocity Ratio | Equation | VR = nP / nG (EQ2) | - |

### 4.2 Action Types

Use consistent action types:

- **User Input**: User enters a value
- **Equation**: Calculate using equation (reference equation ID)
- **Table Lookup**: Manual lookup from PDF table (specify table number)
- **Graph Lookup**: Manual lookup from PDF graph (specify figure number)
- **Dropdown**: User selects from predefined options
- **Range Selection**: User chooses within recommended range
- **Iteration**: Try multiple values until condition is met

### 4.3 Note Dependencies

In the "Notes" column, document:
- Prerequisites (what must be known before this step)
- Constraints (ranges, rounding rules, etc.)
- Warnings (e.g., "Round to nearest standard value")
- Future interactivity plans (e.g., "Could add automatic table lookup later")

---

## Step 5: Decide on Tab Structure

Determine which tabs the calculator needs based on the PDF complexity.

### 5.1 Procedure Tab

**Include Procedure Tab when:**
- ✅ PDF has numbered steps (Step 1, Step 2, etc.)
- ✅ Calculations must be done in a specific order
- ✅ There's a workflow to follow from start to finish
- ✅ PDF guides you through a process

**Omit Procedure Tab when:**
- ❌ PDF only has isolated equations without a procedure
- ❌ User can calculate values in any order
- ❌ No sequential workflow exists

**Example:**
- `mech_spur_stress.html` → ✅ Has Procedure tab (40-step process)
- Pure equation reference → ❌ No Procedure tab needed

### 5.2 General Equations Tab

**Include General Equations Tab when:**
- ✅ PDF contains more than 5-10 equations
- ✅ Users might want quick access to specific formulas
- ✅ Equations are complex and benefit from having a reference
- ✅ Equations are used across multiple steps

**Omit General Equations Tab when:**
- ❌ PDF has fewer than 5 equations
- ❌ Equations are very simple (one-step calculations)
- ❌ All equations fit comfortably in Procedure tab
- ❌ No need for a separate equation reference

**Example:**
- `mech_bevel_geometry.html` → ✅ Has General Equations tab (25+ equations)
- `mech_gear_forces.html` → ❌ No General Equations tab (only 3 simple equations)

### 5.3 Variables Tab

**Include Variables Tab:**
- ✅ Almost always include this tab
- ✅ Especially helpful when there are 20+ variables
- ✅ Provides a single place to view/edit all values
- ✅ Serves as a comprehensive variable reference

**Omit Variables Tab only when:**
- ⚠️ Calculator has fewer than 10 variables (rare)
- ⚠️ It's an extremely simple calculator

**Example:**
- Most calculators → ✅ Has Variables tab
- Very simple calculators → ⚠️ Might omit

### 5.4 Decision Matrix

| PDF Characteristics | Tabs to Include |
|-------------------|-----------------|
| 40-step procedure, 30+ variables, 20+ equations | Procedure + General Equations + Variables |
| 10-step procedure, 15 variables, 8 equations | Procedure + Variables (skip General Equations) |
| No procedure, 25 variables, 20 equations | General Equations + Variables (skip Procedure) |
| 3 simple force equations, 8 variables | Procedure + Variables (skip General Equations) |

### 5.5 Document Your Decision

In the specification, clearly state which tabs to include and why:

```markdown
## Tabs to Include:
- [x] Procedure - PDF has 40 numbered steps
- [x] General Equations - 25+ equations benefit from reference
- [x] Variables - 30+ variables need central management

Rationale: Complex calculator with sequential procedure and many equations.
```

---

## Step 6: Fill Out the Specification Template

Now that you've analyzed the PDF, fill out the `CALCULATOR_SPECIFICATION_TEMPLATE.md`.

### 6.1 Copy the Template

**Folder Structure:**
Specifications are organized by calculator type:
```
specifications/
├── gears/        # Gear calculator specifications
└── belts/        # Belt calculator specifications
```

**Naming Convention:**
Name your specification to match the HTML calculator filename:
- If HTML file is `mech_spur_stress.html` → spec is `mech_spur_stress_spec.md`
- If HTML file is `fizz_bevel.html` → spec is `fizz_bevel_spec.md`

**Copy Template:**

For gear calculators:
```bash
cp CALCULATOR_SPECIFICATION_TEMPLATE.md specifications/gears/your_calculator_name_spec.md
```

For belt calculators:
```bash
cp CALCULATOR_SPECIFICATION_TEMPLATE.md specifications/belts/your_calculator_name_spec.md
```

**Examples:**
```bash
# Gear calculator
cp CALCULATOR_SPECIFICATION_TEMPLATE.md specifications/gears/mech_spur_stress_spec.md

# Belt calculator
cp CALCULATOR_SPECIFICATION_TEMPLATE.md specifications/belts/v_belt_design_spec.md
```

### 6.2 Fill Out Section 1: Metadata

```markdown
## 1. Metadata
- **Calculator Name:** Spur Gear Bending and Contact Stress Analysis
- **Source PDF:** spur_stress.pdf
- **PDF Location:** documents/spur_stress.pdf
- **Description:** Calculates bending stress, contact stress, and safety factors for spur gears following the AGMA method.
- **Tabs to Include:**
  - [x] Procedure
  - [x] General Equations
  - [x] Variables
```

### 6.3 Fill Out Section 2: Variables

Transfer your variable list from Step 2 into the table format.

**For each variable, include:**
- Symbol (HTML format: use `<sub>` and `<sup>` if needed)
- Name
- Description
- Unit
- Source Type
- Source Reference (table/figure number if applicable)
- Future Interactive? (Yes/No/Maybe)
- Notes

**Example:**
```markdown
| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| P | Power | Input power | HP | User Input | - | No | - |
| n<sub>P</sub> | Pinion Speed | Rotational speed of pinion | rpm | User Input | - | No | - |
| K<sub>o</sub> | Overload Factor | Accounts for shock loads | - | Table | Table 9-1 | Maybe | Could add interactive table later |
| P<sub>design</sub> | Design Power | Power adjusted for overload | HP | Equation | EQ1 | No | - |
| P<sub>d</sub> | Diametral Pitch | Gear tooth density | teeth/in | Graph | Figure 9-11 | Maybe | Could add graph interpolation |
| N<sub>P</sub> | Pinion Teeth | Number of teeth on pinion | teeth | User Input | - | No | Choose in range 17-20 |
| D | Pitch Diameter | Pinion pitch diameter | in | Equation | EQ2 | No | - |
| J | Geometry Factor | Tooth strength factor | - | Graph | Figure 14-2 | Maybe | Could add graph interpolation |
```

**Tips:**
- Sort variables by category or order of use
- Mark all table/graph variables clearly
- Note any that might become interactive in the future
- Include constraints in "Notes" column

### 6.4 Fill Out Section 3: Equations

Transfer your equation list from Step 3 into the table format.

**Example:**
```markdown
| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_{design} = P \times K_o` | Design Power | P, Ko | Pdesign | - |
| EQ2 | `D = \frac{N_P}{P_d}` | Pitch Diameter | NP, Pd | D | - |
| EQ3 | `VR = \frac{n_P}{n_G}` | Velocity Ratio | nP, nG | VR | - |
| EQ4 | `v_t = \frac{\pi D n}{12}` | Pitch Line Velocity | D, n | vt | - |
| EQ5 | `h = \frac{2.188}{P_d}` (Pd < 20) <br> `h = \frac{2}{P_d} + 0.002` (Pd ≥ 20) | Whole Depth | Pd | h | Piecewise: coarse vs fine pitch |
```

### 6.5 Fill Out Section 4: Procedure Steps

Transfer your procedure mapping from Step 4 into the table format.

**Example:**
```markdown
| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1-2 | Overload Factor | Table Lookup | Ko from Table 9-1 based on power source and driven machine | Yellow input, manual only |
| 3 | Design Power | Equation | Pdesign = P × Ko (EQ1) | Auto-calculated |
| 4 | Diametral Pitch | Graph Lookup | Pd from Figure 9-11 using design power and pinion speed | Yellow input, round to standard value |
| 5 | Pinion Teeth Selection | User Input | Choose NP in range 17 < NP < 20 | Recommended range |
| 6 | Target Gear Speed | User Input | Choose nG (typically middle of given range) | - |
| 7-10 | Velocity Ratio and Gear Teeth | Equation | VR = nP/nG (EQ3), NG = NP × VR, round NG to integer | Multiple steps combined |
```

### 6.6 Fill Out Section 5: Additional Notes

Document any special considerations:

**Example:**
```markdown
## 5. Additional Notes

### Piecewise Equations
- Whole depth (h) and clearance (c) have different formulas for coarse (Pd < 20) vs fine (Pd ≥ 20) pitch
- Calculator should display which formula is active
- Add info box explaining the conditions

### Validation Rules
- NP must be integer, typically 17 < NP < 20
- NG must be rounded to nearest integer
- F (face width) must satisfy: Fnom ≤ F ≤ Fmax

### Future Enhancements
- Table 9-1 (Ko) could become interactive dropdown
- Figure 9-11 (Pd) could use graph interpolation
- Figure 14-2 (J) could use graph interpolation

### Standard Values
- Cp = 2300 psi^0.5 for steel/steel gears (allow override)
- Pressure angle φ typically 20° or 25°
```

---

## Example: Spur Gear Stress Calculator Specification

Here's a partial example of a completed specification:

```markdown
# Calculator Specification: Spur Gear Bending and Contact Stress

## 1. Metadata
- **Calculator Name:** Spur Gear Bending and Contact Stress Analysis
- **Source PDF:** spur_stress.pdf
- **PDF Location:** documents/spur_stress.pdf
- **Description:** Calculates bending stress, contact stress, and safety factors for spur gears using AGMA method.
- **Tabs to Include:**
  - [x] Procedure (40-step process)
  - [x] General Equations (20+ equations)
  - [x] Variables (30+ variables)

---

## 2. Variables

| Symbol | Name | Description | Unit | Source Type | Source Reference | Future Interactive? | Notes |
|--------|------|-------------|------|-------------|------------------|-------------------|-------|
| P | Power | Input power | HP | User Input | - | No | - |
| K<sub>o</sub> | Overload Factor | Shock load factor | - | Table | Table 9-1 | Maybe | Table lookup |
| P<sub>d</sub> | Diametral Pitch | Tooth density | teeth/in | Graph | Figure 9-11 | Maybe | Graph lookup, round to standard |
| J | Geometry Factor | Tooth strength | - | Graph | Figure 14-2 | Maybe | Graph lookup |
| σ<sub>b</sub> | Bending Stress | Stress at root | psi | Equation | EQ15 | No | - |
| ... | ... | ... | ... | ... | ... | ... | ... |

---

## 3. Equations

| ID | LaTeX Formula | Description | Input Variables | Output Variable | Notes |
|----|---------------|-------------|-----------------|-----------------|-------|
| EQ1 | `P_{design} = P \times K_o` | Design Power | P, Ko | Pdesign | - |
| EQ2 | `D = \frac{N_P}{P_d}` | Pitch Diameter | NP, Pd | D | - |
| EQ15 | `\sigma_b = \frac{W_t K_o K_v}{F P_d J}` | Bending Stress | Wt, Ko, Kv, F, Pd, J | σb | Main stress equation |
| ... | ... | ... | ... | ... | ... |

---

## 4. Procedure Steps

| Step | Description | Action Type | Details | Notes |
|------|-------------|-------------|---------|-------|
| 1-2 | Overload Factor | Table Lookup | Ko from Table 9-1 | Yellow input |
| 3 | Design Power | Equation | Pdesign = P × Ko (EQ1) | - |
| 15 | Bending Stress | Equation | σb = Wt × Ko × Kv / (F × Pd × J) (EQ15) | Main calculation |
| ... | ... | ... | ... | ... |

---

## 5. Additional Notes

### Piecewise Equations
- h and c formulas change at Pd = 20

### Future Enhancements
- Tables 9-1 could have dropdown options
- Figures 9-11 and 14-2 could use interpolation
```

This specification can now be handed to someone (or used by yourself later) to implement the HTML calculator without constantly referring back to the PDF.

---

## Specification Checklist

Use this checklist to ensure your specification is complete:

### Metadata
- [ ] Calculator name filled in
- [ ] Source PDF identified
- [ ] Description written
- [ ] Tab structure decided (checkboxes marked)

### Variables
- [ ] All variables from PDF listed
- [ ] Symbol, name, description, unit documented for each
- [ ] Source type categorized (input/equation/table/graph/dropdown)
- [ ] Source references noted for tables/graphs
- [ ] Future interactivity noted where applicable
- [ ] Variables sorted logically (by category or usage order)

### Equations (if applicable)
- [ ] All equations extracted from PDF
- [ ] Equation IDs assigned
- [ ] LaTeX formulas written
- [ ] Input and output variables noted
- [ ] Piecewise equations documented with conditions
- [ ] Dependencies clear

### Procedure Steps (if applicable)
- [ ] All procedure steps documented
- [ ] Action types specified
- [ ] Details provided (equation IDs, table/figure numbers)
- [ ] Dependencies and constraints noted
- [ ] Manual lookup steps clearly marked

### Additional Notes
- [ ] Piecewise equations explained
- [ ] Validation rules documented
- [ ] Standard values noted
- [ ] Future enhancement ideas captured
- [ ] Special considerations addressed

### Review
- [ ] Specification reviewed for completeness
- [ ] Cross-checked against PDF (nothing missed)
- [ ] Consistent naming and formatting
- [ ] Ready for implementation

---

## Next Steps

Once your specification is complete:

1. **Review the specification** for completeness
2. **Validate** against the source PDF (did you miss anything?)
3. **Save** the specification in the appropriate folder:
   - Gears: `specifications/gears/calculator_name_spec.md`
   - Belts: `specifications/belts/calculator_name_spec.md`
4. **Get feedback** if working with a team
5. **Proceed to implementation** using `HTML_IMPLEMENTATION_GUIDE.md`

The specification serves as your blueprint. If the specification is thorough and accurate, the HTML implementation will be straightforward and error-free.

**File Organization:**
```
project/
├── specifications/
│   ├── gears/
│   │   └── mech_spur_stress_spec.md     ← Completed specification
│   └── belts/
│       └── v_belt_design_spec.md         ← Completed specification
├── documents/
│   ├── spur_stress.pdf                   ← Source PDF
│   └── v_belt_design.pdf                 ← Source PDF
└── calculators/
    ├── mech_spur_stress.html             ← Implemented calculator
    └── v_belt_design.html                ← Implemented calculator
```

**Good luck with your calculator specification!**
