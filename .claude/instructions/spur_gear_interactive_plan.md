# Prompt: Build a Streamlit "Interactive Spur Gear Spec" Calculator (Python)

## Goal
Create a Streamlit app that renders the *spur gear design spec* as interactive equation cards with text boxes. When enough inputs are known for any equation, the remaining unknown is solved automatically. Variables are globally synchronized: editing a variable anywhere updates it everywhere. Include units, piecewise rules (coarse vs fine pitch), constraints (e.g., integer teeth), an explainability panel, and save/load of sessions.

The equation set and variable list come from the document located at `documents/spur_design_spec.pdf`.

---

## Tech Stack
- **Language:** Python 3.10+
- **Framework:** Streamlit (latest version)
- **Libraries:**
  - `sympy` → symbolic math, algebraic solving, numeric solving
  - `pint` → optional units management
  - `pydantic` → data validation for variables and equations
  - `math` and `numpy` → basic math functions

---

## Project Structure
```plaintext
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

---

## Data Models

### Variable
```python
class Variable(BaseModel):
    id: Literal[
        "Pd","phi","Np","Ng","DP","DG","p","a","b","c",
        "DoP","DoG","DRP","DRG","ht","hk","t","C","DbP","DbG","m"
    ]
    label: str
    unit: str | None
    value: float | None
    source: Literal["user","derived"] = "user"
    locked: bool = False
    domain: Literal["R+","Z+","angle"]
    precision: int = 4
    aliases: list[str] = []
```

### Equation
```python
class Equation(BaseModel):
    id: str
    latex: str
    sympy: Callable | None
    vars: list[str]
    piecewise: Optional[dict]
    notes: str | None
```

---

## Equations

Base relationships from `documents/spur_design_spec.pdf`:

| Step | Equation | Notes |
|------|-----------|-------|
| 1 | DP = Np / Pd | Pitch diameter (pinion) |
| 2 | DG = Ng / Pd | Pitch diameter (gear) |
| 3 | p = π / Pd | Circular pitch |
| 4 | a = 1 / Pd | Addendum |
| 5 | b = 1.25 / Pd (coarse) or 1.2 / Pd + 0.002 (fine) | Dedendum |
| 6 | c = 0.25 / Pd (coarse) or 0.2 / Pd + 0.002 (fine) | Clearance |
| 7 | DoP = (Np + 2) / Pd | Outside diameter (pinion) |
| 8 | DoG = (Ng + 2) / Pd | Outside diameter (gear) |
| 9 | DRP = DP - 2b | Root diameter (pinion) |
| 10 | DRG = DG - 2b | Root diameter (gear) |
| 11 | ht = a + b | Whole depth |
| 12 | hk = 2a | Working depth |
| 13 | t = π / (2Pd) | Tooth thickness |
| 14 | C = (Ng + Np) / (2Pd) | Center distance |
| 15 | DbP = DP * cos(φ) | Base circle (pinion) |
| 16 | DbG = DG * cos(φ) | Base circle (gear) |
| 17 | Pd = 25.4 / m | Module conversion |

---

## Solver Logic

### Algorithm Steps
1. Parse all equations into symbolic form using `sympy`.
2. Build a dependency graph (variable → equation → variable relationships).
3. Maintain a set `K` of known (user or computed) variables.
4. For each equation:
   - Select appropriate piecewise rule (coarse/fine) based on Pd.
   - If all but one variable are known, solve for the unknown symbolically or numerically.
   - Validate the result (domain checks, integer constraints).
   - Update `K` and repeat until convergence.
5. Handle inconsistencies:
   - Overdetermined → flag conflict.
   - Underdetermined → show missing variable hints.

### Constraints
- `Np, Ng` must be positive integers.
- `Pd > 0`, all diameters > 0.
- `0 < φ < 90°`.

### Piecewise Behavior
If Pd < 20 → coarse formulas.  
If Pd ≥ 20 → fine formulas.  
Switch automatically and highlight affected variables.

---

## Streamlit UI

### Layout
- **Top bar**: Unit system toggle, assumptions (coarse/fine), Save/Load.
- **Equation cards**:
  - Left: LaTeX-rendered formula.
  - Right: input boxes for variables.
  - Chips for variable source (User / Computed).
  - “Explain” button showing substituted calculation.
- **Global variable drawer**:
  - Editable table of all variables.
  - Searchable.
  - Lock toggle for each variable.
- **Alerts**:
  - Missing inputs → yellow banner.
  - Conflicts → red banner.
  - Piecewise switch → blue banner.

### Persistence
- Save/load states as JSON in local directory.
- Include schema versioning.

---

## Implementation Milestones

1. **Setup**: Define variables and equations, basic Streamlit layout.
2. **Equation Engine**: Implement symbolic solver with dependency graph.
3. **Reactive UI**: Automatically update variables on edit.
4. **Piecewise Logic**: Add coarse/fine pitch handling.
5. **Persistence & Explainability**: Save/Load sessions, display calculation traces.
6. **Testing**: Add acceptance and golden tests.

---

## Acceptance Tests

### Test 1 – Direct Calculation
Inputs: Pd=10, Np=20, Ng=40, φ=20°  
Expected:
- DP=2.0, DG=4.0
- p=π/10
- a=0.1, b=0.125, c=0.025
- DoP=2.2, DoG=4.2
- DRP=1.75, DRG=3.75
- ht=0.225, hk=0.2
- t=π/20
- C=3.0
- DbP=DP*cos(20°), DbG=DG*cos(20°)

### Test 2 – Bi-directional Update
Change DP → Pd recomputes, all values update.

### Test 3 – Piecewise Switch
Cross Pd=20 boundary → b and c formulas switch.

### Test 4 – Integer Validation
Np=20.7 → error, must be integer.

### Test 5 – Module Conversion
m=2.54 ↔ Pd=10.0 bi-directional sync.

---

## Deliverables
- Working Streamlit app
- JSON registry for equations and variables
- Tests verifying computed outputs
- README.md with setup and usage instructions

