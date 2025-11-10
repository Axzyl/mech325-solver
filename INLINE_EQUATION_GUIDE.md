# Inline Equation Implementation Guide

## Overview

This guide clarifies the correct implementation of inline equations in the M325 calculator suite, based on user feedback and the updated fizz_blb.html template.

## Key Principle

**ALL inputs within inline equations (.equation-inputs) are editable with white backgrounds.**
**Color coding (white/yellow/green) ONLY applies to input-row format inputs.**

## Two Input Formats

### 1. Input-Row Format (`.input-row`)

Used for primary data entry and manual lookups. **Uses color coding:**

```html
<div class="input-row">
    <label>Shaft Diameter (d) [in]:</label>
    <input type="number" id="d" step="any" oninput="syncAllInstances('d'); markAsUser('d'); calculate()">
</div>

<div class="input-row">
    <label>Max Allowable Pressure (P<sub>max,allow</sub>) [psi]:</label>
    <input type="number" id="P_max_allow" class="lookup" step="any" oninput="...">
    <span class="hint">From Table 12-12</span>
</div>

<div class="input-row">
    <label>Bearing Inside Diameter (D) [in]:</label>
    <input type="number" id="D_computed" class="computed" readonly>
</div>
```

**Color Scheme:**
- White background = User input (no class)
- Yellow background = Manual table/graph lookup (`.lookup` class)
- Green background = Auto-calculated (`.computed` class + `readonly`)

### 2. Inline Equation Format (`.equation-inputs`)

Used to show mathematical relationships. **ALL inputs are editable, ALL are white:**

```html
<div class="equation-block">
    <div class="equation-latex">$$D = d + c_d$$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>D</label>
            <input type="number" id="D" step="any" oninput="syncAllInstances('D'); markAsUser('D'); calculate()">
        </div>
        <span>=</span>
        <div class="var-input">
            <label>d</label>
            <input type="number" id="d_eq1" step="any" oninput="syncAllInstances('d_eq1'); markAsUser('d_eq1'); calculate()">
        </div>
        <span>+</span>
        <div class="var-input">
            <label>c<sub>d</sub></label>
            <input type="number" id="c_d_eq1" step="any" oninput="syncAllInstances('c_d_eq1'); markAsUser('c_d_eq1'); calculate()">
        </div>
    </div>
</div>
```

**Important:**
- ❌ NO `readonly` attribute
- ❌ NO `.computed` class
- ❌ NO `.lookup` class
- ✅ ALL inputs editable
- ✅ ALL inputs white background
- ✅ Every variable gets its own textbox

## Complex Equation Example

For equations with many terms, include ALL variables:

```html
<div class="equation-block">
    <div class="equation-latex">$$L_{min} \geq \frac{720 f_s n_d F N}{J \bar{h} C_R (T_f - T_\infty)}$$</div>
    <div class="equation-inputs">
        <div class="var-input">
            <label>L<sub>min</sub></label>
            <input type="number" id="L_min" step="any" oninput="syncAllInstances('L_min'); markAsUser('L_min'); calculate()">
        </div>
        <span>=</span>
        <span>720 ×</span>
        <div class="var-input">
            <label>f<sub>s</sub></label>
            <input type="number" id="f_s_eq" step="any" oninput="syncAllInstances('f_s_eq'); markAsUser('f_s_eq'); calculate()">
        </div>
        <span>×</span>
        <div class="var-input">
            <label>n<sub>d</sub></label>
            <input type="number" id="n_d_eq" step="any" oninput="syncAllInstances('n_d_eq'); markAsUser('n_d_eq'); calculate()">
        </div>
        <span>×</span>
        <div class="var-input">
            <label>F</label>
            <input type="number" id="F_eq" step="any" oninput="syncAllInstances('F_eq'); markAsUser('F_eq'); calculate()">
        </div>
        <span>×</span>
        <div class="var-input">
            <label>N</label>
            <input type="number" id="N_eq" step="any" oninput="syncAllInstances('N_eq'); markAsUser('N_eq'); calculate()">
        </div>
        <span>/</span>
        <div class="var-input">
            <label>J</label>
            <input type="number" id="J_eq" step="any" oninput="syncAllInstances('J_eq'); markAsUser('J_eq'); calculate()">
        </div>
        <span>/</span>
        <div class="var-input">
            <label>h̄</label>
            <input type="number" id="h_bar_eq" step="any" oninput="syncAllInstances('h_bar_eq'); markAsUser('h_bar_eq'); calculate()">
        </div>
        <span>/</span>
        <div class="var-input">
            <label>C<sub>R</sub></label>
            <input type="number" id="C_R_eq" step="any" oninput="syncAllInstances('C_R_eq'); markAsUser('C_R_eq'); calculate()">
        </div>
        <span>/</span>
        <div class="var-input">
            <label>ΔT</label>
            <input type="number" id="T_rise_eq" step="any" oninput="syncAllInstances('T_rise_eq'); markAsUser('T_rise_eq'); calculate()">
        </div>
    </div>
</div>
```

## CSS Requirements

### Remove Color Styling from .var-input

```css
.var-input input, .var-input select {
    width: 120px;
    padding: 8px;
    border: 2px solid #ced4da;
    border-radius: 6px;
    font-size: 1em;
    text-align: center;
    background: white;  /* Always white! */
}

/* NO .var-input input.computed {} */
/* NO .var-input input.lookup {} */
```

### Keep Color Styling for .input-row

```css
.input-row input.computed {
    background: #e8f5e9;
    border-color: #4caf50;
}

.input-row input.lookup {
    background: #fff9c4;
    border-color: #fbc02d;
}
```

## JavaScript Requirements

### Variable Instance Mapping

Map all instances of each variable for synchronization:

```javascript
const varInstanceMap = {
    'd': ['d', 'd_eq1', 'd_eq2', 'd_eq3'],
    'F': ['F', 'F_eq', 'F_eq2', 'F_eq3'],
    'n_d': ['n_d', 'n_d_eq', 'n_d_eq2'],
    'L': ['L', 'L_eq1', 'L_eq2'],
    'D': ['D', 'D_eq1', 'D_eq2'],
    // ... all variables
};
```

### Enhanced setValue Function

Update setValue() to sync all instances:

```javascript
function setValue(id, value) {
    const elem = document.getElementById(id);
    if (elem) {
        elem.value = value.toFixed(4);
        // Sync to all instances of this variable
        syncAllInstances(id);
    }
}
```

## Why This Design?

1. **Flexibility**: Users can solve for ANY variable, not just the "result"
2. **Clarity**: No confusion about which inputs are editable
3. **Consistency**: Same white background for all inline equation inputs
4. **Visual Separation**:
   - Input-rows = data entry (color-coded by source)
   - Inline equations = mathematical relationships (all editable, all white)

## Common Mistake to Avoid

❌ **WRONG:**
```html
<div class="equation-inputs">
    <div class="var-input">
        <label>Result</label>
        <input type="number" id="result" class="computed" readonly> <!-- WRONG! -->
    </div>
</div>
```

✅ **CORRECT:**
```html
<div class="equation-inputs">
    <div class="var-input">
        <label>Result</label>
        <input type="number" id="result" step="any" oninput="syncAllInstances('result'); markAsUser('result'); calculate()">
    </div>
</div>
```

## Reference Implementation

See `calculators/bearings/fizz_blb.html` for a complete, correct implementation.

Key equations to examine:
- Line 454: L_min (complex multi-variable equation)
- Line 522: D = d + c_d (simple equation)
- Line 548: A_p = D × L (intermediate equation)
- Line 569: P = (n_d × F) / A_p (division equation)
