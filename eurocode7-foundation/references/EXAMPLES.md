# Worked Examples

Common foundation design scenarios with complete calculations.

## Table of Contents

1. [Example 1: Axial Load, Dense Sand (DA1)](#example-1-axial-load-dense-sand)
2. [Example 2: Eccentric Load, Medium Clay (DA2)](#example-2-eccentric-load-medium-clay)
3. [Example 3: Inclined Load, Cohesionless Soil (DA3)](#example-3-inclined-load-cohesionless-soil)

---

## Example 1: Axial Load, Dense Sand

### Problem Statement

Design a square pad foundation for:
- Column: 400×400 mm
- Dead load: G_k = 1,500 kN
- Live load: Q_k = 500 kN
- Soil: Dense sand, φ'_k = 36°, c'_k = 0, γ = 19 kN/m³
- Water table: 8 m below GL (no effect)
- Embedment: D = 1.2 m
- Design Approach: DA1 (UK)
- Deformation modulus: E' = 40 MPa

### Solution

#### Step 1: Preliminary Sizing

Total characteristic load: V_k = 1,500 + 500 = 2,000 kN

Assume allowable pressure ≈ 400 kPa:
```
A_req = 2,000 / 400 = 5.0 m²
B_trial = √5.0 = 2.24 m → Try B = 2.5 m
```

#### Step 2: Design Actions

**Combination 1 (A1+M1+R1):**
```
V_d = 1.35(1,500) + 1.5(500) = 2,025 + 750 = 2,775 kN
```

**Combination 2 (A2+M2+R1):**
```
V_d = 1.0(1,500) + 1.3(500) = 1,500 + 650 = 2,150 kN
```

#### Step 3: Bearing Capacity - Combination 1

**Soil parameters (M1):**
```
φ'_d = 36°, c'_d = 0
```

**Effective dimensions (axial load, e = 0):**
```
B' = L' = 2.5 m
A' = 6.25 m²
q_d = 2,775 / 6.25 = 444 kPa
```

**Bearing capacity factors:**
```
N_q = exp(π·tan36°)·tan²(45°+18°) = exp(2.285)·tan²(63°) = 9.85·3.85 = 37.75
N_c = (37.75-1)·cot(36°) = 36.75·1.376 = 50.57
N_γ = 2(37.75-1)·tan(36°) = 73.50·0.727 = 53.44
```

**Shape factors (square):**
```
s_q = 1 + sin(36°) = 1.588
s_c = (1.588·37.75-1)/(37.75-1) = 1.604
s_γ = 0.7
```

**Depth factors (D/B = 1.2/2.5 = 0.48):**
```
d_q = 1 + 2·tan(36°)·(1-sin36°)²·0.48 = 1 + 2·0.727·(0.412)²·0.48 = 1.056
d_c = 1.056 - (1-1.056)/(50.57·tan36°) = 1.056 + 0.056/36.76 = 1.058
d_γ = 1.0
```

**No horizontal load:**
```
i_q = i_c = i_γ = 1.0
```

**Level base and ground:**
```
b = g = 1.0
```

**Overburden:**
```
q' = 19·1.2 = 22.8 kPa
```

**Ultimate bearing capacity:**
```
q_ult = 0 + 22.8·37.75·1.588·1.056·1.0·1.0·1.0
      + 0.5·19·2.5·53.44·0.7·1.0·1.0·1.0·1.0
      = 0 + 1,440.5 + 882.4 = 2,322.9 kPa
```

**Design resistance (γ_R;v = 1.0):**
```
R_d = 2,322.9·6.25/1.0 = 14,518 kN
```

**Verification:**
```
V_d = 2,775 kN ≤ R_d = 14,518 kN ✓ OK (19% utilization)
```

#### Step 4: Bearing Capacity - Combination 2

**Soil parameters (M2):**
```
tan(φ'_d) = tan(36°)/1.25 = 0.727/1.25 = 0.582
φ'_d = 30.2°
c'_d = 0
```

**Design load:**
```
q_d = 2,150 / 6.25 = 344 kPa
```

**Bearing capacity factors:**
```
N_q = exp(π·tan30.2°)·tan²(60.1°) = exp(1.827)·3.041 = 18.86
N_c = (18.86-1)·cot(30.2°) = 17.86·1.718 = 30.68
N_γ = 2(18.86-1)·tan(30.2°) = 35.72·0.582 = 20.79
```

**Shape factors:**
```
s_q = 1 + sin(30.2°) = 1.503
s_c = (1.503·18.86-1)/(18.86-1) = 1.531
s_γ = 0.7
```

**Depth factors:**
```
d_q = 1 + 2·tan(30.2°)·(1-sin30.2°)²·0.48 = 1.029
d_c = 1.031
d_γ = 1.0
```

**Ultimate bearing capacity:**
```
q_ult = 0 + 22.8·18.86·1.503·1.029
      + 0.5·19·2.5·20.79·0.7
      = 0 + 665.8 + 345.1 = 1,010.9 kPa
```

**Design resistance:**
```
R_d = 1,010.9·6.25 = 6,318 kN
```

**Verification:**
```
V_d = 2,150 kN ≤ R_d = 6,318 kN ✓ OK (34% utilization)
```

**Combination 2 governs** (higher utilization)

#### Step 5: Settlement

**SLS bearing pressure:**
```
q_SLS = 2,000 / 6.25 = 320 kPa
q_net = 320 - 22.8 = 297.2 kPa
```

**Immediate settlement:**
```
ρ_i = 297.2·2.5·(1-0.3²)/(40,000)·1.12
    = 297.2·2.5·0.91/40,000·1.12
    = 676.4/40,000·1.12
    = 0.0169·1.12 = 18.9 mm ✓ OK
```

#### Step 6: Structural Design

Assume h = 600 mm, cover = 50 mm, φ25 bars:
```
d = 600 - 50 - 12.5 - 10 = 527.5 mm
```

**Design pressure (Combination 1):**
```
q_design = 2,775 / 6.25 = 444 kPa
```

**Bending moment:**
```
L_cant = (2,500 - 400)/2 = 1,050 mm = 1.05 m
M_Ed = 444·1.05²/2 = 244.8 kNm/m
```

**Required steel:**
```
K = 244.8×10⁶/(1,000·527.5²·17) = 0.052
z = 527.5[0.5+√(0.25-0.052/1.134)] = 495.5 mm
A_s = 244.8×10⁶/(435·495.5) = 1,136 mm²/m
```

**Provide φ16 @ 175 mm c/c:**
```
A_s,prov = 1,000·201/175 = 1,149 mm²/m ✓ OK
```

**Punching shear:**
```
u = 4(400 + 2·527.5) = 5,980 mm
A_loaded = 1.455² = 2.117 m²
V_Ed = 2,775 - 444·2.117 = 1,835 kN
v_Ed = 1,835×10³/(5,980·527.5) = 0.582 MPa

k = 1 + √(200/527.5) = 1.616 (use 1.616)
ρ_l = 1,149/(1,000·527.5) = 0.0022
v_Rd,c = 0.12·1.616·(100·0.0022·30)^(1/3) = 0.559 MPa

v_Ed = 0.582 > v_Rd,c = 0.559 ✗ INCREASE DEPTH
```

Revise to h = 650 mm, d = 577.5 mm:
```
u = 6,220 mm
V_Ed = 1,704 kN
v_Ed = 0.474 MPa
v_Rd,c = 0.12·1.588·(6.6)^(1/3) = 0.596 MPa ✓ OK
```

### Final Design

- **Foundation:** 2.5 m × 2.5 m × 0.65 m thick
- **Embedment:** 1.2 m
- **Bottom reinforcement:** φ16 @ 175 mm c/c both ways
- **Top reinforcement:** φ12 @ 200 mm c/c both ways
- **Concrete:** C30/37
- **Steel:** B500B

---

## Example 2: Eccentric Load, Medium Clay

### Problem Statement

Design rectangular foundation for:
- Column: 500×500 mm
- Loads: G_k = 2,000 kN, Q_k = 800 kN
- Moment: M_k = 150 kNm (about minor axis)
- Soil: Medium clay, cu_k = 100 kPa, γ = 18 kN/m³
- Design Approach: DA2 (Germany)
- E' = 15 MPa (for settlement)

### Solution Outline

#### Design Actions (A1)
```
V_d = 1.35(2,000) + 1.5(800) = 3,900 kN
M_d = 1.35(150) = 202.5 kNm
```

#### Eccentricity
```
e = M_d/V_d = 202.5/3,900 = 0.052 m = 52 mm
```

Try B = 3.0 m, L = 3.5 m:
```
B' = 3.0 - 2(0.052) = 2.896 m
L' = 3.5 m
A' = 10.14 m²
```

#### Undrained Analysis (M1)
```
cu_d = 100 kPa (γ_cu = 1.0)
N_c = 5.14
s_c = 1 + 0.2(2.896/3.5) = 1.166
d_c = 1 + 0.4(1.2/2.896) = 1.166
m = (2 + 2.896/3.5)/(1 + 2.896/3.5) = 1.47
i_c = 0.5[1 + √(1-0)] = 1.0 (no horizontal load)

q_ult = 5.14·100·1.166·1.166·1.0 + 21.6 = 699.2 kPa
```

#### Apply Resistance Factor (R2)
```
R_d = 699.2·10.14/1.4 = 5,058 kN
V_d = 3,900 kN ≤ 5,058 kN ✓ OK (77% utilization)
```

---

## Example 3: Inclined Load, Cohesionless Soil

### Problem Statement

Foundation subject to:
- V_k = 1,000 kN
- H_k = 150 kN (horizontal)
- Soil: φ'_k = 32°, c' = 0, γ = 19 kN/m³
- Design Approach: DA3 (France)

### Key Points

#### Design Parameters (M2)
```
φ'_d = arctan(tan32°/1.25) = 26.8°
```

#### Inclination Factors Critical
```
For c' = 0:
m = (2+B'/L')/(1+B'/L')
i_q = (1 - H/V)^m
i_γ = (1 - H/V)^(m+1)
```

These factors significantly reduce bearing capacity when H is substantial.

#### Design Strategy
1. Size foundation for vertical capacity first
2. Check with inclination factors
3. Likely need larger foundation or reduced horizontal load
4. May need passive pressure contribution

---

## Common Patterns

### Pattern 1: Standard Axial Load (DA1)
- Both combinations required
- Combination 2 typically governs (lower resistance)
- Settlement in sand usually acceptable

### Pattern 2: Eccentric Load
- Effective dimensions B', L' critical
- Check bearing pressure doesn't exceed q_ult at toe
- May need to increase foundation size significantly

### Pattern 3: Horizontal Load
- Inclination factors drastically reduce capacity
- Check sliding separately
- Consider passive pressure
- May need shear keys

### Pattern 4: Undrained Clay
- Short-term stability critical
- Settlement may take years
- Check long-term drained capacity also
- Monitor settlement over time
