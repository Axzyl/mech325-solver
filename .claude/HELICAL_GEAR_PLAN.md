# Helical Gear Calculator - Implementation Plan

## Overview
Create a self-contained HTML file calculator for helical gear design calculations based on the helical design specification PDF. Follow the same structure as `spur_gear_calculator.html`.

## Key Difference: Bidirectional Parameter Relationships
Helical gears involve **normal** and **transverse** parameters that can be converted in either direction. The calculator must handle:
- User can provide EITHER Pd OR Pnd (but not both mandatory)
- User can provide EITHER φt OR φn (but not both mandatory)
- Calculator converts between them based on helix angle ψ

## Input Variables (Given Information)

### Primary Inputs (Always Required)
- **N** - Number of teeth (integer)
- **F** - Face width (in)
- **ψ** (psi) - Helix angle (degrees)

### Secondary Inputs (User chooses one from each pair)
**Diametral Pitch** (choose one):
- **Pd** - Transverse diametral pitch (teeth/in), OR
- **Pnd** - Normal diametral pitch (teeth/in)

**Pressure Angle** (choose one):
- **φt** (phi_t) - Transverse pressure angle (degrees), OR
- **φn** (phi_n) - Normal pressure angle (degrees)

## Calculated Variables (8 total)

### Step 1: Convert Between Normal and Transverse Diametral Pitch
**Bidirectional formulas**:
- **Pnd** = Pd / cos(ψ) (if user provides Pd)
- **Pd** = Pnd × cos(ψ) (if user provides Pnd)

### Step 2: Circular Pitches
- **pt** = π / Pd (transverse circular pitch, in)
- **pn** = pt × cos(ψ) (normal circular pitch, in)

### Step 3: Axial Pitch
- **px** = pt / tan(ψ) (axial pitch, in)

### Step 4: Pitch Diameter
- **D** = N / Pd (pitch diameter, in)

### Step 5: Convert Between Pressure Angles
**Bidirectional formulas**:
- **φn** = tan⁻¹(tan(φt) × cos(ψ)) (if user provides φt)
- **φt** = tan⁻¹(tan(φn) / cos(ψ)) (if user provides φn)

### Step 6: Number of Axial Pitches
- **Nax** = F / px (number of axial pitches, dimensionless)

## HTML Structure

```html
<!DOCTYPE html>
<html>
<head>
    <title>Helical Gear Calculator</title>
    <!-- MathJax for LaTeX -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.2/es5/tex-mml-chtml.js"></script>
    <style>
        /* Same CSS as spur gear calculator */
        /* Two-column layout: PDF left, calculator right */
    </style>
</head>
<body>
    <div class="container">
        <div class="pdf-column">
            <embed src="documents/helical_design_spec.pdf" type="application/pdf">
        </div>
        <div class="calculator-column">
            <h1>Helical Gear Calculator</h1>

            <!-- Given Information Section -->
            <div class="section">
                <h2>Given Information</h2>

                <h3>Primary Inputs</h3>
                <!-- N, F, ψ -->

                <h3>Diametral Pitch (choose one to input)</h3>
                <div class="info-box">
                    <p>💡 Provide EITHER Pd (transverse) OR Pnd (normal). The other will be calculated.</p>
                </div>
                <!-- Input boxes for both Pd and Pnd -->
                <!-- Auto-calculate the other when one is entered -->

                <h3>Pressure Angle (choose one to input)</h3>
                <div class="info-box">
                    <p>💡 Provide EITHER φt (transverse) OR φn (normal). The other will be calculated.</p>
                </div>
                <!-- Input boxes for both φt and φn -->
                <!-- Auto-calculate the other when one is entered -->
            </div>

            <!-- Step 1: Diametral Pitch Conversion -->
            <div class="section">
                <h2>Step 1: Diametral Pitch Conversion</h2>
                <!-- Show both equations -->
                <!-- Highlight which direction is being used -->
            </div>

            <!-- Steps 2-6... -->
        </div>
    </div>

    <script>
        // Variable registry
        const variables = {
            // Primary inputs
            N: null, F: null, psi: null,
            // Secondary inputs (user chooses one from each pair)
            Pd: null, Pnd: null,
            phi_t: null, phi_n: null,
            // Calculated variables
            pt: null, pn: null, px: null,
            D: null,
            Nax: null
        };

        const userValues = new Set(['N', 'F', 'psi']);

        // Track which secondary input user provided
        let userProvidedPd = null; // true = Pd, false = Pnd, null = neither
        let userProvidedPhi = null; // true = phi_t, false = phi_n, null = neither

        function calculate() {
            // Read primary inputs
            const N = parseFloat(document.getElementById('N').value) || null;
            const F = parseFloat(document.getElementById('F').value) || null;
            const psi = parseFloat(document.getElementById('psi').value) || null;

            if (!N || !F || !psi) return;

            const psiRad = psi * Math.PI / 180;

            // Handle bidirectional Pd/Pnd conversion
            const Pd = parseFloat(document.getElementById('Pd').value) || null;
            const Pnd = parseFloat(document.getElementById('Pnd').value) || null;

            if (Pd && userProvidedPd !== false) {
                // User provided Pd, calculate Pnd
                userProvidedPd = true;
                variables.Pd = Pd;
                if (!userValues.has('Pnd')) {
                    variables.Pnd = Pd / Math.cos(psiRad);
                    updateField('Pnd', variables.Pnd);
                }
            } else if (Pnd && userProvidedPd !== true) {
                // User provided Pnd, calculate Pd
                userProvidedPd = false;
                variables.Pnd = Pnd;
                if (!userValues.has('Pd')) {
                    variables.Pd = Pnd * Math.cos(psiRad);
                    updateField('Pd', variables.Pd);
                }
            }

            // Similar logic for phi_t and phi_n
            const phi_t = parseFloat(document.getElementById('phi_t').value) || null;
            const phi_n = parseFloat(document.getElementById('phi_n').value) || null;

            if (phi_t && userProvidedPhi !== false) {
                userProvidedPhi = true;
                variables.phi_t = phi_t;
                if (!userValues.has('phi_n')) {
                    const phi_t_rad = phi_t * Math.PI / 180;
                    const phi_n_rad = Math.atan(Math.tan(phi_t_rad) * Math.cos(psiRad));
                    variables.phi_n = phi_n_rad * 180 / Math.PI;
                    updateField('phi_n', variables.phi_n);
                }
            } else if (phi_n && userProvidedPhi !== true) {
                userProvidedPhi = false;
                variables.phi_n = phi_n;
                if (!userValues.has('phi_t')) {
                    const phi_n_rad = phi_n * Math.PI / 180;
                    const phi_t_rad = Math.atan(Math.tan(phi_n_rad) / Math.cos(psiRad));
                    variables.phi_t = phi_t_rad * 180 / Math.PI;
                    updateField('phi_t', variables.phi_t);
                }
            }

            // Now calculate other values if we have Pd
            if (variables.Pd) {
                // Step 2: Circular pitches
                if (!userValues.has('pt')) {
                    variables.pt = Math.PI / variables.Pd;
                    updateField('pt', variables.pt);
                }
                if (!userValues.has('pn')) {
                    variables.pn = variables.pt * Math.cos(psiRad);
                    updateField('pn', variables.pn);
                }

                // Step 3: Axial pitch
                if (!userValues.has('px')) {
                    variables.px = variables.pt / Math.tan(psiRad);
                    updateField('px', variables.px);
                }

                // Step 4: Pitch diameter
                if (!userValues.has('D')) {
                    variables.D = N / variables.Pd;
                    updateField('D', variables.D);
                }

                // Step 6: Number of axial pitches
                if (!userValues.has('Nax') && variables.px) {
                    variables.Nax = F / variables.px;
                    updateField('Nax', variables.Nax);
                }
            }

            // Sync duplicate fields
            syncFields();

            // Update direction indicators
            updateDirectionIndicators();
        }

        function updateDirectionIndicators() {
            // Show which conversion direction is active
            const pdIndicator = document.getElementById('pd-direction');
            const phiIndicator = document.getElementById('phi-direction');

            if (userProvidedPd === true) {
                pdIndicator.textContent = '→ Calculating Pnd from Pd';
            } else if (userProvidedPd === false) {
                pdIndicator.textContent = '→ Calculating Pd from Pnd';
            } else {
                pdIndicator.textContent = '(Provide one to calculate the other)';
            }

            if (userProvidedPhi === true) {
                phiIndicator.textContent = '→ Calculating φn from φt';
            } else if (userProvidedPhi === false) {
                phiIndicator.textContent = '→ Calculating φt from φn';
            } else {
                phiIndicator.textContent = '(Provide one to calculate the other)';
            }
        }

        function markAsUser(varId) {
            userValues.add(varId);

            // Handle mutual exclusivity for Pd/Pnd
            if (varId === 'Pd') {
                userProvidedPd = true;
            } else if (varId === 'Pnd') {
                userProvidedPd = false;
            }

            // Handle mutual exclusivity for phi_t/phi_n
            if (varId === 'phi_t') {
                userProvidedPhi = true;
            } else if (varId === 'phi_n') {
                userProvidedPhi = false;
            }
        }

        // LocalStorage save/load
        function saveSession() {
            localStorage.setItem('helical_gear_state', JSON.stringify({
                variables,
                userValues: Array.from(userValues),
                userProvidedPd,
                userProvidedPhi
            }));
        }

        function loadSession() {
            const saved = localStorage.getItem('helical_gear_state');
            if (saved) {
                const state = JSON.parse(saved);
                Object.assign(variables, state.variables);
                userValues.clear();
                state.userValues.forEach(v => userValues.add(v));
                userProvidedPd = state.userProvidedPd;
                userProvidedPhi = state.userProvidedPhi;
                // Populate fields
                for (let key in variables) {
                    if (variables[key] !== null) {
                        updateField(key, variables[key]);
                    }
                }
            }
        }

        function resetAll() {
            localStorage.removeItem('helical_gear_state');
            location.reload();
        }
    </script>
</body>
</html>
```

## Special Considerations

### 1. Bidirectional Conversions
**Critical Implementation Detail**: The calculator must intelligently handle which parameter the user provides.

**Approach**:
- Track which input user provided first using `userProvidedPd` and `userProvidedPhi` flags
- When user enters Pd, automatically calculate Pnd and gray it out
- When user enters Pnd, automatically calculate Pd and gray it out
- Same logic for φt and φn
- Allow user to override by marking the other field as user-provided
- Show visual indicator of which direction conversion is happening

### 2. Trigonometric Conversions
Step 5 requires careful handling:
- φn = tan⁻¹(tan(φt) × cos(ψ))
- φt = tan⁻¹(tan(φn) / cos(ψ))

**JavaScript implementation**:
```javascript
// Converting φt to φn
const phi_t_rad = phi_t * Math.PI / 180;
const psi_rad = psi * Math.PI / 180;
const phi_n_rad = Math.atan(Math.tan(phi_t_rad) * Math.cos(psi_rad));
const phi_n_deg = phi_n_rad * 180 / Math.PI;

// Converting φn to φt
const phi_n_rad = phi_n * Math.PI / 180;
const psi_rad = psi * Math.PI / 180;
const phi_t_rad = Math.atan(Math.tan(phi_n_rad) / Math.cos(psi_rad));
const phi_t_deg = phi_t_rad * 180 / Math.PI;
```

### 3. Visual Direction Indicators
Add indicator text/icons to show which conversion is active:

```html
<div class="direction-indicator" id="pd-direction">
    → Calculating Pnd from Pd
</div>
```

Update dynamically based on which input user provided.

### 4. Precision
- Number of teeth (N): 0 decimal places (integer)
- Angles (ψ, φt, φn): 4 decimal places
- Diametral pitches (Pd, Pnd): 4 decimal places
- Circular pitches (pt, pn, px): 4 decimal places
- Diameter (D): 4 decimal places
- Face width (F): 4 decimal places
- Number of axial pitches (Nax): 2 decimal places

### 5. Variable Synchronization
Variables that appear in multiple equations:
- Pd, Pnd (appear in multiple steps)
- ψ (used throughout)
- pt, pn (calculated in Step 2, used elsewhere)
- Sync all duplicate fields when any is updated

### 6. Input Validation
- Ensure N is a positive integer
- Ensure ψ is between 0° and 90° (exclusive)
- Ensure all pitches and dimensions are positive
- If user provides BOTH Pd and Pnd, check if they're consistent with ψ
- If inconsistent, show warning

## LaTeX Equation Rendering

Use MathJax with proper LaTeX notation:

```latex
Given:
N = \text{number of teeth}
F = \text{face width (in)}
\psi = \text{helix angle (deg)}

Step 1 (Bidirectional):
P_{nd} = \frac{P_d}{\cos(\psi)} \quad \text{OR} \quad P_d = P_{nd} \cos(\psi)

Step 2:
p_t = \frac{\pi}{P_d}
p_n = p_t \cos(\psi)

Step 3:
p_x = \frac{p_t}{\tan(\psi)}

Step 4:
D = \frac{N}{P_d}

Step 5 (Bidirectional):
\phi_n = \tan^{-1}(\tan(\phi_t) \cos(\psi))
\quad \text{OR} \quad
\phi_t = \tan^{-1}\left(\frac{\tan(\phi_n)}{\cos(\psi)}\right)

Step 6:
N_{ax} = \frac{F}{p_x}
```

## Testing Values
Use typical helical gear values:
- N = 24
- F = 2.0
- ψ = 30°
- Pd = 8 (transverse)
- φt = 20°

Expected results:
- Pnd ≈ 9.238
- pt ≈ 0.3927
- pn ≈ 0.3400
- px ≈ 0.6806
- D = 3.0
- φn ≈ 17.475°
- Nax ≈ 2.94

## File Location
Save as: `helical_gear_calculator.html`
PDF reference: `documents/helical_design_spec.pdf`

## Additional UI Enhancements

### Option 1: Tabbed Interface
Instead of separate HTML files, could create ONE multi-gear calculator with tabs:
- Tab 1: Spur Gears
- Tab 2: Bevel Gears
- Tab 3: Helical Gears

### Option 2: Separate Files
Maintain three separate HTML files:
- `spur_gear_calculator.html` (already exists)
- `bevel_gear_calculator.html` (to be created)
- `helical_gear_calculator.html` (to be created)

**Recommendation**: Start with separate files (Option 2) for simplicity, then can combine later if desired.
