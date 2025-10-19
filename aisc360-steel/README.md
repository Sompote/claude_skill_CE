# AISC 360 Steel Design Skill

## Overview
Comprehensive steel structural member design according to AISC 360-22 (16th Edition) using SI units.

## What This Skill Does
- Design tension members (Chapter D)
- Design compression members (Chapter E)
- Design flexural members (Chapter F)
- Design for shear (Chapter G)
- Check combined loading (Chapter H)
- Design bolted connections (Chapter J3)
- Design welded connections (Chapter J2)
- Generate Python code for calculations

## Units
- Forces: N (Newtons), kN (kilonewtons)
- Lengths: mm (millimeters)
- Stress: MPa (megapascals)
- Moments: N·mm (convert to kN·m by ÷ 10⁶)

## Installation
1. Download `aisc360-steel.zip`
2. In Claude, go to Settings → Custom Skills
3. Click "Upload Skill" or "Add Skill"
4. Select the ZIP file
5. The skill will be available in your skill library

## Usage Examples

### Example 1: Design a tension member
```
Design a tension member using A992 steel with gross area 5000 mm². 
The factored tension load is 1200 kN. Use LRFD method.
```

### Example 2: Check a column
```
Check a W310×52 column (A992 steel) with:
- Length: 4 m
- K = 1.0 (pinned-pinned)
- Factored load: 500 kN compression
Use LRFD method and check about weak axis.
```

### Example 3: Design a beam
```
Design a simply supported beam for:
- Span: 8 m
- Factored moment: 200 kN·m
- Lateral bracing at supports only
- A992 steel, LRFD method
```

### Example 4: Combined loading
```
Check a W360×79 beam-column with:
- Pu = 800 kN (compression)
- Mux = 180 kN·m
- Column length: 5 m, K = 1.0
- Beam unbraced length: 2.5 m
- A992 steel, LRFD method
```

### Example 5: Connection design
```
Design a bolted connection using A325-N bolts (M20):
- Factored shear per bolt: 60 kN
- Plate: A36 steel, 12 mm thick
- Check shear, bearing, and determine number of bolts needed
```

## Features
- Both LRFD and ASD methods
- Complete Python implementations
- Step-by-step calculations
- All relevant AISC 360-22 chapters
- Practical design examples
- Quick reference formulas

## Supported Steel Grades
- A36 (Fy = 250 MPa)
- A992 (Fy = 345 MPa)
- A572 Grade 50 (Fy = 345 MPa)
- A500 Grade B/C (HSS)

## Design Checks Included
✓ Tension: yielding, rupture, net section, shear lag
✓ Compression: buckling, slenderness, effective length
✓ Flexure: lateral-torsional buckling, plastic moment
✓ Shear: web yielding and buckling
✓ Combined: axial + bending interaction
✓ Connections: bolts (shear, tension, bearing) and welds

## Reference
AISC 360-22: Specification for Structural Steel Buildings, 16th Edition

## Version
1.0.0

## Author
Structural Engineering Expert

## Tags
structural engineering, steel design, AISC 360, structural analysis, civil engineering, building design, connection design
