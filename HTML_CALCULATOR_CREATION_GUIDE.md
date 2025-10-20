# HTML Calculator Creation Guide

This document provides step-by-step instructions for creating standalone HTML gear calculators from PDF specifications. These calculators feature bidirectional variable syncing across multiple tabs, real-time calculations, and session persistence.

## Table of Contents
1. [Overview](#overview)
2. [File Structure](#file-structure)
3. [Core Features](#core-features)
4. [Step-by-Step Creation Process](#step-by-step-creation-process)
   - [Step 1: Analyze the Source PDF](#step-1-analyze-the-source-pdf)
   - [Step 1.5: Understanding Tab Structure and Content](#step-15-understanding-tab-structure-and-content)
   - [Step 1.6: Handling Different PDF Elements](#step-16-handling-different-pdf-elements)
5. [Variable Syncing System](#variable-syncing-system)
6. [Tab System Implementation](#tab-system-implementation)
7. [Calculation Engine](#calculation-engine)
8. [Session Persistence](#session-persistence)
9. [Testing and Validation](#testing-and-validation)
10. [Common Issues and Solutions](#common-issues-and-solutions)
11. [Checklist for Creating a New Calculator](#checklist-for-creating-a-new-calculator)

---

## Overview

The HTML calculators are self-contained, single-file applications that:
- Display a reference PDF on the left panel
- Provide an interactive calculator on the right panel
- Feature three tabs: **Procedure**, **General Equations**, and **Variables**
- Sync all variable instances bidirectionally across all tabs
- Save/load sessions using browser LocalStorage
- Work completely offline once loaded

**Examples:** `mech_spur_stress.html`, `mech_helical_stress.html`, `mech_bevel_stress.html`, `mech_bevel_geometry.html`

---

## File Structure

Each calculator is a single HTML file with the following structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Calculator Name</title>

    <!-- MathJax CDN for LaTeX rendering -->
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

    <style>
        /* CSS styles here */
    </style>
</head>
<body>
    <!-- Two-panel layout -->
    <div id="pdf-panel">
        <iframe src="../documents/your_pdf.pdf"></iframe>
    </div>

    <div id="calculator-panel">
        <!-- Header, controls, tabs, and content -->
    </div>

    <script>
        /* JavaScript code here */
    </script>
</body>
</html>
```

---

## Core Features

### 1. Two-Panel Layout
- **Left panel**: Embedded PDF reference document (resizable)
- **Right panel**: Interactive calculator with scrolling

### 2. Three-Tab System
- **Procedure Tab**: Step-by-step workflow following the PDF
- **General Equations Tab**: All equations with editable input boxes
- **Variables Tab**: Complete table of all variables with inputs

### 3. Bidirectional Variable Syncing
- Changing a variable in any tab updates all other instances across all tabs
- Implemented via `varInstanceMap` and `syncAllInstances()` function

### 4. Visual Feedback (Color-Coded Input Fields)
- **White background**: User-entered values (can be edited freely)
- **Yellow background**: Manual lookups from tables/graphs (NO automatic calculation - user must look up from PDF)
- **Green background**: Automatically calculated values (computed by the calculator)

### 5. Session Persistence
- Save current state to browser LocalStorage
- Load previous session on page reload
- Reset all values to empty state

### 6. Real-Time Calculation
- Calculations trigger automatically when any input changes
- Uses `oninput` event handlers on all input fields

---

## Step-by-Step Creation Process

### Step 1: Analyze the Source PDF

1. **Identify all variables** in the PDF
   - Create a list with variable names, symbols, units, and descriptions
   - Example: `Pd` (Diametral Pitch, teeth/in), `phi` (Pressure Angle, degrees)

2. **List all equations** in order
   - Note dependencies between variables
   - Identify piecewise equations (different formulas based on conditions)

3. **Identify the procedure steps**
   - Note the sequential workflow from the PDF
   - Group related calculations together

4. **Find lookup tables/charts**
   - Note which variables require manual lookup (mark as yellow inputs)

### Step 1.5: Understanding Tab Structure and Content

Before building the calculator, understand what content goes into each tab and when tabs are needed.

#### What Goes in the Procedure Tab?

The **Procedure Tab** follows the step-by-step workflow from the PDF document.

**Include:**
- **Sequential steps** exactly as they appear in the PDF (Step 1, Step 2, etc.)
- **Equations** with input boxes for variables, displayed inline with the workflow
- **Instructions** copied from the PDF (e.g., "Look up from Table 9-1")
- **Info boxes** highlighting manual lookups or important notes
- **Dividers** between major steps for visual organization
- **Conditional branches** if the PDF has different paths (e.g., coarse vs fine pitch)

**Example from mech_bevel_stress.html:**
```
Step 1-2: Overload Factor
[Info box: Look up Ko from Table 9-1]
[Input box for Ko]

Step 2: Basic Gear Parameters
[Input boxes for NP, NG, Pd, F, P, n, D]

Step 3: Pitch Line Velocity
[Equation: vt = πDn / 12]
[Input boxes for vt, D, n]
```

**When to use Procedure Tab:**
- ✅ When PDF has a numbered step-by-step procedure
- ✅ When calculations must be done in a specific order
- ✅ When there's a workflow to follow from start to finish
- ❌ When PDF only has isolated equations without a procedure

#### What Goes in the General Equations Tab?

The **General Equations Tab** contains ALL equations from the PDF, presented as a reference with editable inputs.

**Include:**
- **Every equation** from the PDF, even if already in Procedure tab
- **Input boxes for ALL variables** in each equation (not just the result)
- **Section headers** grouping related equations (e.g., "Basic Geometry", "Stress Factors")
- **Brief descriptions** explaining what each equation calculates
- **All formula variations** (e.g., both coarse and fine pitch versions)

**Example from mech_bevel_geometry.html:**
```
### Basic Geometry
mG = NG / NP
[Inputs: mG, NG, NP]
Description: Gear Ratio

D = NG / Pd
[Inputs: D, NG, Pd]
Description: Gear Pitch Diameter

d = NP / Pd
[Inputs: d, NP, Pd]
Description: Pinion Pitch Diameter
```

**When to use General Equations Tab:**
- ✅ When PDF contains more than 5-10 equations
- ✅ When users might want quick access to specific formulas
- ✅ When equations are complex and benefit from having a reference
- ❌ When PDF has only 2-3 simple equations (can omit this tab for simple calculators)

#### What Goes in the Variables Tab?

The **Variables Tab** provides a complete table of ALL variables used in the calculator.

**Include:**
- **Every variable** mentioned in any equation
- **Organized by category** (e.g., "Given Parameters", "Calculated Values", "Stress Factors")
- **Symbol** with proper HTML subscripts/superscripts
- **Description** of what the variable represents
- **Input box** for entering/viewing the value
- **Unit** of measurement

**Example from mech_spur_stress.html:**
```
Category: Basic Parameters
| Symbol | Description           | Value [input] | Unit      |
|--------|-----------------------|---------------|-----------|
| Pd     | Diametral Pitch       | [input]       | teeth/in  |
| φ      | Pressure Angle        | [input]       | deg       |
| NP     | Pinion Teeth          | [input]       | teeth     |

Category: Calculated Values
| Symbol | Description           | Value [input] | Unit      |
|--------|-----------------------|---------------|-----------|
| DP     | Pinion Pitch Diameter | [input]       | in        |
| vt     | Pitch Line Velocity   | [input]       | ft/min    |
```

**When to use Variables Tab:**
- ✅ Always use this tab (it serves as a comprehensive variable reference)
- ✅ Especially helpful when there are 20+ variables
- ✅ Provides a single place to view/edit all values
- ⚠️ Can omit only for the simplest calculators (< 10 variables)

#### When to Omit Tabs?

**Simple calculators** (like `mech_gear_forces.html`) may not need all three tabs:

**Omit General Equations Tab when:**
- PDF has fewer than 5 equations
- Equations are very simple (one-step calculations)
- All equations fit comfortably in the Procedure tab
- No need for a separate equation reference

**Example: mech_gear_forces.html structure:**
- ✅ Has Procedure tab (forces calculation workflow)
- ❌ No General Equations tab (only 3 simple force equations)
- ✅ Has Variables tab (complete variable reference)

**Keep Procedure Tab when:**
- PDF has a clear numbered procedure
- Calculations must follow a specific order

**Omit Procedure Tab when:**
- PDF is just a collection of equations without a workflow
- No sequential steps to follow
- User can calculate values in any order

**Always keep Variables Tab** unless:
- Calculator has fewer than 10 variables
- It's an extremely simple calculator

### Step 1.6: Handling Different PDF Elements

When analyzing the PDF, you'll encounter different types of content. Here's how to handle each:

---

**🔴 CRITICAL RULE: Tables and Graphs**

Whenever you encounter a **table** or **graph** in the PDF:
- ✅ Create a **yellow input box** for the associated variable(s)
- ✅ Add an **info box** directing users to the specific table/figure in the PDF
- ❌ **DO NOT** implement any automatic calculation for this variable
- ❌ **DO NOT** attempt to recreate the table/graph in HTML
- ❌ **DO NOT** write code to interpolate or compute values from the table/graph

**The user MUST manually look up the value from the PDF and enter it.**

Yellow background = Manual lookup only, no calculation.

---

#### Equations

**What to do:**
1. **Copy the equation** exactly as written in the PDF
2. **Convert to LaTeX** for MathJax rendering
   - Example: `σ = Wt × Ko × Kv / (F × Pd × J)` becomes `$\sigma = \frac{W_t \times K_o \times K_v}{F \times P_d \times J}$`
3. **Create input boxes** for:
   - The result variable (left side of equation)
   - ALL variables on the right side of the equation
4. **Add to Procedure Tab** in the step where it's used
5. **Add to General Equations Tab** in the appropriate section

**Example:**

PDF shows: `D = N / Pd`

HTML Implementation:
```html
<div class="equation-block">
    <div class="equation-latex">$$D = \frac{N}{P_d}$$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>D =</label>
            <input type="number" id="D" step="0.0001"
                   oninput="syncAllInstances('D'); markAsUser('D'); calculate()">
            <span class="unit">in</span>
        </div>
        <span>=</span>
        <div class="var-input">
            <label>N =</label>
            <input type="number" id="N_eq1" step="1"
                   oninput="syncAllInstances('N_eq1'); markAsUser('N_eq1'); calculate()">
            <span class="unit">teeth</span>
        </div>
        <span>/</span>
        <div class="var-input">
            <label>P<sub>d</sub> =</label>
            <input type="number" id="Pd_eq1" step="0.0001"
                   oninput="syncAllInstances('Pd_eq1'); markAsUser('Pd_eq1'); calculate()">
            <span class="unit">teeth/in</span>
        </div>
    </div>
</div>
```

**Piecewise Equations:**

If equation changes based on conditions (e.g., coarse vs fine pitch):

PDF shows:
```
h = 2.188 / Pd    (if Pd < 20)
h = 2 / Pd + 0.002    (if Pd ≥ 20)
```

HTML Implementation:
```html
<div class="equation-block">
    <div class="equation-latex" id="h-equation-latex">
        $$h = \frac{2.188}{P_d}$$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>h =</label>
            <input type="number" id="h" step="0.0001"
                   oninput="syncAllInstances('h'); markAsUser('h'); calculate()">
            <span class="unit">in</span>
        </div>
        <span id="h-formula-text">= 2.188 /</span>
        <div class="var-input">
            <label>P<sub>d</sub> =</label>
            <input type="number" id="Pd_eq7" step="0.0001"
                   oninput="syncAllInstances('Pd_eq7'); markAsUser('Pd_eq7'); calculate()">
            <span class="unit">teeth/in</span>
        </div>
    </div>
</div>

<!-- Add info box -->
<div class="info-box">
    Formula changes based on pitch:
    <ul>
        <li>Pd < 20 (coarse): h = 2.188 / Pd</li>
        <li>Pd ≥ 20 (fine): h = 2 / Pd + 0.002</li>
    </ul>
</div>
```

JavaScript:
```javascript
function calculate() {
    const Pd = getValue('Pd');

    if (Pd !== null) {
        let h;
        if (Pd < 20) {
            h = 2.188 / Pd;
            // Update displayed formula
            document.getElementById('h-formula-text').textContent = '= 2.188 /';
        } else {
            h = 2 / Pd + 0.002;
            document.getElementById('h-formula-text').textContent = '= 2 / Pd + 0.002';
        }
        setValue('h', h);
    }
}
```

#### Lookup Tables

**IMPORTANT: Tables require manual lookup only - no automatic calculation.**

**What to do:**
1. **Don't recreate the table** in HTML (too complex, users need PDF reference)
2. **Create a manual INPUT field** with yellow background
3. **Add an info box** directing users to the specific table in the PDF
4. **Reference the table/figure number** from the PDF
5. **DO NOT calculate this variable** - user must look it up manually
6. **Exclude from automatic calculations** - leave the calculate() function empty for this variable

**Example:**

PDF shows: "Table 9-1: Overload Factor Ko"

HTML Implementation:
```html
<h3>Step 1: Overload Factor</h3>

<div class="info-box">
    📊 <strong>MANUAL LOOKUP REQUIRED</strong> - Look up K<sub>o</sub> from <strong>Table 9-1</strong> in the PDF based on:
    <ul>
        <li>Power source characteristics (uniform, light shock, medium shock)</li>
        <li>Driven machine characteristics</li>
    </ul>
    Enter the value you find in the table below.
</div>

<div class="var-input">
    <label>K<sub>o</sub> =</label>
    <input type="number" id="Ko" step="0.01" class="manual-lookup"
           oninput="syncAllInstances('Ko'); markAsUser('Ko'); calculate()">
    <span class="unit">(from Table 9-1 - enter manually)</span>
</div>
```

CSS for yellow background:
```css
input.manual-lookup {
    background-color: #fff3cd;  /* Yellow background */
    border-color: #ffc107;
}
```

JavaScript - **DO NOT** calculate this variable:
```javascript
function calculate() {
    const Ko = getValue('Ko');  // Get user input
    // DO NOT calculate Ko - it's from a table
    // Ko remains whatever the user entered

    // Use Ko in other calculations
    if (Ko !== null && Wt !== null && Kv !== null) {
        const stress = Wt * Ko * Kv / (F * Pd);
        setValue('stress', stress);
    }
}
```

**Key Point:** Variables from tables are **input-only**. The yellow background signals to users that they must manually look up this value from the PDF. The calculator does NOT compute these values automatically.

#### Graphs and Charts

**IMPORTANT: Graphs require manual lookup only - no automatic calculation.**

**What to do:**
1. **Don't recreate the graph** in HTML
2. **Create a manual INPUT field** with yellow background
3. **Reference the figure number** and provide step-by-step reading instructions
4. **DO NOT calculate this variable** - user must read it from the graph manually
5. **Exclude from automatic calculations** - leave the calculate() function empty for this variable

**Example:**

PDF shows: "Figure 14-2: Geometry Factor J"

HTML Implementation:
```html
<h3>Step 4: Geometry Factor</h3>

<div class="info-box">
    📈 <strong>MANUAL LOOKUP REQUIRED</strong> - Look up J from <strong>Figure 14-2</strong> in the PDF:
    <ol>
        <li>Locate your pressure angle (φ) curve on the graph</li>
        <li>Find number of teeth on the horizontal axis</li>
        <li>Read the J value from the vertical axis</li>
        <li>Enter the value you read below</li>
    </ol>
</div>

<div class="var-input">
    <label>J =</label>
    <input type="number" id="J" step="0.0001" class="manual-lookup"
           oninput="syncAllInstances('J'); markAsUser('J'); calculate()">
    <span class="unit">(from Figure 14-2 - enter manually)</span>
</div>
```

JavaScript - **DO NOT** calculate this variable:
```javascript
function calculate() {
    const J = getValue('J');  // Get user input
    // DO NOT calculate J - it's from a graph
    // J remains whatever the user entered

    // Use J in other calculations
    if (Wt !== null && Ko !== null && J !== null) {
        const stress = Wt * Ko / (F * Pd * J);
        setValue('stress', stress);
    }
}
```

**Key Point:** Variables from graphs/charts are **input-only**. The yellow background signals to users that they must manually read this value from the PDF graph. The calculator does NOT compute or interpolate these values automatically.

#### Dropdown Selections

**What to do:**
1. **Use HTML select/dropdown** for discrete choices
2. **List all options** from the PDF
3. **Include numeric values** that will be used in calculations

**Example:**

PDF shows: "Mounting Factor Kmb: 1.0 (both straddle), 1.1 (one straddle), 1.25 (neither straddle)"

HTML Implementation:
```html
<h3>Step 6: Mounting Factor</h3>

<div class="info-box">
    Select based on bearing arrangement:
    <ul>
        <li>Both straddle: Both pinion and gear are straddle-mounted</li>
        <li>One straddle: Only one gear is straddle-mounted</li>
        <li>Neither straddle: Neither gear is straddle-mounted</li>
    </ul>
</div>

<div class="var-input">
    <label>K<sub>mb</sub> =</label>
    <select id="Kmb" onchange="syncAllInstances('Kmb'); markAsUser('Kmb'); calculate()">
        <option value="">Select...</option>
        <option value="1">1.0 (Both straddle)</option>
        <option value="1.1">1.1 (One straddle)</option>
        <option value="1.25">1.25 (Neither straddle)</option>
    </select>
</div>
```

JavaScript adjustment:
```javascript
function getValue(id) {
    const element = document.getElementById(id);
    if (!element) return null;

    // Handle select elements
    if (element.tagName === 'SELECT') {
        return element.value === '' ? null : parseFloat(element.value);
    }

    // Handle number inputs
    const val = parseFloat(element.value);
    return isNaN(val) ? null : val;
}
```

#### Procedures/Instructions

**What to do:**
1. **Copy the text** directly from the PDF
2. **Format as headers** (h3 for steps, h4 for sub-steps)
3. **Use info boxes** for important notes or warnings
4. **Use ordered lists** for multi-step instructions

**Example:**

PDF text: "Step 3: Calculate the bending stress number. Use the equation below. Note that the stress cycle factor must be determined from Figure 14-14."

HTML Implementation:
```html
<h3>Step 3: Calculate Bending Stress</h3>

<div class="info-box">
    ⚠️ <strong>Important:</strong> The stress cycle factor K<sub>L</sub> must be determined from Figure 14-14 based on the number of load cycles.
</div>

<div class="equation-block">
    <!-- Equation here -->
</div>
```

#### Worked Examples

**What to do:**
1. **Don't include in the calculator** (it's for reference only)
2. **Optionally add a note** directing users to the example in the PDF
3. **Focus on implementing the equations**, not the examples

**Example:**
```html
<div class="info-box">
    💡 <strong>Tip:</strong> See Example 14-2 in the PDF for a complete worked example of this calculation.
</div>
```

#### Constants and Standard Values

**What to do:**
1. **Use dropdown or preset values** if there are standard options
2. **Allow manual override** for custom values
3. **Document the source** (e.g., "Typical value", "From AGMA Standard")

**Example:**

PDF shows: "Cp = 2300 psi^0.5 for steel gears"

HTML Implementation:
```html
<div class="var-input">
    <label>C<sub>p</sub> =</label>
    <input type="number" id="Cp" step="1"
           oninput="syncAllInstances('Cp'); markAsUser('Cp'); calculate()">
    <span class="unit">psi<sup>1/2</sup></span>
    <span class="hint">(2300 for steel/steel)</span>
</div>
```

#### Range/Constraint Specifications

**What to do:**
1. **Add validation** in the calculate() function
2. **Display warning messages** when out of range
3. **Don't prevent input** - allow override but warn user

**Example:**

PDF shows: "Face width F must satisfy: Fnom ≤ F ≤ Fmax"

HTML Implementation:
```html
<div class="info-box">
    ⚠️ <strong>Face width (F) is a recommended range, not a single value.</strong><br>
    F should satisfy: <strong>F<sub>nom</sub> ≤ F ≤ F<sub>max</sub></strong>
</div>

<!-- Inputs for Fnom, Fmax, F -->

<div class="warning-box" id="F-warning"></div>
```

JavaScript:
```javascript
function validateFaceWidth() {
    const F = getValue('F');
    const Fnom = getValue('Fnom');
    const Fmax = getValue('Fmax');

    const warning = document.getElementById('F-warning');

    if (F !== null && Fnom !== null && Fmax !== null) {
        if (F < Fnom || F > Fmax) {
            warning.innerHTML = `⚠️ Warning: F = ${F.toFixed(4)} is outside recommended range [${Fnom.toFixed(4)}, ${Fmax.toFixed(4)}]`;
            warning.style.display = 'block';
        } else {
            warning.style.display = 'none';
        }
    }
}
```

#### Summary: Handling PDF Elements

Here's a quick reference table for handling different PDF content:

| PDF Element | Implementation Approach | Automatic Calculation? | Key Points |
|-------------|------------------------|----------------------|------------|
| **Equations** | LaTeX + input boxes for all variables | ✅ YES | Add to both Procedure and General Equations tabs |
| **Piecewise Equations** | Conditional logic in calculate() | ✅ YES | Display current formula, add info box explaining conditions |
| **Lookup Tables** | Yellow input + info box with table reference | ❌ NO - Manual only | Don't recreate table, don't calculate, user enters value from PDF |
| **Graphs/Charts** | Yellow input + figure reference + instructions | ❌ NO - Manual only | Don't recreate graph, don't calculate/interpolate, user enters value from PDF |
| **Dropdown Choices** | HTML `<select>` element | ⚠️ User selects | Include all discrete options from PDF, value used in calculations |
| **Procedures/Instructions** | Copy text, format as headers/info boxes | N/A | Use h3 for steps, ordered lists for sub-steps |
| **Worked Examples** | Optional note only | N/A | Direct users to PDF example, don't implement |
| **Constants** | Input with hint text | ⚠️ Optional default | Document typical values, allow override, can pre-fill |
| **Range Constraints** | Validation with warning message | ✅ Check only | Don't prevent input, but warn when out of range |

**Key Principles:**
1. The PDF remains the reference document for tables and graphs
2. **Variables from tables/graphs are NEVER calculated** - they are yellow input-only fields
3. The calculator implements equations and workflow, but complex tables/graphs require manual lookup
4. Yellow background = manual lookup required, no automatic calculation

### Step 2: Set Up HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Calculator Name</title>

    <!-- MathJax for LaTeX rendering -->
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</head>
<body>
    <div id="pdf-panel">
        <iframe src="../documents/your_reference.pdf"></iframe>
    </div>

    <div id="calculator-panel">
        <h1>Calculator Title</h1>
        <p class="subtitle">Brief description</p>

        <div class="controls">
            <button class="btn btn-primary" onclick="saveSession()">💾 Save Session</button>
            <button class="btn btn-secondary" onclick="resetAll()">🔄 Reset All</button>
        </div>

        <!-- Tab buttons -->
        <div class="tabs">
            <button class="tab-button active" onclick="showTab('procedure')">Procedure</button>
            <button class="tab-button" onclick="showTab('equations')">General Equations</button>
            <button class="tab-button" onclick="showTab('variables')">Variables</button>
        </div>

        <!-- Tab content sections -->
        <div id="procedure-tab" class="tab-content active">
            <!-- Procedure content here -->
        </div>

        <div id="equations-tab" class="tab-content">
            <!-- General Equations content here -->
        </div>

        <div id="variables-tab" class="tab-content">
            <!-- Variables table here -->
        </div>
    </div>

    <script>
        // JavaScript code here
    </script>
</body>
</html>
```

### Step 3: Add CSS Styling

Include all necessary CSS for:

1. **Two-panel layout**
```css
body {
    display: flex;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#pdf-panel {
    flex: 1;
    background: #fff;
    border-right: 2px solid #ddd;
    display: flex;
    flex-direction: column;
}

#pdf-panel iframe {
    flex: 1;
    border: none;
    width: 100%;
}

#calculator-panel {
    flex: 1;
    background: #fff;
    overflow-y: auto;
    padding: 20px 40px;
}
```

2. **Tab system**
```css
.tabs {
    display: flex;
    gap: 5px;
    border-bottom: 2px solid #ddd;
    margin-bottom: 20px;
}

.tab-button {
    padding: 10px 20px;
    border: none;
    background: #f0f0f0;
    cursor: pointer;
    font-size: 14px;
    border-radius: 5px 5px 0 0;
    transition: background 0.3s;
}

.tab-button:hover {
    background: #e0e0e0;
}

.tab-button.active {
    background: #3498db;
    color: white;
    font-weight: bold;
}

.tab-content {
    display: none;
}

.tab-content.active {
    display: block;
}
```

3. **Input styling with color coding**
```css
input[type="number"] {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    width: 100px;
    transition: background 0.3s;
}

input[type="number"].computed {
    background-color: #d4edda; /* Green for computed */
    border-color: #28a745;
}

input[type="number"].manual-lookup {
    background-color: #fff3cd; /* Yellow for manual lookup */
    border-color: #ffc107;
}

/* User inputs remain white (default) */
```

4. **Equation blocks**
```css
.equation-block {
    background: #f8f9fa;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 15px;
    border-left: 4px solid #3498db;
}

.equation-latex {
    font-size: 16px;
    margin-bottom: 10px;
}

.equation-inputs {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 10px;
}

.var-input {
    display: flex;
    align-items: center;
    gap: 5px;
}

.var-input label {
    font-weight: 500;
    min-width: 60px;
}
```

5. **Variables table**
```css
.variables-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

.variables-table th,
.variables-table td {
    padding: 10px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

.variables-table th {
    background: #3498db;
    color: white;
    font-weight: bold;
}

.variables-table tr:hover {
    background: #f5f5f5;
}

.var-category {
    background: #e3f2fd !important;
    font-weight: bold;
    color: #1976d2;
}
```

### Step 4: Create the Procedure Tab

The Procedure tab follows the step-by-step workflow from the PDF.

**Template for each step:**
```html
<h3>Step 1: Calculate Variable Name</h3>

<div class="equation-block">
    <div class="equation-latex">
        $$equation\_in\_LaTeX$$
    </div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>Result =</label>
            <input type="number" id="result_var" step="0.0001"
                   oninput="syncAllInstances('result_var'); markAsUser('result_var'); calculate()">
            <span class="unit">units</span>
        </div>
        <span>=</span>
        <div class="var-input">
            <label>Input1 =</label>
            <input type="number" id="input1_eq1" step="0.0001"
                   oninput="syncAllInstances('input1_eq1'); markAsUser('input1_eq1'); calculate()">
            <span class="unit">units</span>
        </div>
        <span>×</span>
        <div class="var-input">
            <label>Input2 =</label>
            <input type="number" id="input2_eq1" step="0.0001"
                   oninput="syncAllInstances('input2_eq1'); markAsUser('input2_eq1'); calculate()">
            <span class="unit">units</span>
        </div>
    </div>
</div>

<div class="divider"></div>
```

**Key points:**
- Each input has a unique ID (e.g., `result_var`, `input1_eq1`, `input2_eq1`)
- All inputs call three functions: `syncAllInstances()`, `markAsUser()`, `calculate()`
- Use step="0.0001" for decimals, step="1" for integers
- LaTeX equations use `$$..$$` delimiters for MathJax

### Step 5: Create the General Equations Tab

This tab contains ALL equations with input boxes for every variable.

**Template for each equation:**
```html
<div class="equation-block">
    <div class="equation-latex">$equation\_in\_LaTeX$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>Var1 (unit)</label>
            <input type="number" id="var1_genEq1" step="any"
                   oninput="syncAllInstances('var1_genEq1'); markAsUser('var1_genEq1'); calculate()">
        </div>
        <div class="var-input">
            <label>Var2 (unit)</label>
            <input type="number" id="var2_genEq1" step="any"
                   oninput="syncAllInstances('var2_genEq1'); markAsUser('var2_genEq1'); calculate()">
        </div>
        <div class="var-input">
            <label>Var3 (unit)</label>
            <input type="number" id="var3_genEq1" step="any"
                   oninput="syncAllInstances('var3_genEq1'); markAsUser('var3_genEq1'); calculate()">
        </div>
    </div>
    <p style="margin-top:10px; color:#666; font-size:0.9em;">Description of what this equation calculates</p>
</div>
```

**Naming convention for General Equations IDs:**
- Use suffix `_genEq1`, `_genEq2`, etc.
- Example: `Pd_genEq1`, `Pd_genEq2` for multiple equations using Pd

### Step 6: Create the Variables Tab

Complete table of all variables with organized categories.

```html
<div id="variables-tab" class="tab-content">
    <h2>All Variables</h2>
    <table class="variables-table">
        <thead>
            <tr>
                <th>Symbol</th>
                <th>Description</th>
                <th>Value</th>
                <th>Unit</th>
            </tr>
        </thead>
        <tbody>
            <!-- Category header -->
            <tr class="var-category">
                <td colspan="4">Category Name</td>
            </tr>

            <!-- Variable row -->
            <tr>
                <td>P<sub>d</sub></td>
                <td>Diametral Pitch</td>
                <td><input type="number" id="var_Pd" step="0.0001"
                           oninput="syncAllInstances('var_Pd'); markAsUser('var_Pd'); calculate()"></td>
                <td>teeth/in</td>
            </tr>

            <!-- Repeat for all variables -->
        </tbody>
    </table>
</div>
```

**Naming convention for Variables tab:**
- All IDs use prefix `var_`
- Example: `var_Pd`, `var_phi`, `var_NP`

---

## Variable Syncing System

The bidirectional syncing system is the most critical feature. It ensures that changing a variable in ANY tab updates ALL instances across ALL tabs.

### Input ID Naming Convention

For each variable (e.g., "Pd"), create IDs for all instances:

1. **Main variable in Procedure tab**: `Pd`
2. **Variables tab**: `var_Pd`
3. **Procedure tab equations**: `Pd_eq1`, `Pd_eq2`, `Pd_eq3`, etc.
4. **General Equations tab**: `Pd_genEq1`, `Pd_genEq2`, etc.

### Variable Instance Map

Create a mapping of all variable instances:

```javascript
const varInstanceMap = {
    'Pd': ['Pd', 'var_Pd', 'Pd_eq1', 'Pd_eq2', 'Pd_genEq1', 'Pd_genEq2'],
    'phi': ['phi', 'var_phi', 'phi_eq1', 'phi_genEq1'],
    'NP': ['NP', 'var_NP', 'NP_eq1', 'NP_eq2', 'NP_genEq1', 'NP_genEq2'],
    'NG': ['NG', 'var_NG', 'NG_eq1', 'NG_eq2', 'NG_genEq1'],
    // ... map ALL variables
};
```

**How to build this map:**
1. List every unique variable name
2. Search the HTML for all input IDs containing that variable
3. Add all IDs to the array for that variable

### Core Syncing Functions

```javascript
// Extract base variable name from any ID format
function getBaseVarName(id) {
    if (id.startsWith('var_')) {
        return id.substring(4);  // Remove 'var_' prefix
    }
    const underscoreIndex = id.indexOf('_eq');
    if (underscoreIndex !== -1) {
        return id.substring(0, underscoreIndex);  // Remove '_eq...' suffix
    }
    const genEqIndex = id.indexOf('_genEq');
    if (genEqIndex !== -1) {
        return id.substring(0, genEqIndex);  // Remove '_genEq...' suffix
    }
    return id;  // Return as-is (main procedure variable)
}

// Sync all instances of a variable across all tabs
function syncAllInstances(changedId) {
    const baseVar = getBaseVarName(changedId);
    const instances = varInstanceMap[baseVar];

    if (!instances) {
        console.warn('No instances found for:', baseVar);
        return;
    }

    const changedInput = document.getElementById(changedId);
    if (!changedInput) return;

    const newValue = changedInput.value;

    // Update all other instances
    instances.forEach(instanceId => {
        if (instanceId !== changedId) {
            const input = document.getElementById(instanceId);
            if (input) {
                input.value = newValue;
            }
        }
    });
}

// Track user-modified fields
const userValues = new Set();

function markAsUser(fieldId) {
    userValues.add(fieldId);
}
```

### Input Handler Pattern

**Every input field must use this exact pattern:**

```html
<input type="number" id="variable_id" step="0.0001"
       oninput="syncAllInstances('variable_id'); markAsUser('variable_id'); calculate()">
```

This ensures:
1. `syncAllInstances()` updates all instances of the variable
2. `markAsUser()` marks the field as user-modified (prevents auto-overwrite)
3. `calculate()` triggers recalculation of all dependent values

---

## Tab System Implementation

### Tab Navigation HTML

```html
<div class="tabs">
    <button class="tab-button active" onclick="showTab('procedure')">Procedure</button>
    <button class="tab-button" onclick="showTab('equations')">General Equations</button>
    <button class="tab-button" onclick="showTab('variables')">Variables</button>
</div>

<div id="procedure-tab" class="tab-content active">
    <!-- Content -->
</div>

<div id="equations-tab" class="tab-content">
    <!-- Content -->
</div>

<div id="variables-tab" class="tab-content">
    <!-- Content -->
</div>
```

### Tab Navigation JavaScript

```javascript
function showTab(tabName) {
    // Hide all tabs
    document.querySelectorAll('.tab-content').forEach(tab => {
        tab.classList.remove('active');
    });

    // Deactivate all tab buttons
    document.querySelectorAll('.tab-button').forEach(btn => {
        btn.classList.remove('active');
    });

    // Show selected tab
    const activeTab = document.getElementById(tabName + '-tab');
    if (activeTab) {
        activeTab.classList.add('active');
    }

    // Activate corresponding button
    event.currentTarget.classList.add('active');

    // Re-render MathJax equations in the newly visible tab
    if (typeof MathJax !== 'undefined' && activeTab) {
        MathJax.typesetPromise([activeTab]).catch((err) => console.log(err));
    }
}
```

---

## Calculation Engine

### Structure

```javascript
function calculate() {
    // 1. Get all input values
    const Pd = getValue('Pd');
    const phi = getValue('phi');
    const NP = getValue('NP');
    const Ko = getValue('Ko');  // From table - NO calculation for this
    const J = getValue('J');    // From graph - NO calculation for this
    // ... get all variables

    // 2. Perform calculations (in dependency order)

    // ❌ DO NOT calculate variables from tables/graphs
    // Ko, J, Kv, etc. are manual lookups - they remain whatever the user entered

    // ✅ Only calculate variables from equations
    // Example calculation
    if (Pd !== null && NP !== null) {
        const DP = NP / Pd;  // Pitch diameter
        setValue('DP', DP);
    }

    // Example with piecewise equation
    if (Pd !== null) {
        let h;
        if (Pd < 20) {
            h = 2.188 / Pd;  // Coarse pitch
        } else {
            h = 2 / Pd + 0.002;  // Fine pitch
        }
        setValue('h', h);
    }

    // Example using manual lookup value in a calculation
    if (Ko !== null && Wt !== null && Kv !== null) {
        // Ko is from a table (no calculation), but we USE it here
        const stress = Wt * Ko * Kv / (F * Pd);
        setValue('stress', stress);
    }

    // 3. Update all synced instances
    syncAllFields();
}
```

**🔴 IMPORTANT: Variables from Tables/Graphs**

In the `calculate()` function:
- **DO NOT** write code to calculate variables that come from tables or graphs
- Simply use `getValue()` to get whatever the user manually entered
- You can USE these values in OTHER calculations
- These variables remain yellow (manual-lookup class), never turn green

**Example:**
```javascript
// ❌ WRONG - Don't calculate Ko (it's from a table)
const Ko = 1.25;  // Don't hardcode
const Ko = calculateFromTable(source, driven);  // Don't interpolate

// ✅ CORRECT - Just get the user's input
const Ko = getValue('Ko');  // User looked it up and entered it
```

// Helper: Get numeric value from input (returns null if empty/invalid)
function getValue(id) {
    const element = document.getElementById(id);
    if (!element) return null;
    const val = parseFloat(element.value);
    return isNaN(val) ? null : val;
}

// Helper: Set value and mark as computed (green background)
function setValue(id, value) {
    if (value !== null && !userValues.has(id)) {
        const element = document.getElementById(id);
        if (element) {
            element.value = value.toFixed(4);  // 4 decimal places
            element.classList.add('computed');  // Green background

            // Sync to all instances
            syncAllInstances(id);
        }
    }
}

// Sync all fields across tabs (run after calculations)
function syncAllFields() {
    // Get list of all main variables (without prefixes/suffixes)
    const mainVars = ['Pd', 'phi', 'NP', 'NG', 'DP', 'DG', /* ... all variables */];

    mainVars.forEach(varId => {
        const mainValue = getValue(varId);
        const mainElement = document.getElementById(varId);
        const isComputed = mainElement && mainElement.classList.contains('computed');

        if (mainValue !== null) {
            // Update Variables tab
            const varTabInput = document.getElementById('var_' + varId);
            if (varTabInput && getValue('var_' + varId) !== mainValue) {
                varTabInput.value = mainValue.toFixed(4);
                if (isComputed) {
                    varTabInput.classList.add('computed');
                }
            }

            // Update all equation instances
            syncAllInstances(varId);
        }
    });
}
```

### Calculation Best Practices

1. **Order calculations by dependency**
   - Calculate independent variables first
   - Then calculate dependent variables

2. **Check for null values**
   - Always check if required inputs are available before calculating
   - Use `if (var1 !== null && var2 !== null)` pattern

3. **Handle piecewise equations**
   - Use conditional logic for equations that change based on input values
   - Example: coarse vs fine pitch (Pd < 20 vs Pd ≥ 20)

4. **Respect user inputs**
   - Check `userValues.has(id)` before overwriting
   - Don't overwrite values the user manually entered

5. **Use appropriate precision**
   - Typically 4 decimal places: `value.toFixed(4)`
   - Adjust based on the nature of the calculation

---

## Session Persistence

### Save Session

```javascript
function saveSession() {
    const sessionData = {};

    // Collect all input values
    document.querySelectorAll('input[type="number"]').forEach(input => {
        if (input.value !== '') {
            sessionData[input.id] = {
                value: input.value,
                isComputed: input.classList.contains('computed')
            };
        }
    });

    // Save to localStorage
    localStorage.setItem('calculator_session', JSON.stringify(sessionData));
    alert('Session saved successfully! ✓');
}
```

### Load Session

```javascript
function loadSession() {
    const savedData = localStorage.getItem('calculator_session');

    if (!savedData) {
        console.log('No saved session found');
        return;
    }

    try {
        const sessionData = JSON.parse(savedData);

        // Restore all values
        Object.keys(sessionData).forEach(id => {
            const input = document.getElementById(id);
            if (input) {
                input.value = sessionData[id].value;
                if (sessionData[id].isComputed) {
                    input.classList.add('computed');
                }
            }
        });

        console.log('Session loaded successfully');
        calculate();  // Recalculate after loading
    } catch (e) {
        console.error('Error loading session:', e);
    }
}

// Auto-load on page load
window.addEventListener('DOMContentLoaded', function() {
    loadSession();
});
```

### Reset All

```javascript
function resetAll() {
    if (!confirm('Reset all values? This cannot be undone.')) {
        return;
    }

    // Clear all inputs
    document.querySelectorAll('input[type="number"]').forEach(input => {
        input.value = '';
        input.classList.remove('computed');
    });

    // Clear user tracking
    userValues.clear();

    // Clear localStorage
    localStorage.removeItem('calculator_session');

    console.log('All values reset');
}
```

---

## Testing and Validation

### 1. Variable Syncing Test

**Test each variable:**
1. Enter a value in the **Procedure tab**
2. Switch to **General Equations tab** - verify the value appears in all instances
3. Switch to **Variables tab** - verify the value appears there
4. Change the value in **Variables tab**
5. Switch back to **Procedure tab** - verify the value updated
6. Switch to **General Equations tab** - verify all instances updated

**What to check:**
- All instances of the variable update simultaneously
- No instances are missed
- Changing value in any tab updates all tabs

### 2. Calculation Test

**Test the calculation flow:**
1. Enter all required input values for an equation
2. Verify the result is calculated automatically
3. Verify the result has a green background (computed class)
4. Manually change the result value
5. Verify it changes to white background (user override)
6. Clear the manual override and verify it recalculates

### 3. Tab Navigation Test

**Test tab switching:**
1. Click each tab button
2. Verify only one tab is visible at a time
3. Verify LaTeX equations render properly in each tab
4. Verify no JavaScript errors in browser console

### 4. Session Persistence Test

**Test save/load:**
1. Enter various values across multiple fields
2. Click "Save Session"
3. Refresh the page (F5)
4. Verify all values are restored
5. Click "Reset All"
6. Verify all values are cleared

### 5. Edge Cases Test

**Test error conditions:**
- Enter invalid values (negative numbers where inappropriate)
- Leave required fields empty - verify dependent calculations don't error
- Enter very large numbers - verify no overflow errors
- Enter very small numbers - verify precision is maintained

### 6. Browser Compatibility Test

**Test in multiple browsers:**
- Chrome/Edge (Chromium)
- Firefox
- Safari (if available)

**What to verify:**
- Layout renders correctly
- PDF displays properly
- LocalStorage works
- No console errors

---

## Common Issues and Solutions

### Issue: Variable not syncing across tabs

**Cause:** Missing ID in `varInstanceMap` or incorrect ID naming

**Solution:**
1. Search the HTML for all input IDs containing the variable
2. Ensure all IDs are listed in `varInstanceMap`
3. Verify ID naming follows the convention (base, var_base, base_eq1, base_genEq1)

### Issue: Calculations not updating

**Cause:** Missing `calculate()` call in input handler or calculation order issue

**Solution:**
1. Verify all inputs have `oninput="syncAllInstances('id'); markAsUser('id'); calculate()"`
2. Check calculation order - calculate dependencies first
3. Add console.log statements to debug calculation flow

### Issue: User values being overwritten

**Cause:** Not checking `userValues.has(id)` before setting value

**Solution:**
```javascript
function setValue(id, value) {
    if (value !== null && !userValues.has(id)) {  // Add this check
        // ... set value
    }
}
```

### Issue: LaTeX not rendering in tabs

**Cause:** MathJax not re-rendering when tab becomes visible

**Solution:**
```javascript
function showTab(tabName) {
    // ... show tab code ...

    // Re-render MathJax
    if (typeof MathJax !== 'undefined' && activeTab) {
        MathJax.typesetPromise([activeTab]).catch((err) => console.log(err));
    }
}
```

### Issue: Green background not showing for computed values

**Cause:** Not adding 'computed' class when setting value

**Solution:**
```javascript
function setValue(id, value) {
    if (value !== null && !userValues.has(id)) {
        const element = document.getElementById(id);
        if (element) {
            element.value = value.toFixed(4);
            element.classList.add('computed');  // Add this line
            syncAllInstances(id);
        }
    }
}
```

---

## Checklist for Creating a New Calculator

Use this checklist when creating a new HTML calculator:

### Planning Phase
- [ ] Obtain source PDF
- [ ] List all variables (name, symbol, unit, description)
- [ ] List all equations in order
- [ ] **🔴 Identify manual lookup values** (yellow inputs - tables/graphs)
  - [ ] Mark which variables come from tables (NO calculation)
  - [ ] Mark which variables come from graphs (NO calculation)
  - [ ] Note the table/figure numbers for each
- [ ] Map calculation dependencies (which equations use which variables)
- [ ] Identify piecewise equations (conditional formulas)
- [ ] **Determine which tabs are needed:**
  - [ ] Procedure tab? (Does PDF have numbered steps?)
  - [ ] General Equations tab? (More than 5 equations?)
  - [ ] Variables tab? (Always include unless < 10 variables)
- [ ] **Catalog PDF elements:**
  - [ ] List all tables/figures that need manual lookup
  - [ ] Note dropdown choices (discrete options)
  - [ ] Identify range constraints/validations
  - [ ] Mark constants with standard values

### HTML Structure
- [ ] Create two-panel layout (PDF left, calculator right)
- [ ] Add header with title and description
- [ ] Add Save/Reset buttons
- [ ] Create tabs based on planning decision (typically 3 tabs)
- [ ] Link to correct PDF in iframe

### Procedure Tab
- [ ] Create step-by-step workflow from PDF
- [ ] Add equation blocks with LaTeX and input boxes
- [ ] Assign unique IDs to all inputs (base, base_eq1, base_eq2, etc.)
- [ ] Add all three functions to oninput handlers
- [ ] Add dividers between steps
- [ ] Add info boxes for manual lookups

### General Equations Tab
- [ ] Create equation blocks for ALL equations
- [ ] Add input boxes for EVERY variable in each equation
- [ ] Use _genEq1, _genEq2 suffix for IDs
- [ ] Add descriptive text for each equation
- [ ] Organize with section headers

### Variables Tab
- [ ] Create complete variables table
- [ ] Use var_ prefix for all IDs
- [ ] Organize into logical categories
- [ ] Include symbol, description, input, and unit columns

### JavaScript - Syncing System
- [ ] Create varInstanceMap with ALL variable instances
- [ ] Implement getBaseVarName() function
- [ ] Implement syncAllInstances() function
- [ ] Implement markAsUser() and userValues set
- [ ] Verify every input has syncAllInstances() call

### JavaScript - Calculation Engine
- [ ] Implement getValue() helper
- [ ] Implement setValue() helper with computed class
- [ ] Write calculate() function with all equations
- [ ] **🔴 CRITICAL: DO NOT calculate variables from tables/graphs** (use getValue() only)
- [ ] Order calculations by dependency
- [ ] Handle piecewise equations with conditionals
- [ ] Verify table/graph variables stay yellow (never turn green)
- [ ] Implement syncAllFields() to update Variables tab

### JavaScript - Tab System
- [ ] Implement showTab() function
- [ ] Add MathJax re-rendering on tab switch
- [ ] Test tab navigation

### JavaScript - Session Persistence
- [ ] Implement saveSession() with localStorage
- [ ] Implement loadSession() with auto-load on page load
- [ ] Implement resetAll() with confirmation
- [ ] Test save/load/reset functionality

### CSS Styling
- [ ] Two-panel responsive layout
- [ ] Tab system styling (active state)
- [ ] Equation blocks with borders
- [ ] Input color coding (white/yellow/green)
- [ ] Variables table styling
- [ ] Button styling
- [ ] Mobile responsiveness

### Testing
- [ ] Test variable syncing across all tabs
- [ ] Test calculations with various inputs
- [ ] Test tab navigation and LaTeX rendering
- [ ] Test session save/load/reset
- [ ] Test with empty inputs (no errors)
- [ ] Test piecewise equations at boundary conditions
- [ ] Test in multiple browsers
- [ ] Check browser console for errors

### Documentation
- [ ] Add comments explaining complex calculations
- [ ] Document any special cases or assumptions
- [ ] Note which values are manual lookups

---

## Quick Reference: Code Templates

### Input Field Template

```html
<input type="number" id="variable_id" step="0.0001"
       oninput="syncAllInstances('variable_id'); markAsUser('variable_id'); calculate()">
```

### Equation Block Template (Procedure Tab)

```html
<div class="equation-block">
    <div class="equation-latex">$$equation$$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>Var (unit)</label>
            <input type="number" id="var_eq1" step="any"
                   oninput="syncAllInstances('var_eq1'); markAsUser('var_eq1'); calculate()">
        </div>
        <!-- More inputs -->
    </div>
</div>
```

### Equation Block Template (General Equations Tab)

```html
<div class="equation-block">
    <div class="equation-latex">$equation$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>Var (unit)</label>
            <input type="number" id="var_genEq1" step="any"
                   oninput="syncAllInstances('var_genEq1'); markAsUser('var_genEq1'); calculate()">
        </div>
        <!-- More inputs -->
    </div>
    <p style="margin-top:10px; color:#666; font-size:0.9em;">Description</p>
</div>
```

### Variable Table Row Template

```html
<tr>
    <td>Symbol</td>
    <td>Description</td>
    <td><input type="number" id="var_variablename" step="0.0001"
               oninput="syncAllInstances('var_variablename'); markAsUser('var_variablename'); calculate()"></td>
    <td>unit</td>
</tr>
```

---

## 🔴 CRITICAL REMINDER: Tables and Graphs

Before finalizing your calculator, verify:

**✅ DO:**
- Create yellow input boxes for variables from tables/graphs
- Add clear "MANUAL LOOKUP REQUIRED" info boxes
- Reference specific table/figure numbers from the PDF
- Use `getValue()` to get user-entered values
- Use these values in OTHER calculations

**❌ DO NOT:**
- Calculate or compute variables from tables/graphs
- Attempt to recreate tables/graphs in HTML
- Interpolate values from tables/graphs
- Use `setValue()` for table/graph variables
- Turn table/graph inputs green (they stay yellow)

**Yellow Background = Manual Lookup Only = NO Automatic Calculation**

This is the most common mistake when creating calculators. Always treat tables and graphs as manual user input, not calculated values.

---

## Summary

Creating an HTML calculator from a PDF involves:

1. **Analyzing** the PDF to identify variables, equations, and workflow
2. **Structuring** HTML with two panels, three tabs, and proper IDs
3. **Styling** with CSS for visual feedback and responsive layout
4. **Implementing** bidirectional variable syncing via varInstanceMap
5. **Coding** calculation logic with proper dependency ordering
6. **Adding** session persistence with localStorage
7. **Testing** thoroughly across all features and browsers

The key innovation is the **bidirectional variable syncing system** that uses:
- Consistent ID naming conventions (base, var_base, base_eq1, base_genEq1)
- Complete varInstanceMap mapping all instances
- syncAllInstances() function called on every input change
- Three-function pattern: syncAllInstances(), markAsUser(), calculate()

This creates a seamless user experience where changing a variable anywhere instantly updates it everywhere, while maintaining calculation integrity and user overrides.

**Reference Examples:**
- `mech_spur_stress.html` - Complete stress analysis with bidirectional syncing
- `mech_helical_stress.html` - Helical gear stress with piecewise equations
- `mech_bevel_stress.html` - Bevel gear stress with bevel-specific factors
- `mech_bevel_geometry.html` - Complex geometry with 25+ synced equations
