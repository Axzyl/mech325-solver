# M325 Gear Design Calculator Suite

A comprehensive collection of gear design calculators including:
1. **Streamlit Interactive Solver** - Python-based symbolic equation solver for spur gears
2. **15 HTML Standalone Calculators** - Browser-based calculators for all gear types (no installation required)

**Quick Start**: For immediate use without installation, open any `.html` file from the `calculators/` folder in your browser!

**Transfer to Another Device**: See `TRANSFER.md` for quick setup instructions.

## Features

### Streamlit Interactive Solver
- **Interactive Equation Cards**: Each equation is displayed with LaTeX rendering and editable input fields
- **Automatic Solving**: When enough inputs are known for any equation, the remaining unknown is solved automatically
- **Global Variable Synchronization**: Editing a variable anywhere updates it everywhere in the UI
- **Piecewise Rules**: Automatically handles coarse vs. fine pitch formulas (Pd < 20 vs Pd ≥ 20)
- **Constraint Validation**: Enforces integer teeth counts, positive values, and angle ranges
- **Explainability Panel**: Shows step-by-step calculation traces for derived variables
- **Session Persistence**: Save and load your work sessions as JSON files
- **Variable Locking**: Lock specific variables to prevent the solver from overwriting them

### HTML Standalone Calculators
- **No Installation Required**: Works in any modern browser (Chrome, Firefox, Edge, Safari)
- **Split-Screen Layout**: PDF reference on left, interactive calculator on right
- **Two-Tier Visual Feedback**:
  - White fields = user inputs
  - Green fields = auto-calculated values
- **Session Persistence**: Save/load functionality via browser LocalStorage
- **Offline Support**: Works without internet once loaded
- **MathJax Equations**: Professional equation rendering
- **15 Specialized Calculators**: Covering spur, helical, bevel, worm, rack, and comprehensive design guides

## System Requirements

- Python 3.10 or higher
- Windows, macOS, or Linux

## Quick Start with HTML Calculators

**No installation needed!** Simply:
1. Open any `.html` file from the `calculators/` folder in your browser (e.g., double-click `calculators/fizz_spur.html`)
2. The PDF reference appears on the left, calculator on the right
3. Enter values in white fields, see auto-calculated results in green fields
4. Use "Save Session" to preserve your work in the browser

**Available HTML Calculators (in `calculators/` folder):**
- `calculators/fizz_spur.html` - Complete 40-step spur gear design
- `calculators/fizz_helical.html` - Helical design (13 steps → spur steps 32-40)
- `calculators/fizz_bevel.html` - Bevel design (26 steps → spur steps 35-40)
- `calculators/fizz_worm.html` - 21-step worm gear design
- `calculators/fizz_rack.html` - 6-step rack and pinion
- `calculators/mech_spur_geometry.html` - Basic spur geometry
- `calculators/mech_helical_geometry.html` - Helical geometry
- `calculators/mech_bevel_geometry.html` - Bevel geometry
- `calculators/mech_spur_stress.html` - Spur stress analysis
- `calculators/mech_helical_stress.html` - Helical stress
- `calculators/mech_bevel_stress.html` - Bevel stress
- `calculators/mech_rack_calculator.html` - Rack kinematics
- `calculators/mech_worm_calculator.html` - Worm design
- `calculators/mech_gear_forces.html` - Force analysis
- `calculators/index.html` - Landing page with links to all calculators

## Streamlit App Installation

### 1. Clone or download this repository

```bash
cd m325_solver_v2
```

### 2. Create and activate a virtual environment

**Windows:**
```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

## Running the Application

```bash
streamlit run app/app.py
```

The application will open in your default web browser at `http://localhost:8501`.

## Usage Guide

### Basic Workflow

1. **Enter Input Values**: Start by entering known values in any equation card (e.g., Pd, Np, Ng, φ)
2. **Auto-Solve**: The solver automatically runs and computes all derivable variables
3. **Review Results**: Computed values appear with a green "Computed" badge
4. **Lock Variables**: Use the lock checkbox to prevent specific values from being overwritten
5. **Explore Calculations**: Click "Explain" on any equation to see the calculation trace

### Understanding the Interface

#### Equation Cards
- **Left side**: LaTeX-rendered equation
- **Right side**: Input fields for each variable
- **Blue badge**: User-entered value
- **Green badge**: Computed value
- **Lock checkbox**: Prevents solver from changing the value

#### Sidebar Controls
- **Variable Table**: View all variables with search functionality
- **Assumptions Panel**: Shows active pitch type (coarse/fine) and formulas
- **Quick Actions**: Reset all values or view statistics

#### Top Bar
- **Auto-solve toggle**: Enable/disable automatic solving
- **Save button**: Save current session to JSON file
- **Load button**: Load a previously saved session

### Variables Reference

| Variable | Description | Unit | Domain |
|----------|-------------|------|--------|
| Pd | Diametral Pitch | teeth/in | R+ (positive real) |
| φ (phi) | Pressure Angle | degrees | 0° < φ < 90° |
| Np | Number of Teeth (Pinion) | - | Z+ (positive integer) |
| Ng | Number of Teeth (Gear) | - | Z+ (positive integer) |
| DP | Pitch Diameter (Pinion) | in | R+ |
| DG | Pitch Diameter (Gear) | in | R+ |
| p | Circular Pitch | in | R+ |
| a | Addendum | in | R+ |
| b | Dedendum | in | R+ |
| c | Clearance | in | R+ |
| DoP | Outside Diameter (Pinion) | in | R+ |
| DoG | Outside Diameter (Gear) | in | R+ |
| DRP | Root Diameter (Pinion) | in | R+ |
| DRG | Root Diameter (Gear) | in | R+ |
| ht | Whole Depth | in | R+ |
| hk | Working Depth | in | R+ |
| t | Tooth Thickness | in | R+ |
| C | Center Distance | in | R+ |
| DbP | Base Circle Diameter (Pinion) | in | R+ |
| DbG | Base Circle Diameter (Gear) | in | R+ |
| m | Module | mm | R+ |

### Example Usage Scenarios

#### Scenario 1: Complete Design from Basic Parameters
```
Enter:
  Pd = 10 teeth/in
  Np = 20
  Ng = 40
  φ = 20°

Result:
  All 17+ derived parameters computed automatically
```

#### Scenario 2: Reverse Engineering from Physical Measurement
```
Enter:
  DP = 2.0 in (measured pitch diameter)
  Np = 20 (counted teeth)

Result:
  Pd = 10 teeth/in (computed)
  All other parameters can be derived if you add Ng and φ
```

#### Scenario 3: Module Conversion
```
Enter:
  m = 2.54 mm

Result:
  Pd = 10.0 teeth/in (automatically converted)
```

### Piecewise Formulas

The application automatically selects the appropriate formulas based on the diametral pitch:

**Coarse Pitch (Pd < 20):**
- b = 1.25 / Pd
- c = 0.25 / Pd

**Fine Pitch (Pd ≥ 20):**
- b = 1.2 / Pd + 0.002
- c = 0.2 / Pd + 0.002

A blue banner appears when the pitch type switches.

## Running Tests

The project includes comprehensive acceptance tests covering all 5 test scenarios from the specification.

```bash
pytest tests/test_equations.py -v
```

### Test Coverage
- ✅ Direct calculation (coarse pitch)
- ✅ Bi-directional updates
- ✅ Piecewise formula switching
- ✅ Integer validation for tooth counts
- ✅ Module ↔ Pd conversion
- ✅ Constraint validation
- ✅ Solver convergence
- ✅ Variable locking

## Project Structure

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
    presets.json        # Example scenarios
tests/
  test_equations.py     # Acceptance tests
```

## Technical Details

### Architecture
- **Symbolic Math**: Uses SymPy for equation solving and symbolic manipulation
- **Data Validation**: Pydantic models ensure type safety and constraint validation
- **Dependency Graph**: Solver builds a graph of equation dependencies to determine solving order
- **Iterative Solving**: Runs multiple iterations until convergence or no progress

### Solver Algorithm
1. Parse all equations into symbolic form using SymPy
2. Build dependency graph (variable → equation → variable relationships)
3. Maintain set K of known variables (user inputs + computed values)
4. For each equation where all-but-one variables are known:
   - Select appropriate piecewise rule based on Pd
   - Solve symbolically for the unknown variable
   - Validate result against domain constraints
   - Update K and repeat until convergence
5. Report conflicts (overdetermined) or missing inputs (underdetermined)

## Troubleshooting

### Common Issues

**Validation errors:**
- Ensure Np and Ng are whole numbers
- Verify φ is between 0° and 90°
- Check that all length/diameter values are positive

**Solver not converging:**
- Provide more input values (typically need Pd, Np, Ng, φ to fully solve)
- Check for conflicting locked variables
- Review alert banners for missing inputs

**Piecewise formulas unexpected:**
- Check the current Pd value in the Assumptions panel
- Remember: Pd = 20 is the boundary between coarse and fine pitch

## Reference

This implementation is based on the spur gear design specification document (`documents/spur_design_spec.pdf`). All 17 fundamental equations are implemented with proper piecewise handling.

## License

This project is provided as-is for educational and engineering purposes.

## Contributing

To extend this application:
1. Add new equations in `app/engine/equations.py`
2. Add new variables in `app/engine/variables.py` (update VARIABLE_DEFS)
3. Update tests in `tests/test_equations.py`
4. Run tests to verify: `pytest tests/ -v`
