---
name: steel360
description: Comprehensive steel structural member design according to AISC 360-22 using SI units. Includes tension, compression, flexure, shear, combined loading, and connections with complete Python implementations.
---

# AISC 360 Steel Member Design

## Description

Comprehensive steel structural member design according to AISC 360-22 (American Institute of Steel Construction Specification for Structural Steel Buildings, 16th Edition). This skill enables analysis and design verification for tension members, compression members, flexural members, shear, combined loading, and connections using SI units (N, mm, MPa).

## When to Use This Skill

Use this skill when Claude needs to:
- Design or verify steel structural members (beams, columns, tension members)
- Calculate member capacities according to AISC 360-22
- Check strength and stability limit states
- Design bolted or welded connections
- Perform interaction checks for combined loading
- Generate Python code for steel design calculations
- Create design reports with strength calculations

## Key Capabilities

- **Tension Members (Chapter D)**: Yielding, rupture, net section, shear lag, block shear
- **Compression Members (Chapter E)**: Column buckling, slenderness, effective length
- **Flexural Members (Chapter F)**: Bending strength, lateral-torsional buckling
- **Shear Design (Chapter G)**: Web yielding and buckling
- **Combined Loading (Chapter H)**: Interaction equations for axial plus bending
- **Connections (Chapter J)**: Bolts (shear, tension, bearing) and welds (fillet welds)
- **Both LRFD and ASD**: Load and Resistance Factor Design and Allowable Strength Design

## Important Notes

**Units**: All calculations use SI units
- Forces: N (Newtons), kN = 1000 N
- Lengths: mm (millimeters), m = 1000 mm
- Stress: MPa (megapascals) = N/mm²
- Moments: N·mm (convert to kN·m by dividing by 1000000)

**Standard**: AISC 360-22 (16th Edition, 2022)

**Material Properties**:
- E (Modulus of Elasticity) = 200000 MPa
- G (Shear Modulus) = 77200 MPa

---

## 1. MATERIAL PROPERTIES

### Common Steel Grades (ASTM)

```python
# Material properties in MPa
STEEL_A36 = {'Fy': 250, 'Fu': 400}
STEEL_A992 = {'Fy': 345, 'Fu': 450}
STEEL_A572_50 = {'Fy': 345, 'Fu': 450}
```

| Grade | Fy (MPa) | Fu (MPa) | Application |
|-------|----------|----------|-------------|
| A36   | 250      | 400-550  | Plates, angles, general shapes |
| A992  | 345      | 450      | W-shapes (buildings) |
| A572-50 | 345    | 450      | High-strength shapes |
| A500-B | 315     | 400      | HSS rectangular |
| A500-C | 345     | 430      | HSS round |

### Resistance Factors and Safety Factors

| Limit State | phi (LRFD) | Omega (ASD) | Reference |
|-------------|------------|-------------|-----------|
| Tension yielding | 0.90 | 1.67 | D2 |
| Tension rupture | 0.75 | 2.00 | D2 |
| Compression | 0.90 | 1.67 | E3 |
| Flexure | 0.90 | 1.67 | F1 |
| Shear | 1.00 | 1.50 | G2.1 |
| Bolts and Welds | 0.75 | 2.00 | J3, J2 |

---

## 2. TENSION MEMBERS (CHAPTER D)

### Design Equations

The design tensile strength is the minimum of:

**1. Yielding of Gross Section**
```
LRFD: phi*Pn = 0.90 × Fy × Ag  [N]
ASD:  Pn/Omega = (Fy × Ag) / 1.67  [N]
```

**2. Rupture of Net Section**
```
LRFD: phi*Pn = 0.75 × Fu × Ae  [N]
ASD:  Pn/Omega = (Fu × Ae) / 2.00  [N]

Where: Ae = An × U (effective net area)
       An = net area (gross area minus holes)
       U = shear lag factor
```

### Shear Lag Factor U (Table D3.1)

| Connection Type | U |
|----------------|---|
| All elements connected | 1.0 |
| W-shapes, flange connected (bf >= 2d/3) | 0.90 |
| W-shapes, flange connected (bf < 2d/3) | 0.85 |
| Single angle, 1 bolt | 0.80 |
| Single angle, 2-3 bolts | 0.60 |

**General formula**: U = 1 - (x_bar/L) >= 0.60
- x_bar = connection eccentricity (mm)
- L = connection length (mm)

### Python Implementation

```python
def design_tension_member(Fy, Fu, Ag, An=None, U=1.0, Pu=0, method='LRFD'):
    """
    AISC 360 Chapter D - Tension member design
    
    Parameters:
    Fy, Fu : Yield and ultimate strength (MPa)
    Ag : Gross area (mm²)
    An : Net area (mm²), if None use Ag
    U : Shear lag factor
    Pu : Required strength (N)
    method : 'LRFD' or 'ASD'
    
    Returns: dict with Pn_yield, Pn_rupture, Pc, utilization, status
    """
    An = Ag if An is None else An
    Ae = An * U
    
    Pn_yield = Fy * Ag
    Pn_rupture = Fu * Ae
    
    if method == 'LRFD':
        Pc_yield = 0.90 * Pn_yield
        Pc_rupture = 0.75 * Pn_rupture
    else:
        Pc_yield = Pn_yield / 1.67
        Pc_rupture = Pn_rupture / 2.00
    
    Pc = min(Pc_yield, Pc_rupture)
    controlling = 'yielding' if Pc_yield < Pc_rupture else 'rupture'
    
    utilization = Pu / Pc if Pc > 0 else 0
    status = 'OK' if utilization <= 1.0 else 'NG'
    
    return {
        'Pn_yield': Pn_yield,
        'Pn_rupture': Pn_rupture,
        'Pc': Pc,
        'utilization': utilization,
        'status': status,
        'controlling': controlling
    }
```

---

## 3. COMPRESSION MEMBERS (CHAPTER E)

### Design Equations

**Nominal Compressive Strength**
```
Pn = Fcr × Ag  [N]

LRFD: phi_c*Pn where phi_c = 0.90
ASD:  Pn/Omega_c where Omega_c = 1.67
```

**Critical Stress Fcr**

Step 1: Calculate elastic buckling stress
```
Fe = pi² × E / (KL/r)²  [MPa]

Where:
E = 200000 MPa
KL/r = slenderness ratio
K = effective length factor
L = unbraced length (mm)
r = radius of gyration (mm)
```

Step 2: Calculate slenderness parameter
```
lambda_c = sqrt(Fy/Fe)
```

Step 3: Determine Fcr
```
If lambda_c <= 2.25 (inelastic):  Fcr = [0.658^(Fy/Fe)] × Fy
If lambda_c > 2.25 (elastic):     Fcr = 0.877 × Fe
```

### Effective Length Factor K

| Support Condition | K |
|------------------|---|
| Pinned-Pinned | 1.0 |
| Fixed-Fixed | 0.65 |
| Fixed-Pinned | 0.80 |
| Fixed-Free | 2.1 |

**Recommended limit**: KL/r <= 200

### Python Implementation

```python
import numpy as np

def design_compression_member(Fy, Ag, r, K, L, Pu=0, method='LRFD'):
    """
    AISC 360 Chapter E - Compression member design
    
    Parameters:
    Fy : Yield strength (MPa)
    Ag : Gross area (mm²)
    r : Radius of gyration (mm)
    K : Effective length factor
    L : Unbraced length (mm)
    Pu : Required strength (N)
    method : 'LRFD' or 'ASD'
    
    Returns: dict with Fe, Fcr, Pn, Pc, utilization, status
    """
    E = 200000
    
    KL_r = K * L / r
    Fe = np.pi**2 * E / KL_r**2
    lambda_c = np.sqrt(Fy / Fe)
    
    if lambda_c <= 2.25:
        Fcr = np.power(0.658, Fy/Fe) * Fy
        mode = 'inelastic'
    else:
        Fcr = 0.877 * Fe
        mode = 'elastic'
    
    Pn = Fcr * Ag
    
    if method == 'LRFD':
        Pc = 0.90 * Pn
    else:
        Pc = Pn / 1.67
    
    utilization = Pu / Pc if Pc > 0 else 0
    status = 'OK' if utilization <= 1.0 and KL_r <= 200 else 'NG'
    
    return {
        'KL_r': KL_r,
        'Fe': Fe,
        'lambda_c': lambda_c,
        'Fcr': Fcr,
        'Pn': Pn,
        'Pc': Pc,
        'utilization': utilization,
        'status': status,
        'buckling_mode': mode
    }
```

---

## 4. FLEXURAL MEMBERS (CHAPTER F)

### Design Equations

**Nominal Moment Capacity**

For compact doubly-symmetric I-shapes:
```
LRFD: phi_b*Mn where phi_b = 0.90
ASD:  Mn/Omega_b where Omega_b = 1.67
```

### Lateral-Torsional Buckling (LTB)

**Limiting Unbraced Lengths**
```
Lp = 1.76 × ry × sqrt(E/Fy)  [mm]

Lr = 1.95 × rts × (E/0.7Fy) × sqrt[...]  [mm]
```

**Moment Capacity Based on Unbraced Length Lb**

**Case 1: Lb <= Lp** (Fully braced)
```
Mn = Mp = Fy × Zx  [N·mm]
```

**Case 2: Lp < Lb <= Lr** (Inelastic LTB)
```
Mn = Cb × [Mp - (Mp - 0.7*Fy*Sx) × (Lb - Lp)/(Lr - Lp)] <= Mp
```

**Case 3: Lb > Lr** (Elastic LTB)
```
Mn = Fcr × Sx <= Mp
Fcr = (Cb*pi²*E)/(Lb/rts)² × sqrt[1 + 0.078*(J*c/Sx*ho)*(Lb/rts)²]
```

### Moment Gradient Factor Cb

```
Cb = 12.5*Mmax / (2.5*Mmax + 3*MA + 4*MB + 3*MC)

Where:
Mmax = absolute maximum moment in segment
MA, MB, MC = absolute moments at quarter, mid, and three-quarter points

Conservative: Cb = 1.0
Uniform moment: Cb = 1.0
```

### Python Implementation

```python
def design_flexural_member(Fy, Zx, Sx, ry, rts, J, ho, Lb, Cb=1.0, Mu=0, method='LRFD'):
    """
    AISC 360 Chapter F - Flexural member design
    Assumes compact doubly-symmetric I-shape
    """
    E = 200000
    
    Mp = Fy * Zx
    Lp = 1.76 * ry * np.sqrt(E / Fy)
    
    c = 1.0
    term1 = (J * c) / (Sx * ho)
    term2 = 6.76 * (0.7 * Fy / E)**2
    Lr = 1.95 * rts * (E / (0.7 * Fy)) * np.sqrt(term1 + np.sqrt(term1**2 + term2))
    
    if Lb <= Lp:
        Mn = Mp
        case = 'plastic'
    elif Lb <= Lr:
        Mn = Cb * (Mp - (Mp - 0.7 * Fy * Sx) * (Lb - Lp) / (Lr - Lp))
        Mn = min(Mn, Mp)
        case = 'inelastic_LTB'
    else:
        Fcr = (Cb * np.pi**2 * E) / (Lb / rts)**2 * \
              np.sqrt(1 + 0.078 * (J * c) / (Sx * ho) * (Lb / rts)**2)
        Mn = Fcr * Sx
        Mn = min(Mn, Mp)
        case = 'elastic_LTB'
    
    if method == 'LRFD':
        Mc = 0.90 * Mn
    else:
        Mc = Mn / 1.67
    
    utilization = Mu / Mc if Mc > 0 else 0
    status = 'OK' if utilization <= 1.0 else 'NG'
    
    return {
        'Mp': Mp,
        'Lp': Lp,
        'Lr': Lr,
        'Mn': Mn,
        'Mc': Mc,
        'utilization': utilization,
        'status': status,
        'case': case
    }
```

---

## 5. SHEAR DESIGN (CHAPTER G)

### Design Equations

**Shear Strength**
```
Vn = 0.6 × Fy × Aw × Cv1  [N]

LRFD: phi_v*Vn where phi_v = 1.00
ASD:  Vn/Omega_v where Omega_v = 1.50

Where:
Aw = d × tw (web area for I-shapes)
Cv1 = web shear coefficient
```

### Web Shear Coefficient Cv1

```
If h/tw <= 2.24*sqrt(E/Fy):           Cv1 = 1.0

If 2.24*sqrt(E/Fy) < h/tw <= 1.10*sqrt(kv*E/Fy):
    Cv1 = 1.10*sqrt(kv*E/Fy) / (h/tw)

If h/tw > 1.10*sqrt(kv*E/Fy):
    Cv1 = 1.51*kv*E / [(h/tw)²*Fy]

Where:
h = clear distance between flanges (mm)
kv = 5 for unstiffened webs
```

For A992 steel (Fy = 345 MPa):
- Limit 1: h/tw <= 53.9
- Limit 2: h/tw <= 82.9

### Python Implementation

```python
def design_shear(Fy, d, tw, tf, Vu=0, method='LRFD'):
    """AISC 360 Chapter G - Shear design"""
    E = 200000
    
    Aw = d * tw
    h = d - 2 * tf
    h_tw = h / tw
    
    kv = 5.0
    limit1 = 2.24 * np.sqrt(E / Fy)
    limit2 = 1.10 * np.sqrt(kv * E / Fy)
    
    if h_tw <= limit1:
        Cv1 = 1.0
    elif h_tw <= limit2:
        Cv1 = 1.10 * np.sqrt(kv * E / Fy) / h_tw
    else:
        Cv1 = 1.51 * kv * E / (h_tw**2 * Fy)
    
    Vn = 0.6 * Fy * Aw * Cv1
    
    if method == 'LRFD':
        Vc = 1.00 * Vn
    else:
        Vc = Vn / 1.50
    
    utilization = Vu / Vc if Vc > 0 else 0
    status = 'OK' if utilization <= 1.0 else 'NG'
    
    return {
        'Aw': Aw,
        'h_tw': h_tw,
        'Cv1': Cv1,
        'Vn': Vn,
        'Vc': Vc,
        'utilization': utilization,
        'status': status
    }
```

---

## 6. COMBINED LOADING (CHAPTER H)

### Interaction Equations

**Compression plus Bending**

```
If Pr/Pc >= 0.2:
    (Pr/Pc) + (8/9) × [(Mrx/Mcx) + (Mry/Mcy)] <= 1.0

If Pr/Pc < 0.2:
    (Pr/2Pc) + [(Mrx/Mcx) + (Mry/Mcy)] <= 1.0
```

**Tension plus Bending**
```
(Pr/Pc) + [(Mrx/Mcx) + (Mry/Mcy)] <= 1.0
```

Where:
- Pr = required axial strength (N)
- Pc = available axial strength (N)
- Mr = required moment (N·mm)
- Mc = available moment (N·mm)

### Python Implementation

```python
def check_interaction(Pr, Mrx, Mry, Pc, Mcx, Mcy, member_type='compression'):
    """AISC 360 Chapter H - Interaction check"""
    pr_pc = Pr / Pc if Pc > 0 else 0
    mrx_mcx = Mrx / Mcx if Mcx > 0 else 0
    mry_mcy = Mry / Mcy if Mcy > 0 else 0
    
    if member_type == 'compression':
        if pr_pc >= 0.2:
            ratio = pr_pc + (8/9) * (mrx_mcx + mry_mcy)
            equation = 'H1-1a'
        else:
            ratio = pr_pc / 2 + (mrx_mcx + mry_mcy)
            equation = 'H1-1b'
    else:
        ratio = pr_pc + (mrx_mcx + mry_mcy)
        equation = 'H1-1b'
    
    status = 'OK' if ratio <= 1.0 else 'NG'
    
    return {
        'ratio': ratio,
        'status': status,
        'equation': equation
    }
```

---

## 7. BOLTED CONNECTIONS (CHAPTER J3)

### Bolt Strengths

**Shear Strength**
```
LRFD: phi*Rn = 0.75 × Fnv × Ab  [N per bolt]
ASD:  Rn/Omega = (Fnv × Ab) / 2.00  [N per bolt]

Where:
Ab = pi × d² / 4 (nominal bolt area, mm²)
d = bolt diameter (mm)
```

**Bolt Shear Stress Fnv (MPa)**

| Grade | Threads in Shear | Threads Excluded |
|-------|------------------|------------------|
| A325  | 372              | 457              |
| A490  | 457              | 579              |

**Tensile Strength**
```
LRFD: phi*Rn = 0.75 × Fnt × Ab  [N per bolt]
ASD:  Rn/Omega = (Fnt × Ab) / 2.00  [N per bolt]
```

**Bolt Tensile Stress Fnt (MPa)**

| Grade | Fnt |
|-------|-----|
| A325  | 620 |
| A490  | 780 |

**Bearing Strength** (simplified)
```
LRFD: phi*rn = 0.75 × 2.4 × d × t × Fu  [N per bolt]
ASD:  rn/Omega = (2.4 × d × t × Fu) / 2.00  [N per bolt]

Where:
d = bolt diameter (mm)
t = plate thickness (mm)
Fu = plate ultimate strength (MPa)
```

---

## 8. WELDED CONNECTIONS (CHAPTER J2)

### Fillet Weld Strength

**Design Strength**
```
LRFD: phi*Rn = 0.75 × Fnw × Awe  [N]
ASD:  Rn/Omega = (Fnw × Awe) / 2.00  [N]

Where:
Fnw = 0.60 × FEXX × (1.0 + 0.50 × sin^1.5(theta))  (MPa)
Awe = te × L (effective weld area, mm²)
te = 0.707 × w (effective throat, mm)
w = weld leg size (mm)
L = weld length (mm)
theta = load angle to weld axis (degrees)
```

**Simplified (theta = 90 degrees)**
```
Fnw = 0.90 × FEXX

For E70XX: Fnw = 0.90 × 485 = 437 MPa
For E80XX: Fnw = 0.90 × 550 = 495 MPa
```

**Electrode Strengths**
- E70XX: FEXX = 485 MPa
- E80XX: FEXX = 550 MPa

### Minimum Fillet Weld Size

| Plate Thickness (mm) | Min Weld Size (mm) |
|----------------------|---------------------|
| t <= 6               | 3                   |
| 6 < t <= 13          | 5                   |
| 13 < t <= 19         | 6                   |
| t > 19               | 8                   |

---

## 9. DESIGN EXAMPLE

### Problem
Design a W310x52 beam-column (A992 steel) for:
- Factored axial compression: Pu = 500 kN
- Factored moment: Mu = 150 kN·m
- Column length: L = 4000 mm
- Lateral bracing: Lb = 2000 mm

### Section Properties (W310x52)
```python
section = {
    'A': 6645,      # mm²
    'd': 317.5,     # mm
    'tf': 10.8,     # mm
    'tw': 6.6,      # mm
    'Sx': 671.8e3,  # mm³
    'Zx': 757.8e3,  # mm³
    'ry': 37.8,     # mm
    'rts': 45.0,    # mm
    'J': 158.8e3,   # mm⁴
    'ho': 306.7     # mm
}
```

### Solution

```python
# Compression check
comp = design_compression_member(
    Fy=345, Ag=6645, r=37.8, K=1.0, L=4000, 
    Pu=500e3, method='LRFD'
)

# Flexure check
flex = design_flexural_member(
    Fy=345, Zx=757.8e3, Sx=671.8e3, ry=37.8, 
    rts=45.0, J=158.8e3, ho=306.7, Lb=2000, 
    Cb=1.0, Mu=150e6, method='LRFD'
)

# Interaction check
interaction = check_interaction(
    Pr=500e3, Mrx=150e6, Mry=0,
    Pc=comp['Pc'], Mcx=flex['Mc'], Mcy=1e12,
    member_type='compression'
)

print(f"Compression: Util={comp['utilization']:.3f}, {comp['status']}")
print(f"Flexure: Util={flex['utilization']:.3f}, {flex['status']}")
print(f"Interaction: Ratio={interaction['ratio']:.3f}, {interaction['status']}")
```

---

## 10. QUICK REFERENCE

### Tension
```
Pn = min(0.90*Fy*Ag, 0.75*Fu*Ae)  [LRFD]
```

### Compression
```
Fe = pi²*E/(KL/r)²
If lambda_c <= 2.25: Fcr = 0.658^(Fy/Fe) × Fy
If lambda_c > 2.25: Fcr = 0.877*Fe
phi_c*Pn = 0.90*Fcr*Ag  [LRFD]
```

### Flexure (Compact, Lb <= Lp)
```
Mn = Mp = Fy*Zx
phi_b*Mn = 0.90*Mn  [LRFD]
```

### Shear
```
Vn = 0.6*Fy*Aw*Cv1
phi_v*Vn = 1.00*Vn  [LRFD]
```

---

## 11. REFERENCES

1. AISC 360-22: Specification for Structural Steel Buildings, 16th Edition
2. AISC Steel Construction Manual, 15th Edition, 2017
3. ASTM Standards: A36, A992, A572, A500, A325, A490

---

## Usage Guidelines

When using this skill, Claude should:

1. Identify the design task (tension, compression, flexure, shear, combined)
2. Extract parameters (material, section, loads, lengths)
3. Apply appropriate equations (LRFD or ASD)
4. Generate Python code using the provided functions
5. Check all limit states
6. Present results with utilization ratios and pass/fail status
7. Include units (N, mm, MPa, kN·m)
8. State assumptions (compact sections, doubly-symmetric, etc)
