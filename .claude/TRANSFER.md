# Quick Transfer Guide

## For New Device Setup

### Easiest Method: HTML Calculators (No Installation)

1. **Copy the folder**: Transfer the entire `m325_solver_v2` folder to your new device
2. **Open in browser**: Double-click any `.html` file from the `calculators/` folder (e.g., `calculators/fizz_spur.html`)
3. **Start calculating**: The calculator works immediately in your browser

**That's it!** No installation, no setup, no dependencies.

### Files You Need

#### Essential (for HTML calculators):
```
m325_solver_v2/
├── calculators/*.html (15 calculator files)
└── documents/*.pdf (14 PDF reference files)
```

#### Optional (for Python/Streamlit app):
```
├── app/ (complete folder)
├── requirements.txt
└── tests/ (optional)
```

### Available Calculators (in `calculators/` folder)

#### Basic Geometry Calculators
- `calculators/mech_spur_geometry.html` - Spur gear dimensions
- `calculators/mech_helical_geometry.html` - Helical gear dimensions
- `calculators/mech_bevel_geometry.html` - Bevel gear dimensions

#### Stress Analysis Calculators
- `calculators/mech_spur_stress.html` - Spur stress & safety factors
- `calculators/mech_helical_stress.html` - Helical stress analysis
- `calculators/mech_bevel_stress.html` - Bevel stress analysis

#### Specialized Calculators
- `calculators/mech_rack_calculator.html` - Rack & pinion kinematics
- `calculators/mech_worm_calculator.html` - Worm gear design
- `calculators/mech_gear_forces.html` - Force analysis

#### Comprehensive Design Guides (Fizz Series)
- `calculators/fizz_spur.html` - 40-step complete spur design
- `calculators/fizz_helical.html` - Helical design (13 steps → spur steps 32-40)
- `calculators/fizz_bevel.html` - Bevel design (26 steps → spur steps 35-40)
- `calculators/fizz_worm.html` - 21-step worm design
- `calculators/fizz_rack.html` - 6-step rack design

#### Index
- `calculators/index.html` - Landing page with links to all calculators

### Running the Streamlit App (Optional)

If you want the Python interactive solver:

```bash
# Navigate to project folder
cd m325_solver_v2

# Create virtual environment
python -m venv .venv

# Activate it (Windows)
.venv\Scripts\activate

# Activate it (macOS/Linux)
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run app/app.py
```

### Troubleshooting

**PDFs not showing in HTML calculators?**
- Make sure the `documents/` folder is in the same location as the HTML files
- Check that all PDF files are present in `documents/`

**Calculator not calculating?**
- Try entering values in the white (user input) fields first
- Green fields auto-calculate automatically when enough inputs are provided

**Want to save your work?**
- HTML calculators: Use the "Save Session" button (saves to browser)
- Streamlit app: Use the Save button in the app

### Need Help?

- See `README.md` for detailed usage instructions
- See `CLAUDE.md` for complete technical documentation
- See `README_HTML_CALCULATOR.md` for HTML calculator specifics
