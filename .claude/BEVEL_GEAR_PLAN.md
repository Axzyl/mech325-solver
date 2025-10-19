# Bevel Gear Calculator - Implementation Plan

## Overview
Create a self-contained HTML file calculator for bevel gear design calculations based on the bevel design specification PDF. Follow the same structure as `spur_gear_calculator.html`.

## Input Variables (Given Information)
- **Pd** - Diametral Pitch (teeth/in)
- **NP** - Number of teeth on Pinion (integer)
- **NG** - Number of teeth on Gear (integer)
- **φ** - Pressure Angle (degrees)

## Calculated Variables (17 total)

### Step 1: Gear Ratio and Pitch Diameters
- **mG** = NG / NP (gear ratio)
- **D** = NG / Pd (pitch diameter of gear, in)
- **d** = NP / Pd (pitch diameter of pinion, in)

### Step 2: Pitch Cone Angles
- **γ** = tan⁻¹(NP / NG) (pitch cone angle of pinion, degrees)
- **Γ** = tan⁻¹(NG / NP) (pitch cone angle of gear, degrees)

### Step 3: Cone Distance
- **AO** = 0.5 × D / sin(Γ) (cone distance, in)

### Step 4: Face Width (SPECIAL CASE - RANGE)
**Important**: Face width is specified as a RANGE, not a single value.

Two formulas given:
- **Fnom** = AO / 3 (nominal face width)
- **Fmax** = 10 / Pd (maximum face width)

**Constraint**: Fnom ≤ F ≤ Fmax

**Implementation approach**:
- Calculate both Fnom and Fmax
- Display as: "Fnom ≤ F ≤ Fmax"
- Show both values but let user choose F within range
- Make F a USER INPUT (not auto-calculated) that validates against the range
- Show warning if user's F < Fnom (too small) or F > Fmax (too large)

### Step 5: Mean Cone Distance
- **Am** = AO - (F / 2) (mean cone distance, in)

### Step 6: Mean Circular Pitch
- **pm** = π × Am / (0.5 × NG) (mean circular pitch, in)

### Step 7: Whole Depth and Clearance
**Piecewise formula based on Pd**:

If Pd < 20 (Coarse pitch):
- **h** = 2.188 / Pd (whole depth, in)
- **c** = 0.188 / Pd (clearance, in)

If Pd ≥ 20 (Fine pitch):
- **h** = (2 / Pd) + 0.002 (whole depth, in)
- **c** = 0.002 (clearance, in)

### Step 8: Mean Addendum and Dedendum
If Pd < 20:
- **hm** = h × cos(Γ) - (c / 2) (mean whole depth, in)
- **c1** = c / 2 (half clearance, in)

If Pd ≥ 20:
- **hm** = h × cos(Γ) - c (mean whole depth, in)
- **c1** = c (clearance for calculation, in)

Then:
- **am** = hm / 2 (mean addendum, in)
- **bm** = hm - am (mean dedendum, in)

### Step 9: Addendum and Dedendum Angles
- **aG** = am / AO (gear addendum, in)
- **aP** = am / AO (pinion addendum, in)
- **bG** = bm / AO (gear dedendum, in)
- **bP** = bm / AO (pinion dedendum, in)

### Step 10: Face Angle and Outside Diameters
- **δG** = Γ + aG (gear face angle, degrees)
- **δP** = γ + aP (pinion face angle, degrees)
- **aOG** = am + (F / 2) × sin(δG) (gear outside addendum, in)
- **aOP** = am + (F / 2) × sin(δP) (pinion outside addendum, in)
- **DO** = D + 2 × aOG × cos(Γ) (gear outside diameter, in)
- **dO** = d + 2 × aOP × cos(γ) (pinion outside diameter, in)

## HTML Structure

```html
<!DOCTYPE html>
<html>
<head>
    <title>Bevel Gear Calculator</title>
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
            <embed src="documents/bevel_design_spec.pdf" type="application/pdf">
        </div>
        <div class="calculator-column">
            <h1>Bevel Gear Calculator</h1>

            <!-- Given Information Section -->
            <div class="section">
                <h2>Given Information</h2>
                <!-- Inputs: Pd, NP, NG, φ -->
            </div>

            <!-- Step 1: Gear Ratio -->
            <div class="section">
                <h2>Step 1: Calculate Gear Ratio and Pitch Diameters</h2>
                <!-- Equations with input boxes for mG, D, d, plus all variables in equations -->
            </div>

            <!-- Steps 2-10... -->

            <!-- Step 4 SPECIAL HANDLING -->
            <div class="section">
                <h2>Step 4: Determine Face Width Range</h2>
                <div class="info-box">
                    <p>⚠️ Face width (F) should satisfy: Fnom ≤ F ≤ Fmax</p>
                </div>
                <!-- Show Fnom calculation -->
                <!-- Show Fmax calculation -->
                <!-- User input for F with validation -->
                <!-- Show warning if F < Fnom or F > Fmax -->
            </div>
        </div>
    </div>

    <script>
        // Variable registry
        const variables = {
            // Input variables
            Pd: null, NP: null, NG: null, phi: null,
            // Calculated variables
            mG: null, D: null, d: null,
            gamma: null, Gamma: null,
            AO: null,
            F: null, Fnom: null, Fmax: null,
            Am: null, pm: null,
            h: null, c: null, hm: null, c1: null,
            am: null, bm: null,
            aG: null, aP: null, bG: null, bP: null,
            deltaG: null, deltaP: null,
            aOG: null, aOP: null,
            DO: null, dO: null
        };

        const userValues = new Set(['Pd', 'NP', 'NG', 'phi', 'F']);

        function calculate() {
            // Read inputs
            const Pd = parseFloat(document.getElementById('Pd').value) || null;
            const NP = parseFloat(document.getElementById('NP').value) || null;
            const NG = parseFloat(document.getElementById('NG').value) || null;
            const phi = parseFloat(document.getElementById('phi').value) || null;

            if (!Pd || !NP || !NG || !phi) return;

            // Step 1
            if (!userValues.has('mG')) {
                variables.mG = NG / NP;
                updateField('mG', variables.mG);
            }
            if (!userValues.has('D')) {
                variables.D = NG / Pd;
                updateField('D', variables.D);
            }
            if (!userValues.has('d')) {
                variables.d = NP / Pd;
                updateField('d', variables.d);
            }

            // Step 2 - Trigonometry
            if (!userValues.has('gamma')) {
                variables.gamma = Math.atan(NP / NG) * (180 / Math.PI);
                updateField('gamma', variables.gamma);
            }
            if (!userValues.has('Gamma')) {
                variables.Gamma = Math.atan(NG / NP) * (180 / Math.PI);
                updateField('Gamma', variables.Gamma);
            }

            // Continue for all steps...
            // Step 4 special handling for F range
            // Step 7-8 piecewise formulas based on Pd < 20

            // Sync duplicate fields
            syncFields();
        }

        function validateFaceWidth() {
            const F = parseFloat(document.getElementById('F').value) || null;
            const Fnom = variables.Fnom;
            const Fmax = variables.Fmax;
            const warning = document.getElementById('F-warning');

            if (F && Fnom && Fmax) {
                if (F < Fnom) {
                    warning.textContent = `⚠️ Warning: F (${F.toFixed(4)}) is below recommended minimum Fnom (${Fnom.toFixed(4)})`;
                    warning.style.display = 'block';
                } else if (F > Fmax) {
                    warning.textContent = `⚠️ Warning: F (${F.toFixed(4)}) exceeds recommended maximum Fmax (${Fmax.toFixed(4)})`;
                    warning.style.display = 'block';
                } else {
                    warning.style.display = 'none';
                }
            } else {
                warning.style.display = 'none';
            }
        }

        // LocalStorage save/load
        function saveSession() { /* ... */ }
        function loadSession() { /* ... */ }
        function resetAll() { /* ... */ }
    </script>
</body>
</html>
```

## Special Considerations

### 1. Trigonometric Functions
- tan⁻¹ (arctan) → `Math.atan()`
- sin → `Math.sin()`
- cos → `Math.cos()`
- **IMPORTANT**: JavaScript trig functions use RADIANS
- Convert: radians = degrees × (π / 180)
- Convert back: degrees = radians × (180 / π)

### 2. Face Width Range Validation
- Calculate both Fnom and Fmax
- Display recommended range clearly: Fnom ≤ F ≤ Fmax
- Allow user to input F
- Show warning (not error) if F < Fnom or F > Fmax
- Make warning dismissible but visible

### 3. Piecewise Formulas (Coarse vs Fine Pitch)
- Same as spur gear: Pd < 20 vs Pd ≥ 20
- Step 7: Different formulas for h and c
- Step 8: Different formulas for hm and c1
- Use blue info box to show which formulas are active
- Display active formula set based on current Pd value

### 4. Precision
- Angles: 4 decimal places
- Diameters/lengths: 4 decimal places
- Gear ratio (mG): 4 decimal places
- Integer inputs (NP, NG): 0 decimal places

### 5. Variable Synchronization
Many variables appear in multiple equations. Same approach as spur gear calculator:
- Pd, NP, NG, φ appear throughout
- γ, Γ, D, d used in multiple steps
- Sync all duplicate fields when any is updated

## LaTeX Equation Rendering

Use MathJax with proper LaTeX notation:

```latex
Step 1:
m_G = \frac{N_G}{N_P}
D = \frac{N_G}{P_d}
d = \frac{N_P}{P_d}

Step 2:
\gamma = \tan^{-1}\left(\frac{N_P}{N_G}\right)
\Gamma = \tan^{-1}\left(\frac{N_G}{N_P}\right)

Step 3:
A_O = \frac{0.5 D}{\sin(\Gamma)}

Step 4:
F_{nom} = \frac{A_O}{3}
F_{max} = \frac{10}{P_d}
F_{nom} \leq F \leq F_{max}

Step 5:
A_m = A_O - \frac{F}{2}

Step 6:
p_m = \frac{\pi A_m}{0.5 N_G}

Step 7 (Coarse):
h = \frac{2.188}{P_d}
c = \frac{0.188}{P_d}

Step 7 (Fine):
h = \frac{2}{P_d} + 0.002
c = 0.002

Step 8 (Coarse):
h_m = h \cos(\Gamma) - \frac{c}{2}
c_1 = \frac{c}{2}

Step 8 (Fine):
h_m = h \cos(\Gamma) - c
c_1 = c

Then:
a_m = \frac{h_m}{2}
b_m = h_m - a_m

Step 9:
a_G = \frac{a_m}{A_O}, \quad a_P = \frac{a_m}{A_O}
b_G = \frac{b_m}{A_O}, \quad b_P = \frac{b_m}{A_O}

Step 10:
\delta_G = \Gamma + a_G, \quad \delta_P = \gamma + a_P
a_{OG} = a_m + \frac{F}{2} \sin(\delta_G)
a_{OP} = a_m + \frac{F}{2} \sin(\delta_P)
D_O = D + 2 a_{OG} \cos(\Gamma)
d_O = d + 2 a_{OP} \cos(\gamma)
```

## Testing Values
Use example from PDF or typical values:
- Pd = 10
- NP = 15
- NG = 45
- φ = 20

Expected intermediate results:
- mG = 3
- γ ≈ 18.43°
- Γ ≈ 71.57°

## File Location
Save as: `bevel_gear_calculator.html`
PDF reference: `documents/bevel_design_spec.pdf`
