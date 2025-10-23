# HTML Calculator Implementation Guide

This document provides step-by-step technical instructions for **implementing** standalone HTML gear calculators from a **completed specification document**. These calculators feature bidirectional variable syncing across multiple tabs, real-time calculations, and session persistence.

## Prerequisites

Before using this guide, you **must** have a completed specification created using:
- `CALCULATOR_SPECIFICATION_GUIDE.md` - Instructions for analyzing the PDF
- `CALCULATOR_SPECIFICATION_TEMPLATE.md` - Template filled out with all variables, equations, and procedures

**Do not proceed with implementation until the specification is complete.**

This guide focuses on the **technical implementation** (HTML/CSS/JavaScript) assuming you already know:
- What variables exist and their source types (User Input/Equation/Table/Graph/Dropdown)
- What equations to implement
- What tabs to include
- What procedure steps to follow

## Table of Contents
1. [Overview](#overview)
2. [File Structure](#file-structure)
3. [Core Features](#core-features)
4. [Step-by-Step Implementation Process](#step-by-step-implementation-process)
   - [Step 1: Review Your Specification](#step-1-review-your-specification)
   - [Step 2: Understanding HTML Implementation for Different Content Types](#step-2-understanding-html-implementation-for-different-content-types)
   - [Step 3: Set Up HTML Structure](#step-3-set-up-html-structure)
   - [Step 4: Add CSS Styling](#step-4-add-css-styling)
   - [Step 5: Create the Procedure Tab](#step-5-create-the-procedure-tab)
   - [Step 6: Create the General Equations Tab](#step-6-create-the-general-equations-tab)
   - [Step 7: Create the Variables Tab](#step-7-create-the-variables-tab)
5. [Variable Syncing System](#variable-syncing-system)
6. [Tab System Implementation](#tab-system-implementation)
7. [Calculation Engine](#calculation-engine)
8. [Session Persistence](#session-persistence)
9. [Testing and Validation](#testing-and-validation)
10. [Common Issues and Solutions](#common-issues-and-solutions)
11. [Implementation Checklist](#implementation-checklist)

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
- **Left panel**: Embedded PDF reference document with interactive crosshairs
- **Right panel**: Interactive calculator with scrolling

### 2. PDF Page Navigation Feature

**Purpose:** Allow users to jump directly to the page containing a referenced table or graph with a single click.

**How it works:**
- When a table/graph is referenced (e.g., "Look up from Table 7-1"), provide a clickable link
- Clicking the link jumps the PDF viewer to the specific page
- Uses PDF URL fragment syntax: `file.pdf#page=5`

**Important:** Use `<embed>` tag (not `<iframe>`) with id="pdf" for reliable PDF page navigation.

**Implementation:**
```html
<!-- PDF embed with id="pdf" -->
<embed src="../../documents/your_pdf.pdf" type="application/pdf" id="pdf">

<!-- Info box with page navigation link -->
<div class="info-box">
    📊 <strong>MANUAL LOOKUP</strong> - Look up K<sub>s</sub> from <a href="#" onclick="goToPage(3)">Table 7-1</a> based on machine type
</div>
```

**JavaScript function:**
```javascript
function goToPage(page) {
    const embed = document.getElementById("pdf");
    const currSrc = embed.src.split('#')[0];

    // Force complete reload by clearing and resetting with delay
    embed.src = '';
    setTimeout(() => {
        embed.src = `${currSrc}#page=${page}`;
    }, 50);
}
```

**Important:**
- Use `embed.src` (property) not `embed.getAttribute('src')` - the `.src` property returns the fully resolved absolute URL
- The function clears the embed first (`src = ''`) then reloads with the page parameter after a brief delay
- This forces a complete reload, which is necessary for reliable page navigation with `<embed>` tags

**Note:** The PDF will briefly disappear and reload when navigating to a new page. This is expected behavior to force the browser to load the correct page.

**CSS styling:**
```css
/* Style both iframe and embed for compatibility */
.pdf-column embed,
.pdf-column iframe {
    width: 100%;
    height: 90vh;
    border: none;
    border-radius: 8px;
}

/* PDF navigation link styling */
.warning-box a,
.info-box a {
    color: #0066cc;
    text-decoration: underline;
    font-weight: 600;
    cursor: pointer;
}

.warning-box a:hover,
.info-box a:hover {
    color: #004499;
}
```

**Required in specification:**
- All table/graph references MUST include page numbers
- Format: "Table 7-1 (page 3)", "Figure 14-2 (page 11)"

### 3. PDF Crosshairs Feature

**Purpose:** Help users read precise values from tables and graphs by displaying crosshair guide lines.

**Key Design Decision - Toggle Button Approach:**
- The feature uses a toggle button to enable/disable crosshairs functionality
- **Why?** The click overlay that captures crosshair clicks blocks PDF scrolling/zooming
- **Solution:** Overlay is hidden by default, shown only when user explicitly enables crosshairs
- **Result:** PDF is fully interactive by default, crosshairs available when needed

**Functionality:**
- **Toggle button** to enable/disable the crosshairs feature
- **When DISABLED (default):**
  - Click overlay is hidden
  - PDF is fully interactive (scroll, zoom, text selection, link clicking all work)
  - Button shows gray with text "🎯 Enable Crosshairs"
- **When ENABLED:**
  - Click overlay becomes visible (transparent layer above PDF)
  - Crosshair cursor (➕) appears when hovering over PDF
  - Clicking PDF toggles crosshairs on/off at click position
  - Button shows green with text "🎯 Disable Crosshairs"
- **Lines stay static** and extend to the edges of the PDF viewer for precise reading
- **Useful for** reading values from tables, graphs, and aligning with equations

### 3. Three-Tab System
- **Procedure Tab**: Step-by-step workflow following the PDF
- **General Equations Tab**: All equations with editable input boxes
- **Variables Tab**: Complete table of all variables with inputs

### 3. Bidirectional Variable Syncing
- Changing a variable in any tab updates all other instances across all tabs
- Implemented via `varInstanceMap` and `syncAllInstances()` function

### 4. Input Fields
- **All input fields are editable** regardless of whether they are user inputs, calculated values, or manual lookups
- **All input fields have the same styling** (no color coding by source type)
- **Bidirectional solving** - you can edit any variable and the calculator will recalculate dependent values

### 5. Session Persistence
- Save current state to browser LocalStorage
- Load previous session on page reload
- Reset all values to empty state

### 6. Real-Time Calculation
- Calculations trigger automatically when any input changes
- Uses `oninput` event handlers on all input fields

---

## Step-by-Step Implementation Process

### Step 1: Review Your Specification

Before you start coding, have your completed specification document open. You should know:

1. **All variables** - from the Variables Table in your specification
   - Variable symbols, names, descriptions, units
   - **Source Type** for each: User Input, Equation, Table, Graph, or Dropdown
2. **All equations** - from the Equations Table
   - LaTeX formulas
   - Input and output variables for each equation
3. **Procedure steps** - from the Procedure Table (if applicable)
   - Action Type for each step
   - Details and dependencies
4. **Tab structure decision** - which tabs to include
5. **Special notes** - piecewise equations, validation rules, dropdown options

**📋 Reference your specification continuously while implementing.**

---

### Step 2: Understanding HTML Implementation for Different Content Types

Your specification will indicate different **Source Types** for variables. Here's how to implement each type in HTML:

---

**🔴 CRITICAL RULE: Tables and Graphs**

When your specification shows **Source Type: Table** or **Source Type: Graph**:
- ✅ Create an **input box** for the associated variable(s)
- ✅ Add an **info box** directing users to the specific table/figure in the PDF
- ❌ **DO NOT** implement any automatic calculation for this variable
- ❌ **DO NOT** attempt to recreate the table/graph in HTML
- ❌ **DO NOT** write code to interpolate or compute values from the table/graph

**The user MUST manually look up the value from the PDF and enter it.**

---

#### Implementing Equations (Source Type: Equation)

When your specification shows **Source Type: Equation**, implement as follows:

**What to do:**
1. **Copy the LaTeX formula** from your specification's Equations Table
2. **Create input boxes** for:
   - The result variable (left side of equation)
   - ALL variables on the right side of the equation
3. **Add to Procedure Tab** in the step where it's used
4. **Add to General Equations Tab** in the appropriate section

**Example:**

Specification shows: `EQ1: D = N / Pd`

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

If your specification notes a piecewise equation (formula changes based on conditions):

Specification shows:
```
EQ7: h = 2.188 / Pd (if Pd < 20)
     h = 2 / Pd + 0.002 (if Pd ≥ 20)
Notes: Piecewise - coarse vs fine pitch
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

#### Implementing Lookup Tables (Source Type: Table)

**IMPORTANT: Tables require manual lookup only - no automatic calculation.**

When your specification shows **Source Type: Table**, implement as follows:

**What to do:**
1. **Don't recreate the table** in HTML (too complex, users need PDF reference)
2. **Create an INPUT field** for the user to enter the looked-up value
3. **Add an info box** directing users to the specific table in the PDF
4. **Reference the table number** from your specification's "Source Reference" column
5. **DO NOT calculate this variable** - user must look it up manually
6. **Exclude from automatic calculations** - leave the calculate() function empty for this variable

**Example:**

Specification shows:
```
Variable: Ko
Source Type: Table
Source Reference: Table 9-1
```

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
    <input type="number" id="Ko" step="0.01"
           oninput="syncAllInstances('Ko'); markAsUser('Ko'); calculate()">
    <span class="unit">(from Table 9-1 - enter manually)</span>
</div>
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

**Key Point:** Variables from tables are **input-only**. The info box signals to users that they must manually look up this value from the PDF. The calculator does NOT compute these values automatically.

#### Implementing Graphs and Charts (Source Type: Graph)

**IMPORTANT: Graphs require manual lookup only - no automatic calculation.**

When your specification shows **Source Type: Graph**, implement as follows:

**What to do:**
1. **Don't recreate the graph** in HTML
2. **Create an INPUT field** for the user to enter the looked-up value
3. **Reference the figure number** from your specification
4. **Provide step-by-step reading instructions** in an info box
5. **DO NOT calculate this variable** - user must read it from the graph manually
6. **Exclude from automatic calculations** - leave the calculate() function empty for this variable

**Example:**

Specification shows:
```
Variable: J
Source Type: Graph
Source Reference: Figure 14-2
```

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
    <input type="number" id="J" step="0.0001"
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

**Key Point:** Variables from graphs/charts are **input-only**. The info box signals to users that they must manually read this value from the PDF graph. The calculator does NOT compute or interpolate these values automatically.

#### Implementing Dropdown Selections (Source Type: Dropdown)

When your specification shows **Source Type: Dropdown**, implement as follows:

**What to do:**
1. **Use HTML select/dropdown** for discrete choices
2. **List all options** from your specification's "Dropdown Options" section
3. **Include numeric values** that will be used in calculations

**Example:**

Specification shows:
```
Variable: Kmb
Source Type: Dropdown
Options:
  - 1.0 - Both gears straddle-mounted
  - 1.1 - One gear straddle-mounted
  - 1.25 - Neither gear straddle-mounted
```

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

#### Implementing User Input Variables (Source Type: User Input)

When your specification shows **Source Type: User Input**, implement as a standard white input field:

**Example:**

Specification shows:
```
Variable: Pd
Source Type: User Input
```

HTML Implementation:
```html
<div class="var-input">
    <label>P<sub>d</sub> =</label>
    <input type="number" id="Pd" step="0.0001"
           oninput="syncAllInstances('Pd'); markAsUser('Pd'); calculate()">
    <span class="unit">teeth/in</span>
</div>
```

**No special styling** - default white background indicates user input.

#### Implementing Procedures/Instructions

Your specification's Procedure Table includes text instructions. Implement them as:

**What to do:**
1. **Copy the text** from the "Description" column
2. **Format as headers** (h3 for steps, h4 for sub-steps)
3. **Use info boxes** for important notes or warnings
4. **Use ordered lists** for multi-step instructions

**Example:**

Specification shows:
```
Step 3: Calculate Bending Stress
Action Type: Equation
Details: Use EQ5. Note: stress cycle factor from Figure 14-14
```

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

#### Implementing Constants and Standard Values

If your specification lists standard values or constants:

**What to do:**
1. **Create input field** (allow manual override)
2. **Add hint text** showing the standard value
3. **Document the source** in parentheses

**Example:**

Specification shows:
```
Variable: Cp
Source Type: User Input
Notes: Typical value 2300 for steel/steel gears
```

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

#### Implementing Range/Constraint Validations

If your specification lists validation rules or range constraints:

**What to do:**
1. **Add validation** in the calculate() function
2. **Display warning messages** when out of range
3. **Don't prevent input** - allow override but warn user

**Example:**

Specification shows:
```
Validation Rule: F must satisfy Fnom ≤ F ≤ Fmax
```

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

#### Summary: Implementing Different Source Types

Quick reference table based on your specification's Source Type column:

| Source Type | HTML Implementation | Automatic Calculation? | Key Points |
|-------------|---------------------|----------------------|------------|
| **User Input** | Standard input field | ❌ NO - User enters | Starting values, all editable |
| **Equation** | LaTeX + input boxes for ALL variables | ✅ YES - Auto-calculated | Add to Procedure and General Equations tabs, but user can override |
| **Table** | Input field + info box | ❌ NO - Manual only | Don't calculate, reference table number from spec |
| **Graph** | Input field + info box | ❌ NO - Manual only | Don't calculate/interpolate, reference figure number |
| **Dropdown** | HTML `<select>` element | ❌ NO - User selects | List all options from spec |

**Key Implementation Principles:**
1. Your specification tells you the **Source Type** - this determines the HTML implementation
2. **Table and Graph variables are NEVER calculated** - they are input-only fields with info boxes
3. **Equation variables** get calculated in the `calculate()` function but remain editable
4. **All input fields are editable** regardless of source type
5. **In equation blocks, show ALL variables with input boxes**, not just the result

---

### Step 3: Set Up HTML Structure

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

        <!-- IMPORTANT: Control buttons MUST be at the top, immediately after title -->
        <div class="button-group">
            <button class="save-btn" onclick="saveSession()">💾 Save Session</button>
            <button class="reset-btn" onclick="resetAll()">🔄 Reset All</button>
            <button class="toggle-crosshairs-btn" id="toggle-crosshairs-btn" onclick="toggleCrosshairsFeature()">🎯 Enable Crosshairs</button>
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

### Step 4: Add CSS Styling

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

3. **Toggle Crosshairs Button**
```css
/* Button styling */
.toggle-crosshairs-btn {
    background: #6c757d;  /* Gray when disabled (default) */
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    font-size: 1em;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.toggle-crosshairs-btn:hover {
    background: #5a6268;  /* Darker gray on hover */
}

/* Active state - when crosshairs are enabled */
.toggle-crosshairs-btn.active {
    background: #28a745;  /* Green when enabled */
}

.toggle-crosshairs-btn.active:hover {
    background: #218838;  /* Darker green on hover */
}
```

**Important Notes:**
- The `.active` class is toggled by the `toggleCrosshairsFeature()` function
- Color coding provides instant visual feedback:
  - Gray = Crosshairs DISABLED, PDF fully interactive
  - Green = Crosshairs ENABLED, click overlay active
- Button text also changes: "Enable Crosshairs" ↔ "Disable Crosshairs"

4. **Input styling**
```css
input[type="number"] {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    width: 100px;
}

/* All inputs have the same styling - no color coding */
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

### Step 5: Create the Procedure Tab

The Procedure tab follows the step-by-step workflow from your specification's Procedure Table.

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

### Step 6: Create the General Equations Tab

This tab contains ALL equations from your specification's Equations Table, with input boxes for every variable.

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

### Step 7: Create the Variables Tab

Complete table of all variables from your specification's Variables Table, with organized categories.

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

## PDF Crosshairs Feature

The PDF crosshairs feature helps users read values from tables and graphs by displaying vertical and horizontal guide lines at the click position.

---

### Quick Reference Summary

**What:** Toggle-enabled crosshair guide lines for precise PDF measurements

**Why:** Users need to read exact values from tables/graphs, but also need full PDF interactivity (scrolling/zooming)

**How:**
1. Toggle button enables/disables a transparent click overlay
2. When enabled: Overlay captures clicks to place/remove crosshairs
3. When disabled: Overlay hidden, PDF fully interactive

**Key Components:**
- **Toggle Button** - Gray (disabled) / Green (enabled)
- **Click Overlay** - Transparent div, captures clicks when visible
- **SVG Crosshairs** - Red dashed lines, pointer-events: none
- **JavaScript** - `initCrosshairs()` + `toggleCrosshairsFeature()`

**User Flow:**
```
Default → PDF scrollable
Click "Enable" → Overlay appears → Crosshair cursor
Click PDF → Crosshairs appear/disappear
Click "Disable" → Overlay hidden → PDF scrollable again
```

**Implementation Checklist:**
- [ ] Add toggle button to HTML (with Save/Reset)
- [ ] Add click-overlay div (initially hidden)
- [ ] Add SVG with crosshair lines
- [ ] Implement toggleCrosshairsFeature() function
- [ ] Start with overlay hidden: `clickOverlay.style.display = 'none'`
- [ ] Test: Enable → Place crosshairs → Disable → PDF scrolls

---

### HTML Structure for PDF Panel

```html
<div id="pdf-panel">
    <div class="pdf-container">
        <embed src="../../documents/your_pdf.pdf" type="application/pdf" id="pdf-embed">
        <div class="click-overlay" id="click-overlay"></div>
        <svg id="pdf-crosshairs" class="crosshairs-overlay">
            <line id="crosshair-vertical" class="crosshair-line" x1="0" y1="0" x2="0" y2="100%" />
            <line id="crosshair-horizontal" class="crosshair-line" x1="0" y1="0" x2="100%" y2="0" />
        </svg>
    </div>
</div>
```

### CSS for Crosshairs

```css
.pdf-container {
    position: relative;
    width: 100%;
    height: 100%;
}

.pdf-container embed {
    width: 100%;
    height: 90vh;
    border: none;
    border-radius: 8px;
}

.click-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 5;
    cursor: crosshair;
}

.crosshairs-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;  /* Don't block PDF scrolling/interaction */
    z-index: 10;
}

.crosshair-line {
    stroke: #ff0000;
    stroke-width: 2;
    stroke-dasharray: 5, 5;
    opacity: 0;
    transition: opacity 0.2s;
}

.crosshair-line.active {
    opacity: 0.8;
}
```

### JavaScript for Crosshairs

```javascript
let crosshairsActive = false;
let crosshairsFeatureEnabled = false;

// Initialize crosshairs functionality
function initCrosshairs() {
    const pdfContainer = document.querySelector('.pdf-container');
    const clickOverlay = document.getElementById('click-overlay');
    const svg = document.getElementById('pdf-crosshairs');
    const verticalLine = document.getElementById('crosshair-vertical');
    const horizontalLine = document.getElementById('crosshair-horizontal');

    if (!pdfContainer || !clickOverlay || !svg || !verticalLine || !horizontalLine) return;

    // Start with overlay hidden (PDF fully interactive by default)
    clickOverlay.style.display = 'none';

    // Listen for clicks on the click overlay - toggle crosshairs on/off
    clickOverlay.addEventListener('click', function(e) {
        if (!crosshairsActive) {
            // SHOW CROSSHAIRS
            // Get click position relative to container
            const rect = pdfContainer.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            // Update line positions (static - don't move with mouse)
            verticalLine.setAttribute('x1', x);
            verticalLine.setAttribute('x2', x);
            verticalLine.setAttribute('y2', rect.height);

            horizontalLine.setAttribute('y1', y);
            horizontalLine.setAttribute('y2', y);
            horizontalLine.setAttribute('x2', rect.width);

            // Show lines
            verticalLine.classList.add('active');
            horizontalLine.classList.add('active');
            crosshairsActive = true;
        } else {
            // HIDE CROSSHAIRS
            verticalLine.classList.remove('active');
            horizontalLine.classList.remove('active');
            crosshairsActive = false;
        }
    });
}

// Toggle crosshairs feature on/off
function toggleCrosshairsFeature() {
    const clickOverlay = document.getElementById('click-overlay');
    const toggleBtn = document.getElementById('toggle-crosshairs-btn');
    const verticalLine = document.getElementById('crosshair-vertical');
    const horizontalLine = document.getElementById('crosshair-horizontal');

    if (!clickOverlay || !toggleBtn) return;

    // Toggle the enabled state
    crosshairsFeatureEnabled = !crosshairsFeatureEnabled;

    if (crosshairsFeatureEnabled) {
        // ENABLE CROSSHAIRS FEATURE
        clickOverlay.style.display = 'block';  // Show overlay (captures clicks)
        toggleBtn.textContent = '🎯 Disable Crosshairs';  // Update button text
        toggleBtn.classList.add('active');  // Turn button green
    } else {
        // DISABLE CROSSHAIRS FEATURE
        clickOverlay.style.display = 'none';  // Hide overlay (PDF interactive)
        toggleBtn.textContent = '🎯 Enable Crosshairs';  // Update button text
        toggleBtn.classList.remove('active');  // Turn button gray

        // Also hide any active crosshairs when disabling
        if (crosshairsActive) {
            verticalLine.classList.remove('active');
            horizontalLine.classList.remove('active');
            crosshairsActive = false;
        }
    }
}

/**
 * HOW THE TOGGLE MECHANISM WORKS:
 *
 * 1. Default State (Page Load):
 *    - clickOverlay.style.display = 'none'
 *    - PDF is fully interactive (scrolling, zooming work)
 *    - Button is gray, shows "Enable Crosshairs"
 *
 * 2. User Clicks "Enable Crosshairs":
 *    - clickOverlay.style.display = 'block'
 *    - Transparent overlay now covers PDF
 *    - Overlay captures all clicks (for placing crosshairs)
 *    - PDF scrolling is blocked (trade-off for crosshair placement)
 *    - Button turns green, shows "Disable Crosshairs"
 *    - User can now click PDF to place/remove crosshairs
 *
 * 3. User Clicks "Disable Crosshairs":
 *    - clickOverlay.style.display = 'none'
 *    - Overlay hidden, no longer captures clicks
 *    - PDF is fully interactive again
 *    - Any visible crosshairs are removed
 *    - Button turns gray, shows "Enable Crosshairs"
 *
 * KEY INSIGHT: The overlay must be hidden to allow PDF interaction.
 * This is why we use a toggle button instead of always-on crosshairs.
 */

// Call on page load
window.addEventListener('DOMContentLoaded', function() {
    initCrosshairs();
    loadSession();
});
```

### Usage

#### Step-by-Step User Workflow

**Scenario 1: Using Crosshairs to Read a Table Value**

1. **Open the calculator** - PDF is fully scrollable/zoomable by default
2. **Scroll to the table** you need to read from (PDF works normally)
3. **Click "Enable Crosshairs" button**
   - Button turns green
   - Crosshair cursor (➕) appears when hovering over PDF
4. **Click on the table** at the intersection of the row/column you need
   - Red crosshair lines appear, extending to edges
   - Lines stay static, making it easy to trace to axis values
5. **Read the value** using the aligned crosshairs
6. **Click on PDF again** to remove crosshairs (or leave them for reference)
7. **Click "Disable Crosshairs" button**
   - Button turns gray
   - Crosshairs disappear
   - PDF becomes fully interactive again
8. **Continue scrolling** or zooming the PDF normally

**Scenario 2: Quick Toggle Workflow**

1. **Need to measure something?**
   - Click "Enable Crosshairs" → Click PDF → Read value
2. **Done measuring?**
   - Click "Disable Crosshairs" → Resume normal PDF use
3. **Need to measure again?**
   - Click "Enable Crosshairs" → Click new position → Read value

#### How It Works Internally

**When Crosshairs are DISABLED (default state):**
- `clickOverlay.style.display = 'none'`
- Click overlay is completely hidden
- All clicks go directly to the PDF embed element
- PDF's native controls work: scroll, zoom, text selection, links
- Button is gray, shows "🎯 Enable Crosshairs"

**When Crosshairs are ENABLED:**
- `clickOverlay.style.display = 'block'`
- Transparent overlay covers the PDF
- Overlay captures all click events (for placing crosshairs)
- PDF scrolling is blocked (overlay is in front)
- Button is green, shows "🎯 Disable Crosshairs"
- Clicking PDF toggles crosshair visibility at click position

#### Benefits of Toggle Button Approach

✅ **No interference by default** - PDF works exactly like a normal embedded PDF
✅ **Opt-in feature** - Users only enable when they need precise measurements
✅ **Clear state indication** - Button color shows whether feature is active
✅ **Flexible workflow** - Easy to switch between measuring and browsing
✅ **Clean UX** - No permanent overlay blocking interaction

This feature is especially useful for:
- Reading values from tables (align with row/column)
- Reading graph coordinates
- Measuring distances in diagrams
- Aligning with specific points on charts and figures

---

### Implementation Notes and Best Practices

#### Critical Requirements

**1. Click Overlay Must Start Hidden**
```javascript
// In initCrosshairs() function:
clickOverlay.style.display = 'none';  // CRITICAL - Start hidden
```
- **Why?** Ensures PDF is interactive on page load
- **Common mistake:** Forgetting to hide overlay initially

**2. Toggle Function Must Update 3 Things**
```javascript
function toggleCrosshairsFeature() {
    // 1. Toggle the boolean state
    crosshairsFeatureEnabled = !crosshairsFeatureEnabled;

    // 2. Show/hide the overlay
    clickOverlay.style.display = crosshairsFeatureEnabled ? 'block' : 'none';

    // 3. Update button appearance
    toggleBtn.textContent = crosshairsFeatureEnabled ? '🎯 Disable' : '🎯 Enable';
    toggleBtn.classList.toggle('active');
}
```

**3. Clean Up on Disable**
- Always remove any active crosshairs when disabling the feature
- Prevents orphaned crosshairs from staying visible
- See implementation in code example above

#### Common Issues and Solutions

**Issue 1: PDF scrolling doesn't work**
- **Cause:** Click overlay is visible (not hidden)
- **Solution:** Ensure `clickOverlay.style.display = 'none'` on page load and when disabled

**Issue 2: Crosshairs don't appear when clicking**
- **Cause:** Overlay is hidden when it should be visible
- **Solution:** Check that toggle button properly shows overlay when enabled

**Issue 3: Button doesn't change color**
- **Cause:** `.active` class not being toggled
- **Solution:** Use `classList.add('active')` when enabling, `classList.remove('active')` when disabling

**Issue 4: Crosshairs stay visible after disabling**
- **Cause:** Forgetting to clear crosshairs in disable logic
- **Solution:** Add cleanup code that removes `.active` class from crosshair lines

#### Styling Considerations

**Button Placement:**
- Place in button-group with Save/Reset buttons
- Users should see it immediately (top of page, after title)
- Consistent positioning across all calculator pages

**Color Choices:**
- Gray (#6c757d) for disabled: Neutral, indicates inactive state
- Green (#28a745) for enabled: Positive action, clearly different from gray
- Avoid red/yellow for toggle (reserved for reset/warning buttons)

**Crosshair Line Styling:**
- Red (#ff0000) lines: High contrast, easy to see against most PDF backgrounds
- Dashed pattern: Distinguishes from PDF content
- 80% opacity: Visible but not overwhelming
- 2px width: Thin enough for precision, thick enough to see

#### Testing Checklist

When implementing crosshairs, test these scenarios:

- [ ] **Default state:** Open page → PDF scrolls normally
- [ ] **Enable:** Click button → Button turns green → Crosshair cursor appears
- [ ] **Place crosshairs:** Click PDF → Red lines appear at exact position
- [ ] **Remove crosshairs:** Click PDF again → Lines disappear
- [ ] **Reposition:** Click PDF third time → Lines appear at new position
- [ ] **Disable:** Click button → Button turns gray → Lines disappear → PDF scrolls normally
- [ ] **Edge case:** Enable → Place crosshairs → Disable → Verify lines removed
- [ ] **Edge case:** Enable → Disable (without placing) → No errors

#### Browser Compatibility

The crosshairs feature uses standard DOM APIs and should work in:
- ✅ Chrome/Edge (Chromium) - Tested
- ✅ Firefox - Works
- ✅ Safari - Works (may have slight styling differences)

**Fallback:** If SVG doesn't render, crosshairs simply won't appear (graceful degradation).

#### Performance Considerations

- **Minimal overhead:** Feature only active when explicitly enabled
- **No continuous tracking:** Lines update only on click, not on mouse move
- **Small footprint:** SVG overlay is lightweight, doesn't impact PDF rendering

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

// Helper: Set value (if not user-modified)
function setValue(id, value) {
    if (value !== null && !userValues.has(id)) {
        const element = document.getElementById(id);
        if (element) {
            element.value = value.toFixed(4);  // 4 decimal places

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

        if (mainValue !== null) {
            // Update Variables tab
            const varTabInput = document.getElementById('var_' + varId);
            if (varTabInput && getValue('var_' + varId) !== mainValue) {
                varTabInput.value = mainValue.toFixed(4);
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
            sessionData[input.id] = input.value;
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
                input.value = sessionData[id];
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
3. Manually change the result value
4. Enter a different input value
5. Verify the manually-changed result does NOT recalculate (user override respected)
6. Delete the manual override value and verify it recalculates

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

### Issue: Calculated values being overwritten by user inputs

**Cause:** Not checking userValues before setting calculated value

**Solution:**
```javascript
function setValue(id, value) {
    if (value !== null && !userValues.has(id)) {  // Check userValues first
        const element = document.getElementById(id);
        if (element) {
            element.value = value.toFixed(4);
            syncAllInstances(id);
        }
    }
}
```

---

## Implementation Checklist

Use this checklist when implementing a new HTML calculator from a completed specification:

### Prerequisites
- [ ] Specification document is complete (all variables, equations, procedures documented)
- [ ] Tab structure decision is made
- [ ] All manual lookups (tables/graphs) are identified
- [ ] Piecewise equations are documented
- [ ] Validation rules are noted

### HTML Structure
- [ ] Create two-panel layout (PDF left, calculator right)
- [ ] Add PDF container with relative positioning
- [ ] Add click-overlay div for capturing clicks (z-index: 5, initially hidden)
- [ ] Add PDF crosshairs SVG overlay with vertical/horizontal lines (z-index: 10, pointer-events: none)
- [ ] Add header with title and description
- [ ] Add Save/Reset/Toggle Crosshairs buttons at TOP (immediately after title, before tabs)
- [ ] Create tabs based on planning decision (typically 3 tabs)
- [ ] Link to correct PDF in embed element

### Procedure Tab
- [ ] Create step-by-step workflow from PDF
- [ ] Add equation blocks with LaTeX and input boxes FOR ALL VARIABLES (not just result)
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
- [ ] Implement setValue() helper (respects userValues)
- [ ] Write calculate() function with all equations
- [ ] **🔴 CRITICAL: DO NOT calculate variables from tables/graphs** (use getValue() only)
- [ ] Order calculations by dependency
- [ ] Handle piecewise equations with conditionals
- [ ] Verify table/graph variables are not auto-calculated
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

### JavaScript - PDF Page Navigation
- [ ] Implement goToPage(page) function (simple version using template literals)
- [ ] Use `<embed>` tag (not `<iframe>`) with id="pdf" for the PDF
- [ ] Add page navigation links to all table/graph references using format: `<a href="#" onclick="goToPage(N)">Table X-Y</a>`
- [ ] Verify all page numbers are correct from PDF
- [ ] Test navigation links jump to correct pages within the embedded PDF
- [ ] Update CSS to style both iframe and embed elements

### JavaScript - PDF Crosshairs
- [ ] Implement initCrosshairs() function
- [ ] Add transparent click-overlay div above PDF (starts HIDDEN)
- [ ] Add single click event listener to click-overlay that toggles crosshairs
- [ ] In click handler: check if crosshairsActive
  - [ ] If false: calculate position, show lines, set crosshairsActive = true
  - [ ] If true: hide lines, set crosshairsActive = false
- [ ] Implement toggleCrosshairsFeature() function
  - [ ] Toggle crosshairsFeatureEnabled boolean
  - [ ] Show/hide click-overlay based on state
  - [ ] Update button text and styling
  - [ ] Clear any active crosshairs when disabling
- [ ] Ensure SVG has pointer-events: none so it doesn't block clicks to overlay
- [ ] Lines should stay static at click position (no mousemove tracking)
- [ ] Test enable button shows overlay (crosshair cursor appears)
- [ ] Test clicking PDF shows/hides crosshairs
- [ ] Test disable button hides overlay and restores PDF interaction

### CSS Styling
- [ ] Two-panel responsive layout with PDF container
- [ ] PDF page navigation links (style .warning-box a and .info-box a with blue underlined links)
- [ ] Click overlay styling (transparent, z-index: 5, crosshair cursor)
- [ ] PDF crosshairs SVG overlay (z-index: 10, pointer-events: none)
- [ ] Crosshair line styling (red, dashed, opacity transitions)
- [ ] Tab system styling (active state)
- [ ] Equation blocks with borders
- [ ] Input styling (consistent for all inputs)
- [ ] Variables table styling
- [ ] Button styling (save-btn, reset-btn, toggle-crosshairs-btn at top)
- [ ] Toggle button active state (gray when off, green when on)
- [ ] Mobile responsiveness

### Testing
- [ ] Test variable syncing across all tabs
- [ ] Test calculations with various inputs
- [ ] Test tab navigation and LaTeX rendering
- [ ] Test session save/load/reset
- [ ] Test PDF crosshairs workflow:
  - [ ] Default state: PDF is fully scrollable/interactive
  - [ ] Click "Enable Crosshairs" button
    - [ ] Button turns green
    - [ ] Crosshair cursor appears over PDF
  - [ ] Click on PDF → crosshairs appear at click position
  - [ ] Lines stay static and don't move
  - [ ] Click on PDF again → crosshairs disappear
  - [ ] Click on PDF third time → crosshairs appear at new position
  - [ ] Click "Disable Crosshairs" button
    - [ ] Button turns gray
    - [ ] Any active crosshairs disappear
    - [ ] PDF becomes fully interactive again (scrolling works)
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

## 🔴 CRITICAL REMINDERS

Before finalizing your calculator, verify:

### Tables and Graphs

**✅ DO:**
- Create input boxes for variables from tables/graphs
- Add clear "MANUAL LOOKUP REQUIRED" info boxes
- Reference specific table/figure numbers from the PDF
- Use `getValue()` to get user-entered values
- Use these values in OTHER calculations

**❌ DO NOT:**
- Calculate or compute variables from tables/graphs
- Attempt to recreate tables/graphs in HTML
- Interpolate values from tables/graphs
- Use `setValue()` for table/graph variables

This is the most common mistake when creating calculators. Always treat tables and graphs as manual user input, not calculated values.

### Equation Blocks

**✅ DO:**
- Show ALL variables with input boxes in equation blocks
- Allow bidirectional solving (any variable can be edited)
- Make all inputs editable regardless of source type

**❌ DO NOT:**
- Only show the result variable with an input box
- Make any inputs readonly
- Apply different styling to inputs based on source type

---

## Summary

Implementing an HTML calculator from a completed specification involves:

1. **Reviewing** your specification to understand variables, equations, and tab structure
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
