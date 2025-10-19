# Structural Design of Foundations (EC2)

Structural design of foundation elements according to EN 1992-1-1 (Eurocode 2).

## Table of Contents

1. [Design Principles](#design-principles)
2. [Flexural Design](#flexural-design)
3. [Punching Shear](#punching-shear)
4. [Beam Shear](#beam-shear)
5. [Detailing Requirements](#detailing-requirements)

---

## Design Principles

### Limit State

Use **STR limit state** with appropriate partial factors:
- Concrete: γ_c = 1.5
- Steel: γ_s = 1.15
- Actions: Use Combination 1 (γ_G = 1.35, γ_Q = 1.5) for structural design

### Soil Pressure Distribution

**Rigid foundation assumption:**
- Uniform pressure distribution
- q_design = V_d / A

**Flexible foundation:**
- Non-uniform distribution (advanced analysis required)
- For most pad footings, assume rigid behavior

### Material Properties

**Concrete:**
```
f_cd = α_cc × f_ck / γ_c
```
where α_cc = 0.85 (typically)

For C30/37: f_ck = 30 MPa → f_cd = 0.85 × 30 / 1.5 = 17 MPa

**Steel:**
```
f_yd = f_yk / γ_s
```

For B500B: f_yk = 500 MPa → f_yd = 500 / 1.15 = 435 MPa

---

## Flexural Design

### Critical Section

Bending moment critical at **face of column** for pad footings.

### Cantilever Model

For square/rectangular pad with concentric load:

**Cantilever length:**
```
L_cant = (B - b_col) / 2
```

**Ultimate moment per meter width:**
```
M_Ed = q_design × L_cant² / 2
```

For rectangular foundation with different overhangs in each direction, calculate separately for each direction.

### Singly Reinforced Section Design

**Step 1: Calculate K**
```
K = M_Ed / (b × d² × f_cd)
```
where:
- b = width considered (typically 1000 mm per meter)
- d = effective depth
- f_cd = design concrete strength

**Limit for singly reinforced:**
K ≤ K_bal ≈ 0.167 (for f_ck ≤ 50 MPa)

If K > 0.167, compression reinforcement or increased depth required.

**Step 2: Calculate lever arm**
```
z = d × [0.5 + √(0.25 - K/1.134)]
```

**Maximum lever arm:**
```
z ≤ 0.95d
```

**Step 3: Required steel area**
```
A_s = M_Ed / (f_yd × z)
```

### Minimum Reinforcement

```
A_s,min = 0.26 × (f_ctm / f_yk) × b × d
```

but not less than:
```
A_s,min = 0.0013 × b × d
```

**Mean tensile strength f_ctm:**
- C25/30: 2.6 MPa
- C30/37: 2.9 MPa
- C35/45: 3.2 MPa

### Maximum Reinforcement

```
A_s,max = 0.04 × A_c
```
where A_c = gross concrete cross-sectional area

---

## Punching Shear

### Critical Perimeter

Located at distance **d** (effective depth) from loaded area perimeter.

**For rectangular column:**
```
u = 2(b_col + h_col) + 2πd
```

Simplified (conservative):
```
u = 4(b_col + 2d)  [for square column]
```

### Design Punching Shear Force

```
V_Ed = V_d - ΔV_Rd
```

where ΔV_Rd = soil reaction within critical perimeter:
```
ΔV_Rd = q_design × A_loaded
A_loaded = (b_col + 2d) × (h_col + 2d)  [rectangular]
```

### Punching Shear Stress

```
v_Ed = β × V_Ed / (u × d)
```

**Load distribution factor β:**
For concentric loading: β = 1.0
For eccentric loading, β accounts for moment transfer (see EC2 cl. 6.4.3)

### Punching Shear Resistance (No Shear Reinforcement)

```
v_Rd,c = C_Rd,c × k × (100 × ρ_l × f_ck)^(1/3) ≥ v_min
```

**Parameters:**

```
C_Rd,c = 0.18 / γ_c = 0.18 / 1.5 = 0.12

k = 1 + √(200/d) ≤ 2.0
```
where d in mm

```
ρ_l = √(ρ_ly × ρ_lz) ≤ 0.02
```
where ρ_ly, ρ_lz = reinforcement ratios in y and z directions

```
v_min = 0.035 × k^(3/2) × f_ck^(1/2)
```

**Use:**
```
v_Rd,c = max(calculated value, v_min)
```

### Verification

```
v_Ed ≤ v_Rd,c
```

If not satisfied:
1. Increase effective depth d
2. Provide punching shear reinforcement
3. Increase column dimensions

### Maximum Punching Shear Stress

At column perimeter (u_0):
```
v_Ed,max = β × V_Ed / (u_0 × d)
```

Must satisfy:
```
v_Ed,max ≤ 0.5 × ν × f_cd
```

where:
```
ν = 0.6 × (1 - f_ck/250)
```

---

## Beam Shear

### Critical Section

Located at distance **d** from face of loaded area.

### Design Shear Force

For one-way shear (strip foundation or edge of pad):
```
V_Ed = q_design × L_shear × b
```

where L_shear = distance from critical section to foundation edge

### Shear Stress

```
v_Ed = V_Ed / (b × d)
```

### Shear Resistance (No Shear Reinforcement)

```
v_Rd,c = [C_Rd,c × k × (100 × ρ_l × f_ck)^(1/3)] ≥ v_min
```

Same formulas as punching shear, but:
```
ρ_l = A_sl / (b × d) ≤ 0.02
```
where A_sl = area of tensile reinforcement anchored beyond section

### Verification

```
v_Ed ≤ v_Rd,c
```

If not satisfied, increase depth or provide shear reinforcement.

---

## Detailing Requirements

### Concrete Cover

**Exposure classes (typical):**
- XC1 (dry): c_min = 15 mm
- XC2 (wet, rarely dry): c_min = 25 mm
- XC3 (moderate humidity): c_min = 25 mm
- XC4 (cyclic wet/dry): c_min = 30 mm

**Nominal cover:**
```
c_nom = c_min + Δc_dev
```
where Δc_dev = 10 mm (typical tolerance)

**Typical values:**
- XC2: c_nom = 25 + 10 = 35 mm (use 40-50 mm)
- XC3/XC4: c_nom = 40-50 mm

### Bar Spacing

**Minimum spacing:**
```
s_min = max(bar_diameter, aggregate_size + 5mm, 20mm)
```

**Maximum spacing (crack control):**
For principal reinforcement: 3h ≤ 400 mm
For secondary reinforcement: 3.5h ≤ 450 mm

### Anchorage Length

**Basic required anchorage length:**
```
l_b,rqd = (φ/4) × (f_yd / f_bd)
```

**Design bond stress:**
```
f_bd = 2.25 × η_1 × η_2 × f_ctd
```

where:
- η_1 = 1.0 (good bond) or 0.7 (poor bond)
- η_2 = 1.0 (φ ≤ 32mm)
- f_ctd = α_ct × f_ctk,0.05 / γ_c

**Design anchorage length:**
```
l_bd = α × l_b,rqd ≥ l_b,min
```

**Minimum anchorage:**
```
l_b,min = max(0.3 × l_b,rqd, 10φ, 100mm)
```

**Typical values for B500B in C30/37:**
- φ12: l_bd ≈ 350-450 mm
- φ16: l_bd ≈ 500-600 mm
- φ20: l_bd ≈ 600-750 mm
- φ25: l_bd ≈ 750-950 mm

### Lap Lengths

```
l_0 = α_1 × α_2 × α_3 × α_5 × α_6 × l_b,rqd ≥ l_0,min
```

**Minimum lap length:**
```
l_0,min = max(0.3 × α_6 × l_b,rqd, 15φ, 200mm)
```

Typically: l_0 ≈ 1.4 × l_bd for most cases

### Top and Bottom Reinforcement

**Bottom (main) reinforcement:**
- Based on flexural design
- Extends to foundation edges
- Proper anchorage beyond critical sections

**Top (distribution) reinforcement:**
- Minimum 20% of bottom reinforcement
- Controls shrinkage and temperature cracks
- Typical: 0.0013 × b × h minimum

### Edge Reinforcement

Provide nominal edge bars where foundation cantilever exceeds 1.0 m to control edge cracking.

---

## Design Summary Checklist

**Flexural design:**
- [ ] Critical section at column face
- [ ] K ≤ 0.167 (no compression steel needed)
- [ ] A_s ≥ A_s,min
- [ ] A_s ≤ A_s,max

**Punching shear:**
- [ ] Critical perimeter at d from column
- [ ] v_Ed ≤ v_Rd,c
- [ ] v_Ed,max at column face acceptable
- [ ] If failed, increase d or add shear reinforcement

**Beam shear:**
- [ ] Critical section at d from column face
- [ ] v_Ed ≤ v_Rd,c

**Detailing:**
- [ ] Adequate cover for exposure class
- [ ] Bar spacing within limits
- [ ] Anchorage lengths satisfied
- [ ] Top reinforcement provided
- [ ] Edge bars if required

---

## Quick Design Steps

1. Assume h (total depth)
2. Calculate d = h - cover - φ/2 - φ_top/2
3. Check punching shear → adjust d if needed
4. Design flexural reinforcement
5. Check beam shear
6. Detail bars with proper cover and anchorage
7. Verify all minimum requirements
