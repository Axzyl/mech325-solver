# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **comprehensive gear design calculator suite** containing two complementary systems:

### 1. Streamlit Interactive Solver (Python)
A Streamlit-based interactive spur gear specification calculator that renders equations as interactive cards where users can input values, and the system automatically solves for unknowns using symbolic math. All variables are globally synchronized across the UI.

**Key Reference**: The complete equation set and variable definitions come from `documents/spur_design_spec.pdf`. The detailed implementation plan is in `instructions/spur_gear_interactive_plan.md`.

### 2. HTML Standalone Calculators (Browser-based)
A collection of 14 standalone HTML calculators (no installation required) covering all major gear types. Each calculator displays the reference PDF on the left and an interactive calculator on the right. Features include:
- Automatic calculation as you type
- Three-tier input system: user inputs (white), manual lookups from tables/graphs (yellow), auto-calculated (green)
- Session persistence via LocalStorage (save/load)
- MathJax for equation rendering
- Works offline once loaded

**HTML Calculators Available (in `calculators/` folder):**
- `calculators/index.html` - Landing page with links to all calculators
- `calculators/mech_spur_geometry.html` - Basic spur gear geometry
- `calculators/mech_helical_geometry.html` - Helical gear geometry
- `calculators/mech_bevel_geometry.html` - Bevel gear geometry
- `calculators/mech_gear_forces.html` - Force analysis
- `calculators/mech_spur_stress.html` - Spur gear stress analysis
- `calculators/mech_helical_stress.html` - Helical stress analysis
- `calculators/mech_bevel_stress.html` - Bevel stress analysis
- `calculators/mech_rack_calculator.html` - Rack and pinion kinematics
- `calculators/mech_worm_calculator.html` - Worm gear design
- `calculators/fizz_spur.html` - Complete 40-step spur gear design (Fizz guide)
- `calculators/fizz_helical.html` - Helical design (steps 1-13) → warps to spur (steps 32-40)
- `calculators/fizz_bevel.html` - Bevel design (steps 1-26) → warps to spur (steps 35-40)
- `calculators/fizz_worm.html` - Complete 21-step worm gear design
- `calculators/fizz_rack.html` - 6-step rack and pinion design

**Note:** All HTML calculators require the corresponding PDF files in the `documents/` folder to display reference material.

## Architecture

### Module Structure
```
app/
  app.py                # Streamlit entry point
  engine/
    variables.py        # Variable model, registry, validators
    equations.py        # Equation registry and symbolic forms
    solver.py           # Dependency graph + automatic solving
    units.py            # Unit conversion (Pd↔m, degrees↔radians)
  ui/
    cards.py            # Equation card layout
    drawers.py          # Global variable and assumption drawers
  data/
    presets.json        # Example scenarios for testing
  tests/
    test_equations.py   # Unit tests for equations and solver
```

### Core Components

**Engine Layer** (`app/engine/`):
- `variables.py`: Pydantic models for 21 gear variables (Pd, phi, Np, Ng, etc.) with domain validation (R+, Z+, angle), precision, and lock state
- `equations.py`: 17 symbolic equations using SymPy, including piecewise rules for coarse/fine pitch (Pd < 20 vs Pd ≥ 20)
- `solver.py`: Dependency graph builder that iteratively solves equations when all-but-one variables are known. Detects overdetermined (conflicts) and underdetermined (missing inputs) states
- `units.py`: Handles Pd ↔ m (module) conversion using formula Pd = 25.4 / m

**UI Layer** (`app/ui/`):
- `cards.py`: Renders each equation as a card with LaTeX display and input boxes, shows variable source (User/Computed), includes "Explain" button for calculation trace
- `drawers.py`: Global variable table (editable, searchable, lockable) and assumptions panel (coarse/fine pitch toggle)

**Data Persistence** (`app/data/`):
- JSON-based save/load for sessions with schema versioning
- Preset scenarios for acceptance testing

## Development Commands

### Environment Setup
```bash
# Activate virtual environment (Windows)
.venv\Scripts\activate

# Install dependencies
pip install streamlit sympy pint pydantic numpy
```

### Running the Streamlit App
```bash
streamlit run app/app.py
```
Browser will open at `http://localhost:8501`

### Testing HTML Calculators
```bash
# Simply open any HTML file in a browser
# Windows: start calculators\index.html
# macOS: open calculators/index.html
# Linux: xdg-open calculators/index.html

# Or drag and drop any .html file from calculators/ folder into your browser
```

### Testing Python Code
```bash
# Ensure virtual environment is activated first
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pytest tests/
pytest tests/test_equations.py -v  # verbose output
pytest tests/test_equations.py::test_name -v  # run single test
```

## Critical Implementation Details

### Piecewise Equation Handling
Equations for `b` (dedendum) and `c` (clearance) have different formulas based on pitch:
- **Coarse** (Pd < 20): `b = 1.25 / Pd`, `c = 0.25 / Pd`
- **Fine** (Pd ≥ 20): `b = 1.2 / Pd + 0.002`, `c = 0.2 / Pd + 0.002`

The solver must automatically select the correct formula and highlight affected variables when Pd crosses the threshold.

### Constraint Validation
- `Np`, `Ng` must be positive integers
- `Pd`, all diameters, and `m` must be > 0
- Pressure angle `phi` must be 0° < φ < 90°
- Solver should reject or round fractional teeth counts

### Solver Algorithm
1. Parse all 17 equations into SymPy symbolic form
2. Build dependency graph (variable → equation → variable)
3. Track known variables set `K` (user inputs + computed)
4. For each equation where (n-1) of n variables are known:
   - Select piecewise rule if applicable
   - Solve symbolically or numerically for unknown
   - Validate domain constraints
   - Add to `K` and repeat until convergence
5. Flag conflicts (overdetermined) or show hints (underdetermined)

### UI Reactivity
When a user edits any variable anywhere in the UI:
- Update the global variable registry
- Trigger solver to recompute all dependent variables
- Update all equation cards displaying that variable
- Maintain source attribution (User vs Computed)

## Acceptance Test Cases

See `instructions/spur_gear_interactive_plan.md` for 5 comprehensive test scenarios:
1. Direct calculation (all inputs given)
2. Bi-directional update (changing derived value recalculates inputs)
3. Piecewise switch (crossing Pd=20 boundary)
4. Integer validation (rejecting fractional teeth)
5. Module conversion (m ↔ Pd sync)

## Tech Stack

### Streamlit App (Python)
- **Python 3.10+**
- **Streamlit** (reactive UI framework)
- **SymPy** (symbolic math and equation solving)
- **Pydantic** (data validation)
- **Pint** (optional units management)
- **NumPy** (numerical operations)
- **pytest** (testing framework)

### HTML Calculators
- **Vanilla JavaScript** (no framework dependencies)
- **MathJax 3** (LaTeX equation rendering via CDN)
- **LocalStorage API** (session persistence)
- **PDF.js** (PDF display, loaded via CDN)

## Transferring to Another Device

### Required Files and Folders
To transfer this project to another device, copy the entire project directory which includes:

#### Core Application Files
```
m325_solver_v2/
├── app/                          # Streamlit application
│   ├── app.py                    # Main entry point
│   ├── engine/                   # Core solver engine
│   │   ├── variables.py
│   │   ├── equations.py
│   │   ├── solver.py
│   │   └── units.py
│   ├── ui/                       # UI components
│   │   ├── cards.py
│   │   └── drawers.py
│   └── data/
│       └── presets.json
├── tests/
│   └── test_equations.py
├── requirements.txt              # Python dependencies
└── .venv/                        # (optional - can recreate)
```

#### HTML Calculators (Standalone - no dependencies)
```
├── calculators/
│   ├── index.html
│   ├── mech_spur_geometry.html
│   ├── mech_helical_geometry.html
│   ├── mech_bevel_geometry.html
│   ├── mech_gear_forces.html
│   ├── mech_spur_stress.html
│   ├── mech_helical_stress.html
│   ├── mech_bevel_stress.html
│   ├── mech_rack_calculator.html
│   ├── mech_worm_calculator.html
│   ├── fizz_spur.html
│   ├── fizz_helical.html
│   ├── fizz_bevel.html
│   ├── fizz_worm.html
│   └── fizz_rack.html
```

#### Reference Documents (Required for HTML calculators)
```
├── documents/
│   ├── spur_design_spec.pdf      # Basic spur geometry
│   ├── helical_design_spec.pdf   # Helical geometry
│   ├── bevel_design_spec.pdf     # Bevel geometry
│   ├── gear_forces.pdf           # Force analysis
│   ├── spur_stress.pdf           # Spur stress analysis
│   ├── helical_stress.pdf        # Helical stress
│   ├── bevel_stress.pdf          # Bevel stress
│   ├── rack.pdf                  # Rack and pinion
│   ├── worm.pdf                  # Worm gears
│   ├── fizz_spur.pdf             # 40-step spur design
│   ├── fizz_helical.pdf          # Helical (warps to spur)
│   ├── fizz_bevel.pdf            # Bevel (warps to spur)
│   ├── fizz_worm.pdf             # 21-step worm design
│   └── fizz_rack.pdf             # 6-step rack design
```

#### Documentation
```
├── CLAUDE.md                     # This file
├── README.md                     # User guide
├── README_HTML_CALCULATOR.md     # HTML calculator docs
├── HELICAL_GEAR_PLAN.md         # Implementation plans
└── BEVEL_GEAR_PLAN.md
```

### Setup on New Device

#### Option 1: Using HTML Calculators Only (No Installation)
1. Copy the entire `m325_solver_v2` folder to the new device
2. Open any `.html` file from the `calculators/` folder in a web browser (Chrome, Firefox, Edge, Safari)
3. The calculators work offline and save sessions to browser LocalStorage
4. **Note**: The PDFs must be in the `documents/` subfolder for proper display

#### Option 2: Running the Streamlit App
1. Copy the entire `m325_solver_v2` folder to the new device
2. Install Python 3.10+ if not already installed
3. Open terminal/command prompt in the project folder
4. Create virtual environment:
   ```bash
   # Windows
   python -m venv .venv
   .venv\Scripts\activate

   # macOS/Linux
   python3 -m venv .venv
   source .venv/bin/activate
   ```
5. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
6. Run the application:
   ```bash
   streamlit run app/app.py
   ```

### Minimum Transfer Requirements

**For HTML calculators only:**
- Complete `calculators/` folder with all `.html` files
- Complete `documents/` folder with all PDFs
- Any modern web browser

**For Streamlit app:**
- Complete `app/` folder
- `requirements.txt`
- Python 3.10+ installed on target device

### Quick Transfer Checklist
- [ ] Copy entire `m325_solver_v2` folder
- [ ] Verify all 15 HTML files are present in `calculators/` folder
- [ ] Verify all 14 PDFs are in `documents/` folder
- [ ] If using Streamlit: verify `app/` folder structure
- [ ] If using Streamlit: verify `requirements.txt` is present
- [ ] Test HTML calculators by opening files from `calculators/` folder in browser
- [ ] (Optional) Test Streamlit app with `streamlit run app/app.py`

### Storage Requirements
- Complete project: ~50-100 MB (mainly PDFs)
- HTML calculators only: ~50 MB
- Streamlit app code: <5 MB (excluding .venv)

### Notes
- The `.venv` folder can be excluded from transfer (will be recreated)
- HTML calculators store session data in browser LocalStorage (not transferred between devices)
- Streamlit app sessions are not persistent by default
- All calculators work independently - no internet connection required after loading
