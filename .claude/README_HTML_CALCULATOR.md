# HTML Spur Gear Calculator - Quick Start Guide

## 🚀 How to Use

### Option 1: Double-Click (Easiest)
Simply **double-click** any HTML file in the `calculators/` folder (e.g., `calculators/mech_spur_geometry.html`). It will open in your default web browser.

### Option 2: Open in Browser
1. Right-click any HTML file in the `calculators/` folder
2. Select "Open with" → Choose your browser (Chrome, Firefox, Edge, etc.)

## ✨ Features

### What You Get
- ✅ **No installation required** - Just open and use
- ✅ **Instant calculations** - Updates as you type
- ✅ **PDF embedded** - Reference spec on the left, calculator on the right
- ✅ **Works offline** - No internet needed
- ✅ **Auto-saves** - Values persist between sessions
- ✅ **No Python needed** - Pure HTML/CSS/JavaScript

### Layout
```
┌─────────────────┬─────────────────┐
│                 │ Calculator      │
│  PDF Viewer     │ Given Info:     │
│                 │ Pd = [___]      │
│  (Reference     │ Np = [___]      │
│   Spec)         │ φ  = [___]      │
│                 │ Ng = [___]      │
│                 │                 │
│                 │ Step 1: ...     │
│                 │ DP = [___] ✓    │
│                 │ ...             │
└─────────────────┴─────────────────┘
```

## 📝 Usage

1. **Enter Given Values** (top section):
   - Pd (Diametral Pitch)
   - φ (Pressure Angle)
   - Np (Number of teeth - Pinion)
   - Ng (Number of teeth - Gear)

2. **Calculations Update Instantly**:
   - All 9 steps calculate automatically
   - Results show with ✓ checkmark
   - Grayed-out fields are computed values

3. **Automatic Features**:
   - Detects Coarse vs Fine pitch (< 20 or ≥ 20)
   - Shows appropriate formulas
   - Module (m) ↔ Pd conversion works both ways

4. **Save Your Work**:
   - Click "💾 Save Session" to save values
   - Values persist even after closing browser
   - Click "🔄 Reset All" to start fresh

## 🎯 Example

Try these values:
- Pd = 10
- Np = 20
- φ = 20
- Ng = 40

Watch all calculations update instantly!

## 🔧 Advanced Tips

### Module Conversion
If you have module (m) instead of Pd:
- Enter the module value in the "Module Converter" section
- Pd will auto-calculate (Pd = 25.4 / m)
- Or enter Pd and m will auto-calculate

### Coarse vs Fine Pitch
- **Coarse** (Pd < 20): Uses b = 1.25/Pd, c = 0.25/Pd
- **Fine** (Pd ≥ 20): Uses b = 1.2/Pd + 0.002, c = 0.2/Pd + 0.002
- Automatically switches and shows blue indicator

### Viewing the PDF
- PDF displays on the left side
- Scroll independently from calculator
- If PDF doesn't load, make sure `documents/spur_design_spec.pdf` exists

## ⚡ Why This is Better Than Streamlit

1. **No Variable Update Issues**: Pure JavaScript - instant reactivity
2. **PDF Integration**: Embedded directly, side-by-side
3. **No Dependencies**: No Python, no server, no installation
4. **Blazing Fast**: All calculations happen in browser
5. **Easy to Share**: Just send the HTML file
6. **Works Anywhere**: Any computer with a browser

## 📂 File Structure

You only need:
```
calculators/
  *.html                      ← All calculator files
documents/
  *.pdf                       ← Reference PDFs for embedded viewer
```

## 🐛 Troubleshooting

**PDF doesn't show?**
- Make sure the `documents/` folder exists at the same level as the `calculators/` folder
- Check that the corresponding PDF file exists in `documents/`
- Some browsers block local PDFs - try Chrome or Firefox

**Values not saving?**
- Make sure browser allows localStorage
- Check browser's privacy settings

**Calculator not updating?**
- Make sure JavaScript is enabled in your browser
- Try clearing browser cache

## 💡 Customization

Want to modify the calculator? It's just one HTML file!
- Open in any text editor
- All code is commented and organized
- CSS is at the top for easy styling changes
- JavaScript is at the bottom for logic changes

---

**That's it! Enjoy your self-contained spur gear calculator!** 🎉
